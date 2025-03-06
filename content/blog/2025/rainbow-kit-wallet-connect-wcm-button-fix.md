+++
date = '2025-03-06T13:38:45-08:00'
draft = false
title = 'Fixing: WalletConnect bug - the name "wcm-button" has already been used with this registry'
description = "Why and how to create a blog using Hugo and hosting it for free on Github Pages."
+++

RainbowKit is a beautifully designed library that enables developers to support multiple Web3 wallets effortlessly. It's highly customizable, plug-and-play, and eliminates the headache of setting up a smooth wallet connection UX. Since it uses Wagmi connectors under the hood, you could technically achieve the same functionality by using Wagmi directly. However, for Dojo and Ideacoin, we chose RainbowKit to save time. I highly recommend it to anyone building dApps.

### WalletConnect is Web3 wallet's Internet Explorer
One of my biggest struggles with dApp development, in regards to wallet management, has always been WalletConnect. No matter which dApp I've worked on, this wallet consistently causes the most issues. I'm not entirely sure why, but the multiple versions, different phone compatibilities, and recurring user complaints make it a nightmare. I hate to say this, but WalletConnect reminds me of the Internet Explorer of Web3 wallets.

RainbowKit and Wagmi have tried to simplify WalletConnect integration, but new problems continue to surface.

To top it all off, WalletConnect now requires a project ID, adding yet another layer of friction—one that no other provider demands. Whatever though, not a huge deal.

What is a huge deal is all of the errors I started getting because of integrating the wallet. Every now and then my project would explode with:

`NotSupportedError: Failed to execute 'define' on 'CustomElementRegistry': the name "wcm-button" has already been used with this registry`

### What I was trying to do
One of RainbowKit's features is that it supplies you with an easy to use `ConnectButton`. You can also customize it and make your own. 

Creating a custom button is what I decided to do, so I had more flexibility in the look and feel of the site. Ultimately, I wanted to place a variety of different looking buttons on the page when the user was not connected to their wallet. 

For me, I needed buttons in both the navigation bar and the swap panel to allow users to connect easily.

I develop most of my dApps using Next.js. If you've used Next.js, you're probably familiar with the anxiety-inducing red error button that appears in the bottom left corner.

While developing Ideacoin, I kept seeing hydration and registry errors related to `wcm-button`. 

After some investigation, I traced the issue back to, unsurprisingly: WalletConnect.

### Finding the root cause of the issue

The issue is, even though you're not rendering WalletConnect's modal directly, `wcm-button` is still being invoked/registered somewhere in the RainbowKit pipeline.

After some digging, I discovered that you can only have one `wcm-button` registered on your page at any given time.

I'm using RainbowKit/Wagmi specifically to avoid dealing with WalletConnect's complexities, but now I'm being forced to worry about this leaky abstraction anyway.

### What even is `wcm-button`?
I searched GitHub for answers:
https://github.com/search?q=org%3AWalletConnect%20wcm-button&type=code
Unfortunately, nothing helpful came up.

The only relevant discussion I found was this:
https://github.com/orgs/WalletConnect/discussions/2966

It mentions "double initializing WalletConnect/modal." WTF? I don't even use WalletConnect directly in my code... The issue seems to originate inside RainbowKit/Wagmi.

While poking around and debugging: I tried several approaches to resolve it.

### Fix #1: Using dynamic imports
The first attempted fix was to import the custom button as a dynamic component:

```typescript
const RainbowConnectButton = dynamic(
  () =>
    import('@rainbow-me/rainbowkit').then((mod) => mod.ConnectButton.Custom),
  { ssr: false },
)

export const ConnectButton = ({
  connectedButton,
  connectButton,
  wrongNetworkButton,
}: ConnectWalletButtonProps) => {
  return (
    <RainbowConnectButton>
      {({
        account,
        chain,
        openAccountModal,
        openChainModal,
        openConnectModal,
        mounted,
      }) => {
        // ...
      }}
    </RainbowConnectButton>
```

This prevents the component from loading on the server, which is critical for Web3 apps since they rarely need SSR (Server-Side Rendering). While this helped reduce errors, it didn't completely solve the `wcm-button` issue. I'd think the problem was fixed, prematurely celebrate, but after enough clicking around -- I'd eventually get the same error again. Shit.

### Further review and the next fix: Custom ConnectButton
In my custom button, I have a prop to pass in a `connectedButton` ReactNode. If no `connectedButton` was supplied, I defaulted to using RainbowKit's. This was my second problem. Since I was importing both the `ConnectButton.Custom` AND the default RainbowKit `ConnectButton`, I was getting the registry error.

If you're attempting to build a custom connect button, ensure that it's entirely custom throughout:
```typescript
    import { ConnectButton as DefaultRainbowConnectButton } from '@rainbow-me/rainbowkit';

    //.... custom component code

    if (connectedButton) {
        return connectedButton({
            account,
            chain,
            openAccountModal,
            openChainModal,
        })
    }

    // BAD!
    <DefaultRainbowConnectButton />
```

### The Real Issue: Multiple `wcm-button`'s can't rendering simultaneously
The problem with `wcm-button` is that it doesn't like multiple instances being rendered at the same time. When I was setting up a custom button and defaulting to RainbowKit's connect button, I was creating two registered `wcm-button`s with the same name. I looked into the registry to understand what was happening, but since I had no control over when the `wcm-button` was being spawned or how it was registered, my options were limited.

I wish there was a cleaner fix (outside of not using WalletConnect at all) but this is the best I found. I'd love to hear if you have solved in another way.