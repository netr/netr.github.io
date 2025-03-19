+++
date = '2024-11-04T20:51:45-08:00'
draft = false
title = 'Portfolio'
description = "A collection of all my professional and personal projects."
+++

I've been programming since I was 13 years old. I learned how to program by hanging out in [AOL chatrooms](https://github.com/readme/featured/aol-programming-culture). My first real adventure into building full programs was reverse engineering the AOL 5.0 protocol (I effectively rebuilt AOL using raw sockets). I fell in love with programming right away and have been doing it almost daily ever since.

I've worked on countless projects and I've touched almost every type of technology/framework that has been popular in the last 20 years. Here is a list of my most recent projects (within the last 5 years), since it would be far too time consuming to list them all. Plus I'm sure you don't care about my Cities: Skylines mods or my GTA 5 server scripts (unless you do, then [let me know](mailto:corey@netr.dev)).

## *Professional Projects*

### Muse Markets | [Website](https://muse.markets)
*February 2025 - Present*
- **Backend:** Node.js
- **Frontend:** React, TypeScript, Next.js, Tailwind CSS, Shadcn, Server-Sent Events (SSE)
- **Cloud:** AWS (S3, SQS, Lambda, API Gateway), Vercel
- **Infrastructure:** Docker, Nginx, Terraform, GitHub Actions
- **Databases:** MongoDB, Redis
- **Blockchain:** TheGraph (Subgraph), Viem, RainbowKit
- **Testing:** Vitest, React Testing Library
- **Monitoring:** Sentry

TBD

### Dojo.ing: Memecoin Launchpad | [Website](https://dojo.ing)
*October 2024 - Present*
- **Backend:** Node.js, Express.js
- **Frontend:** React, TypeScript, Next.js, Tailwind CSS, Shadcn, Server-Sent Events (SSE)
- **Cloud:** AWS (S3, SQS, Lambda, API Gateway), Vercel
- **Infrastructure:** Docker, Nginx, Terraform, GitHub Actions
- **Databases:** MongoDB, Redis
- **Blockchain:** TheGraph (Subgraph), Viem, RainbowKit
- **Testing:** Jest, React Testing Library
- **Monitoring:** Sentry

Led the development of a high-performance Web3 platform integrating real-time blockchain data with a modern user interface. The system achieved 80-90% improvement in response times through strategic Redis caching and reduced blockchain query costs by 30-40%. Outside of sending transactions to the blockchain, Subgraph gave the system the ability to handle 100% of the presentation layer without needing to directly query the blockchain. Huge win.

After thorough investigation, this was the first time that I implemented Server-Sent Events (SSE) in a production environment. I absolutely love them. Given their unidirectional design, they were a perfect match for what we needed. I was able to add real-time updates to the platform without having to worry about the complexity of WebSockets.

### Optimistiq: Peer-to-Peer Texting Platform | [Website](https://optq.com)
*June 2024 - November 2024*
- **Backend**: Golang
- **Frontend**: React, TypeScript, Tailwind CSS
- **Cloud**: AWS (Lambda, SQS, API Gateway, RDS)
- **Infrastructure**: Terraform, Docker
- **Databases**: PostgreSQL, Redis
- **Testing**: Testify (Golang), Jest + Cypress (React)
- **Monitoring:** Sentry

Designed and implemented a high-throughput SMS campaign management platform handling 600,000+ messages daily per client. The system provides comprehensive campaign management, real-time analytics, and automated list processing capabilities through a modern React/TypeScript interface.

This was the first major project I developed after the LLM and Copilot craze. It got to roughly ~100K LOC in three months. I developed it entirely by myself and was able to maintain a very high standard, while moving fast. It showed me the real power of AI in development and where the industry is headed. The technology is not perfect (far from it), but for small concentrated teams, it's a game changer. 

### Ease.org: DeFi Coverage | [Website](https://ease.org)
*April 2021 - July 2023*
- **Backend:** Golang
- **Frontend:** React, TypeScript, Next.js, Tailwind CSS
- **Cloud:** DigitalOcean, Vercel
- **Databases:** Postgres, Redis
- **Blockchain:** React.js, Web3React, Solidity
- **Testing:** Jest, React Testing Library

I was the lead developer on this project. Web3 applications are a whole different beast. Web3 applications are extremely hard to test and the documentation is inconsistent. We still needed to use Web3React in this project. I'm deeply grateful for the work that has been put into that library, but it was still a bit messy to work with. Since I had a lot of experience with Golang, Ethereum (from bot building), and Solidity, I was able to manage the project well. I'm not sure if I want to keep working in the Web3 space, but it was a great learning experience. The tools are definitely getting better, but the ecosystem is still developing standards.

