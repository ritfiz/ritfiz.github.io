---
date: '2025-05-28T12:02:48+05:30'
draft: true
title: 'Frontend System Design'
---

# Steps involved in Frontend System Design

A tried and tested framework that I have come to use pretty frequently is the RADIOS framework
1. Requirement gathering
    - Functional requirements
    - Non-functional requirements

2. Architecture
3. Data Model
4. Interface / API
5. Optimizations
    - User experience
    - Performance
    - Accessibility
    - Internationalization
    - Security
    - Observability
6. Scalability
    - Micro frontends
    - Design libraries Storybooks

## 1. Requirements

Understand the problem statement and ask clarifying questions. No question is dumb, and this is where we will be able to set a scope to where we need to dive deep into.

### Functional requirements

These can be all the features and functionalities that one can imagine. An exhaustive list. A few examples could be

- Do we need an input box here?
- Do we need three buttons?
- Does the list have pagination?
- Do we want infinite scrolling?

### Non - Functional requirements

These are features that are present in the app but not readily visible to the user. Examples may include

- Fast page performance
- Accessibility
- Responsiveness
- Internationalization
- Offline support
    - Web workers

## 2. Architecture

This typically involves drawing a lot of boxes and lines connecting them.

- Create a basic wireframe of the product that you are building so that writing APIs becomes easier later on
- Design the component architecture
- Discuss design patterns that we may use here
- Explain data flow
- Explain interactions

This is also a good place to talk about
- What rendering strategy are you planning?
- Are you using an orchestration layer?

### Rendering Strategies

**CSR** - Client-Side Rendering
Minimal HTML is fetched from the server; JavaScript in the browser fetches the data and displays it, making the page interactive. e.g., Dashboards, SPAs

**SSR** - Server-Side Rendering: The full HTML is created on the server and sent to the client, then hydration happens. Can be achieved with Next.js (`getServerSideProps`), Nuxt.js. e.g., e-commerce, Blogs

**SSG** - Static Site Generation: HTML is prebuilt on the server and served to the UI. Can be achieved with NextJS (`getStaticProps`), Gatsby, or Hugo (on which this site is made :D). e.g., Docs, Marketing

**ISR** - Incremental Static Regeneration. Good for pages that periodically update. e.g., Product pages

**Progressive Hydration / Partial Hydration** - Only updates parts of a page that are interactive. Can be achieved with Astro, Marko. e.g., Content sites with minimal activity

**ESR** - Edge-Side Rendering. Note that ESR is more a deployment target than an actual rendering mode. Cloudflare, Vercel Edge functions, etc., provide the infrastructure for the same.

**Streaming SSR** - Sends HTML in chunks as it’s generated (React 18 + Next.js). `renderToPipeableStream().`


> - Is SEO critical? → SSR, SSG, ISR.
> - Is data real-time? → SSR + WebSockets.
> - Static content? → SSG or ISR.
> - Global audience? → ESR.
> - Highly interactive? → CSR + Lazy Loading.

## 3. Data Model

Tell how each component's data is structured and what keys and type of data these structures will have. e.g.,

    App: {
        global: Object,
        settings: Object
    }

    User {
        name: string
        email: string
    }

Let's talk about where the client data is being stored:
- State
    - How is the state managed?
- LocalStorage
    - Size up to 5MB - 10MB
    - Only text
    - Synchronous
- IndexDB
    - Large size
    - Can store files and blobs
    - Asynchronous
    - Transactional

## 4. API design

Discuss what API protocol you would use here:
- HTTP/1.1
- HTTP/2
- GraphQL
- gRPC
- WebSockets

Listing out details of each with a few pros and cons of each:

HTTP/1.1

Legacy systems generally tend to be using this protocol; this creates synchronous blocking requests.

HTTP/2

Can take parallel requests, which was not possible in HTTP/1.1; has better header compression using the HPACK algorithm.

Polling
- Short polling - frequent intervals to check
- Long polling - open a connection and wait; if something new comes up, send that as a response.

GraphQL
- Pull the data we need
- Type safety because of schema
- May start taking more latency

WebSockets
- Bi-directional
- Can send binary data - so good for images, media, etc.
- Requires a special `ws://` protocol
- Requires separate load balancing

SSE
- Single directional
- Can only stream text
- Works over standard HTTP connection

gRPC

Even though this is mainly used for backend-to-backend connections, it's good to discuss it. Also, the browser support is very limited, but there are projects like `grpc-web` which are making gRPC accessible on browsers.

- Instead of the regular JSON, the data is sent across in a binary protocol (Protocol Buffers), resulting in smaller sizes and faster serialization/deserialization compared to text-based formats.
- Strongly typed contracts via `.proto` files
- Supports bi-directional streaming
- Language agnostic

## 4. Interface/API

What APIs will be needed, and what should the contracts for each API be?

## 5. Optimization

### Performance

- HTTP/2 for multiplexing
- Compression
    - Header compression
    - Brotli
    - Gzip

- Caching
    - If using GraphQL, we can use Apollo caching.
- Batch requests if possible
    - For example, analytics events, etc.
- Image optimization
    - Use WebP, compressed images
    - Use source sets for mobile and web
- Code splitting
    - Lazy load whatever JS chunk you need for a page.
- Rendering performance
    - Mobile-friendly
    - Show preloaders
    - Handle errors gracefully
    - Focus on above-the-fold content and load the rest of the page asynchronously.
    - Check Web Vitals
    - TTI -> FCP -> Get the p50, p75, or p90 data from any RUM (Real User Monitoring) tools, which helps you identify what percentile of users are getting TTI below and what are getting above. e.g., For p90 - 90th percentile of users are getting TTI below x value, and 10th percentile are getting TTI value above it.
    - Import only what is needed (e.g., Lodash).
    - Tree shaking - eliminate dead code
    - Virtualization when using long lists
    - CSS in JS only loads CSS that is used for the loaded component.
- Early hints
- Preload and Prefetch
- Pagination - Offset vs. Cursor-Based

### Accessibility

    - Contrast of text and background
    - Keyboard navigation
    - Semantic tags
    - `aria-roles`

### Security
>I'll add more details to this section soon

XSS - Cross-Site Scripting attacks.

CORS - Cross-Origin Request Sharing

CSP - Content Security Policy

CSRF tokens

### Observability

Use a platform to gather performance metrics. RUM tools like datadog, webpagetest etc can come in handy

## Scalability

- Micro-Frontends - Breaking down the app into MFEs can help multiple teams work on separate projects.
    - Build-time integration
        - Use separate packages as MFEs and include them in `package.json`.
    - Run-time integration
        - Iframes - generally avoided
        - Client-side composition using Module Federation from Webpack 5 or Single SPA
        - SSI is a server-side technique where fragments of HTML are stitched together at runtime (when a user requests the page).
        Imagine an HTML page containing code like below, which looks like normal commented HTML code, but on the web-server side like Nginx, it has a special meaning.

            ```
            <!--#include virtual="/api/header?user=123" -->
            ```
        - Import maps can also be a way to achieve MFEs architecture natively. Browsers support import maps to include ESM modules directly within the HTML of the page.
