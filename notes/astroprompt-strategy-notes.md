---
- Version: 1
- Date: 2026-04-21
- Author: Claude Chat
- Depends on: (original stream-of-consciousness notes)
---

# AstroPrompt Strategy Notes

## 1. The Core Problem: Product-Market Fit

I was thinking about the AstroPrompt product-market fit problem. AstroPrompt is doing too many things. I've gotten feedback from Divina that it is not clear what the product does. I already realized that there are several products embedded in AstroPrompt:

- A financial predictions product
- A news product
- A relationship helper
- An astrological planner

## 2. Onboarding and Training Challenges

A related problem is that effective use of AstroPrompt requires onboarding with a video call training session (from what I've seen with Judy and Divina). I think I've added help banners and an onboarding UI to guide first-time users, but it seems people are lazy or want more handholding — probably because the complexity of the app is intimidating.

Options for onboarding and training:

- A series of YouTube videos
- A free or paid course on a platform like Udemy
- A paid course as a potentially greater revenue stream (course fees are higher than app subscriptions)
- A tiered marketing plan: make course materials available to astrology influencers and teachers who are already teaching; they use the free materials to build and sell their own courses, and Timecasters makes money because their students adopt AstroPrompt and upgrade to premium subscriptions

## 3. Potential Focused Product Spin-Offs

Rather than one sprawling product, I could develop more focused products.

### "Bad Astrology" Simple iOS App
- Shows Mercury Retrograde and Moon VOC times
- Revenue model: upgrade to a premium "show personal Moon VOC times" tier
- Upsell path to a separate "Astrology Daily Planner" product

### Daily Planner as the Flagship
The Planner features, especially the Daily Planner, are high-retention sticky features that might be the best foundation for a focused app. A course and product focus could be: **"Use AI for Planning Your Life and Work or Business."**

## 4. Market Channels and Go-To-Market

### Channel A: Professional Astrologers (Tiered / Indirect)

In my thinking and interests I'm ahead of the astrologer market — very few astrologers are thinking about using AI as a tool for astrology. So far I've met Hollie McKintrick, who is excited and curious about AI and is trying to position herself as the go-to AI expert in the astrologers' community. I haven't seen any other YouTube videos or podcast episodes about using AI for astrology, other than Alisa Dixon interviewing Hollie McKintrick.

**UAC astrology conference in Chicago in September:**
- I'd meet many astrologers, many interested in AI and astrology
- Attending will be expensive, and I'm not sure how much product uptake for AstroPrompt (in its present form) I'd actually get
- It's a tiered/sequenced approach: sell to professionals, who then sell to their clients, students, or followers

**Limitations of the professional channel:**
- Not all UAC astrologers are influencers with followings
- Many do astrology just for themselves or a small client base
- Many astrology influencers may not attend UAC — they may not be part of the cliquish professional community, may not know about it, or can't afford to travel
- Professional astrologers tend to be gatekeepers, following the most influential or persuasive leaders in the community
- There are traps in the cliques and dynamics (e.g., my friend Dorothy Oja has bad blood with Ray Merriman)
- Professionals may resist AI for astrology because:
  - They've seen students taking shortcuts using AI for delineations (Dorothy's experience)
  - They fear AI will disrupt their roles and work
  - They've experienced the inaccuracy and unreliability of AI for astrology (which is what I developed AstroPrompt to address)

### Channel B: Direct-to-Consumer

A more direct path to less sophisticated or non-expert consumers with a casual interest in astrology.

- **Paid advertising:** Katy Bohinc's pitch deck for her data-science-backed astrology startup uses paid advertising for customer acquisition
- **Influencer-driven marketing:** reach out to podcasters, YouTubers, Instagrammers, and TikTokers

## 5. Professional Astrologers' Product Expectations

Professional astrologers are very demanding. They expect all the professional features: solar returns, progressions, choice of house systems, minor objects, and more. They identify strongly with:

- **Traditions:** Hellenistic, evolutionary, various Vedic schools, etc.
- **Lineages:** Liz Greene, Robert Hand, Steven Forrest, Dane Rudhyar, etc.