### Ally MS | [Website](https://app.allyms.com/)
*April 2020 - April 2021*
- **Backend:** Laravel/PHP, Laravel Echo
- **Frontend:** Vue.js, Bootstrap, React Native
- **Databases:** MySQL, Redis
- **Testing:** PHPUnit
- **Monitoring:** Sentry

I got the opportunity to work for this company where my buddy was the project manager. It was the first time that I got to see Laravel in production after using it for a few years, only on personal projects. I was mainly working on the backend, but I did get to work on the frontend a bit. Near the end of my time there, I took it upon myself to learn React Native to finish a mobile app that was under development by a third party. Following months of delays, I was able to finish the app in a few weeks. 

After that, I was tasked with creating an interactive dashboard for the app. The MySQL databases were so convoluted that trying to generate reports in real time was practically impossible. I ended up creating a system that would generate reports in the background and then serve them to the dashboard. I spent hours running queries and optimizing the database to get the reports to generate in a reasonable amount of time. I wish I could always get tasked with projects like that. It was a great problem to solve.

## *Personal Projects*

### Haki: AI Powered Anki Flashcard Generator
*September 2024*

- **Backend**: Golang
- **Tools**: OpenAI, Anthropic API

Personal project to generate Anki flashcards for vocabulary words and variety of topics. Powered by OpenAI and Anthropic APIs. The system uses AI to select which deck to place the card in, generates images, text-to-speech, and a variety of different card types. Dramatically improved the amount of time spent on creating flashcards so I can create more and learn faster.

### MarcoPolo: Cryptocurrency Trading Bot
*August 2021 - Present*
- **Backend**: Golang
- **Frontend**: React, TypeScript, Tailwind CSS, Websockets
- **Cloud**: Vercel, AWS EC2
- **Databases**: PostgreSQL, Redis
- **Testing**: Jest + Cypress (React), Testify (Golang)
- **Monitoring:** Sentry

Fully managed trading bot that uses Binance and Coinbase Pro APIs to execute trades based on a variety of strategies. The system is designed to be fully automated, but also allows for manual trading. I have always been way too much of an emotional trader and I created this system to remove myself from the equation. When i'm thinking sensibly, I can plan out all of my trades and let the system execute them for me. It's made me some decent money over the years, but mainly, it's just fun to build automated systems.

### AI Showcase: AI Tool Database
*May 2023 - May 2024*
- **Backend**: Laravel/PHP
- **Frontend**: React, TypeScript, Tailwind CSS
- **Cloud**: Laravel Forge, Vercel
- **Databases**: PostgreSQL, Redis
- **Testing**: Jest + Cypress (React)
- **Monitoring:** Sentry

Aggregated a comprehensive database of AI tools and resources, enabling users to discover, compare, and rate AI tools across various categories. Developed a hybrid (vector embeddings and meilisearch) RAG system, for a OpenAI GPT plugin, to provide users with relevant information based on their queries. Made heavy use of OpenAI's API to generate tool descriptions and summaries based on the content of the tool's website. 

### MemeRS: Memecoin Monitoring Tool | [Repo](https://github.com/netr/memers)
*October 2023*
- **Backend:** Rust

I created this small project to get more familiar with Rust. Meme coins were getting popular and I wanted to see if I could create a tool to monitor them. The system was tracking when liquidity was added or removed, whether LP tokens were locked, and how new pairs were created. Was a very interesting insight into how many rug pulls, repetitive scams (same person or team re-launching new coins over and over), and near instant liquidity drains were happening. 

### OpenSea NFT Sniper and Monitoring Bot
*January 2022 - June 2022*
- **Backend**: Golang
- **Frontend**: React, TypeScript, Tailwind CSS, Websockets
- **Cloud**: Vercel, AWS EC2
- **Databases**: PostgreSQL, Redis

This is probably the most fun personal project I've ever worked on. I created a bot that would monitor OpenSea for high quality NFTs (based on ranking algorithms) and would buy them if they were under a certain price. Since OpenSea had very strict API limits, I decided to approach the problem from the blockchain instead. As long as I knew the collections floor price (limited API was OK here), I could use the rank of the NFTs to decide the resell value. It leveraged Ethereum's mempool and gas mechanism to snipe NFTs before they were purchased by others. Unsuspecting users would find the deal for me  (mempool), I'd cross-reference the resell value in real-time and, if it was a great deal, snipe it (gas mechanism). It's a little messed up but, hey, that's public blockchains for you. I built an entire frontend for it, tracked my profits in real-time using websockets, it was a blast. Too bad NFTs were trash.