They measure each other's status by conformance to the standards and practices of the lineage holders (gatekeeping). The professionals have high demands about available tools — for example, Divina wanted to enter questions about progressions.

## 6. The Deeper Product Problem: Matching Questions to Data

With AstroPrompt it isn't always clear which view is the best match for a particular question, and some questions aren't supported by the datasets and views currently available. Examples:

- Hollie McKintrick wanted to ask "What are my lucky days this year?" or "What does my most recent Solar Return indicate for the year ahead?"
- Questions about five-year life plans require progressions, solar returns, and transits of outer planets
- Questions about the best time this week for a task require Moon VOC data

This is a big general problem, especially with non-expert users — they won't know what kind of astrological data is needed to answer common questions.

### Near-Term Product Response
From a product standpoint, AstroPrompt needs a **"Question Helper" view** that uses AI to parse and classify a question and suggest which astrological datasets (and AstroPrompt views) are needed to answer it.

## 7. The Bigger Architectural Question

This raises a bigger question: how can people get AI to answer questions and give advice that requires astrological calculations? It's not just performing calculations — it's knowing *which* calculations and data are appropriate for a given question.

AstroPrompt currently addresses this by pre-packaging views based on my own (limited) expert knowledge. I need to think more about what views/datasets users will expect (a product design problem). But the more general question is: if a non-expert user enters a request into an AI chat, how does the AI respond with an answer supported by appropriate astrological calculations?

Maybe I shouldn't be building a product like AstroPrompt that assumes the user knows which view to use for a given question (or job-to-be-done). Instead, I should think about the evolving future of AI tool use — where no one chooses a special-purpose tool like AstroPrompt or a GPT (which seems to be a failed product category). Instead, the user just asks any LLM any question that might be best answered by astrology (e.g., "What are my lucky days this week?"), and the AI knows how to answer.

## 8. Toward an MCP-Based Architecture

There needs to be a question classifier as part of the AI harness (I think that's how it works). Maybe there's an MCP server that functions as a question classifier — analyzing a question and suggesting the appropriate astrological calculations for a good answer.

### Open Questions on Resource Discovery and Monetization

- How does an AI platform currently know what MCP servers are available?
- How does it recognize that an MCP server or API is needed to provide access to a specialized dataset or calculation engine?
- Is there a standardized practice or algorithm for resource discovery?
- Is there a mechanism for monetization of access to a specialized dataset or calculation engine?
- Could I build an MCP server that functions as a question classifier for questions that can be answered by astrology? How would that work?
- How does the LLM or its harness recognize that a general question requires astrological calculations?
- Would I write a paper for arXiv, which then gets into training data so AI platforms learn the MCP server is available?
- Is this something AI agents are starting to do? (This assembly of tools is more than simple AI chat.)

### A Layered Architecture

I'm imagining something bigger than just building AstroPrompt and looking for product-market fit:

1. **A question classifier** (first layer) that points to other MCP servers or APIs
2. **Specialized MCP servers or APIs** (second layer) that respond to certain classes of questions — lucky days, relationships, best times — by performing astrological calculations and returning astrological datasets

## 9. Technical Infrastructure Options

**What exists now:** I've built an API that uses Astrolog as a subprocess to return datasets for a daily planner, weekly planner, etc.

**Possible next steps:**

- Build a dedicated **Astrolog MCP server** that exposes the raw Astrolog command set for someone who knows how to formulate a technical astrological request
- Build a higher-level MCP server or API that handles the general questions referred by the question classifier
- Move beyond Astrolog: I've already seen its API isn't well-suited to generating some datasets. Calculating Moon VOC periods requires many iterations and parsing, as does reporting on house ingresses for transiting planets
- Start building a server that uses **Swiss Ephemeris directly**, with algorithms matched to common classes of astrological questions

## 10. The Synthesis Question

What's the technological infrastructure that is needed, and what are the products and product-market GTM strategies that match users' jobs-to-be-done with that infrastructure?
