# Project Hivesong

Published Draft - Apr 2026

**A continuous sync pipeline that pumps tweets from Community Archive to Bluesky with structural fidelity.**

> *"A document is not necessarily a simulation of paper. In the most general sense, a document is a package of ideas created by human minds and addressed to human minds, intended for the furtherance of those ideas and those minds. Human ideas manifest as text, connections, diagrams and more: thus how to store them and present them is a crucial issue for civilization."*
> — Ted Nelson

Author: UltimApe / Nicholas Perry (@ultimape)

Status: Published Draft. Prototype work in progress; formal funding drive being set up.

Repository: [`github.com/ultimape/hivesong`](https://github.com/ultimape/hivesong) (currently private; will be public when initial scaffolding is in place)

> **Note**: This is a working draft for discussion with potential developer collaborators and funders. Drafted collaboratively with Claude (Anthropic): the requirements, vision, and architectural direction reflect UltimApe's intentions, but specific code snippets, API details, schema examples, and technical suggestions should be treated as illustrative rather than copy-paste-ready. Verify all technical claims against current documentation before implementation. Funding details (specific amounts, channels, timelines) will be finalized as the project moves forward; this document is not yet a published funding pitch.

---

## TL;DR

**What**: Hivesong is a tool that copies your Twitter archive to Bluesky with the threading, timestamps, quote-posts, and media intact. A "pump" that turns a flat export back into a navigable graph. Built in TypeScript on top of Community Archive (the public Twitter archive) and the AT Protocol.

**Why**: For people who use Twitter as a research notebook (a "memex"), the archive isn't just content; it's a cognitive prosthesis built up over a decade. Twitter's broken export and the disappearance of accounts is destroying that work. Hivesong preserves it on infrastructure the user controls.

**Who**: UltimApe (~150,000 tweets across a decade of public research) and a developer collaborator. Open from day one. The MIT-licensed core is what's being built; if the project goes well, an AGPL service layer may eventually be added to fund continued development.

**Ask**: A scoped grant or sponsorship to pay the developer collaborator for ~3-4 months of part-time work to ship the prototype. Multiple funding channels possible: open-source grants, direct sponsorship, individual backers, earmarked Patreon contributions.

---

## Table of Contents

> Sections are grouped by audience below; the document itself is in linear reading order. Anchors link to the original positions.

### For everyone (context, motivation, FAQ)

- [Why This Exists](#why-this-exists)
  - [Tweets as cognitive prosthesis](#tweets-as-cognitive-prosthesis)
  - [Trapped infrastructure](#trapped-infrastructure)
  - [What Hivesong does](#what-hivesong-does)
  - [UltimApe's archive](#ultimapes-archive)
  - [The urgency](#the-urgency)
  - [For everyone](#for-everyone)
  - [Why "Hivesong"?](#why-hivesong)
- [FAQ](#faq)

### For funders, sponsors, and backers (project shape, team, money, licensing)

- [The Project](#the-project)
  - [The team](#the-team)
  - [Funding model](#funding-model)
  - [Why this attempt is different](#why-this-attempt-is-different)
  - [Licensing and business model](#licensing-and-business-model)
  - [What backers get](#what-backers-get)

### For developers (technical spec)

- [1. Structural Requirements (What Must Survive)](#structural-requirements-what-must-survive)
  - [1.1 Thread Structure](#thread-structure)
  - [1.2 Timestamps as Epistemic Record](#timestamps-as-epistemic-record)
  - [1.3 Quote Posts and Embeds](#quote-posts-and-embeds)
  - [1.4 Cross-Platform References](#cross-platform-references)
  - [1.5 Media Attachments](#media-attachments)
  - [1.6 Link and Media Preservation (Anti-Rot)](#link-and-media-preservation-anti-rot)
- [2. Data Sources and Formats](#data-sources-and-formats)
  - [2.1 Community Archive](#community-archive-remote-api)
  - [2.2 Twitter Archive](#twitter-archive-local)
  - [2.3 Are.na](#arena-remote-api)
  - [2.4 Digital Garden](#digital-garden-static-site)
  - [2.5 Bluesky](#bluesky-target)
- [3. Architecture (Prototype Scope)](#architecture-prototype-scope)
  - [Tech Stack](#tech-stack)
  - [Success Criteria](#success-criteria)
  - [Milestones](#milestones)
  - [Getting Started](#getting-started)
  - [3.1 Core Concept: The Pump](#core-concept-the-pump)
  - [3.2 Phase 1: Fetch from Community Archive](#phase-1-fetch-from-community-archive)
  - [3.3 Phase 2: Dependency Check](#phase-2-dependency-check)
  - [3.4 Phase 3: Publish to Bluesky](#phase-3-publish-to-bluesky)
  - [3.5 Text and Facet Handling](#text-and-facet-handling)
  - [3.6 State Management and Resumability](#state-management-and-resumability)
  - [3.7 Sync Efficiency (Scaling Sub-Goal)](#sync-efficiency-scaling-sub-goal)
  - [3.8 Live Sync Mode](#live-sync-mode)
  - [3.9 Dry Run Mode](#dry-run-mode)
  - [3.10 Risks, Assumptions and Dependencies](#risks-assumptions-and-dependencies)
- [4. Future Directions](#future-directions)
  - [4.1 Self-Hosted PDS](#self-hosted-pds-core-goal)
  - [4.2 Bidirectional Sync](#bidirectional-sync)
  - [4.3 Multi-User Hosted Service](#multi-user-hosted-service)
  - [4.4 Cross-User Reference Remapping](#cross-user-reference-remapping)
- [5. Open Questions](#open-questions)

### For everyone (appendix)

- [References and Resources](#references-and-resources)
- [Further Reading](#further-reading)

---

## Why This Exists

### Tweets as cognitive prosthesis

Some people use Twitter like a chat room. Others use it as a notebook.

If you've spent years thinking in public, developing ideas across threads, linking posts to papers, quoting your own older work as evidence for newer claims, your tweet history isn't a social media feed. It's a research corpus. A memex. The timestamps are evidence of when you knew something. The threads are arguments that develop over months. The quote-tweets are cross-references that weave separate lines of thought into a single body of work.

This is true for independent researchers, open-source scientists, writers, theorists, and a surprising number of people in communities like tpot who do their most important thinking in 280-character increments.

More than that: for some people, these archives are cognitive prostheses. In medicine, a prosthesis replaces a missing body part. A memex replaces (or rather extends) a missing cognitive capacity: the ability to hold ten years of interconnected ideas in your head at once. One of the best ways to improve memory is by not having to remember stuff to begin with. The tweet archive is where thinking is externalized: not as finished exhibitions of knowledge, but as working notes in a living index. Dostoyevsky put it bluntly in *Notes From Underground*: "I do not want to hamper myself with anything in preparing my notes. I will not introduce any order or system. Whatever I recall, I will write down." Charles Darwin worked the same way: heavy annotation, personal indexes, breaking books in half if they were too heavy. Howard Gruber called it "a man at work using books as tools for getting knowledge, not as exhibitions of knowledge already crystallised." That's the relationship UltimApe has with his own tweets. Losing that archive isn't like losing a photo album. It's like losing a limb.

Alex Singh coined a term for this kind of thinker: the **Liminist**.

> *"The opposite of the specialist is not the generalist, but the Liminist. Where the generalist picks the low hanging fruit of knowledge, the Liminist operates in the liminal interzones between them. In a world of disconnected silos stretching vertically into the sky, the Liminist stretches horizontally across them. They function as guides, navigating people across these intersecting planes to comprehend the work and ideas of their unrelated peers. It is very much its own specialization, but where all other domains specialize in Form, the Liminist specializes in Non-Form. The evidence of their work exists at the intersections between objects."*
> — Alex Singh, [alexsingh.com](https://www.alexsingh.com/)

A specialist's archive is organized by domain. A Liminist's archive is organized by *intersection*. The connections between domains are the work product, not a byproduct. That's why a flat tweet export is more destructive for this kind of thinker than for anyone else. The threads and quote-tweets aren't navigational convenience; they *are* the evidence of liminal work.

### Trapped infrastructure

And that infrastructure is trapped on a platform they don't control.

Twitter's archive export is notoriously incomplete. It has been broken in various ways since it launched in 2012. Images go missing. Threads are flattened. Quote-tweets lose their context. The export gives you a pile of disconnected posts when what you actually had was a graph, a web of timestamped, interlinked ideas.

Projects like the Community Archive (community-archive.org) have made progress on the preservation side: you can upload your Twitter archive to a public database where it's queryable and searchable. But preservation isn't the same as liberation. Your data is saved, but it's still not *yours* in the way that matters: living on infrastructure you control, in a format with a real interface, on a federated protocol.

### What Hivesong does

**Project Hivesong bridges that gap.** It's a sync pipeline that takes your Twitter archive from Community Archive and pumps it into Bluesky, preserving thread structure, original timestamps, quote-post references, and media attachments. Because Bluesky is built on the AT Protocol, the result isn't just a mirror: it's a federated, self-hostable copy of your intellectual work that any Bluesky client can browse and search, that lives on a Personal Data Server you can run yourself, and that nobody can take away from you.

And it's not just about the platform disappearing. A memex rots from the inside too. Every outbound link in a tweet (to a paper, a blog post, a collaborator's thread) is a synapse. When those links die, the thought they supported loses its evidence. Hivesong includes link preservation as a core concern: unwrapping Twitter's `t.co` redirects before they break, archiving linked pages to the Internet Archive, and monitoring for rot over time. An external brain that can't remember what it was pointing at isn't much of a brain.

### UltimApe's archive

For UltimApe specifically, this means taking ~150,000 tweets spanning a decade of research into complex systems biology, neuroimmunology, microbiome science, and open-source medical devices, and putting them somewhere they can survive regardless of what happens to Twitter. The navigation model is unusual: there's no formal index. UltimApe navigates the corpus through word-association loops: chains of connected concepts that let him find specific tweets by wandering through semantic neighborhoods. This means the *relationships between posts* carry as much information as the posts themselves. A flat list of tweets, even with timestamps, is a dead archive. The living memex requires the graph.

To make this concrete: UltimApe can find any idea in his archive in seconds. He types `twitter[tab]from:ultimape signposts[enter]` into his browser without thinking, and he's at a thread he wrote months or years ago. The term "signposts" wasn't meaningful when he started that thread. It became the key to a whole cluster of ideas only after the pattern emerged. He searches for old tweets and remixes them into new narrative threads; people click through for context; he adds meta-notes. It's stigmergic thinking: each tweet is a trace left for his future self to follow. This pattern depends on deep full-text search across one's own corpus, a feature Twitter happens to have and other platforms (including Mastodon and Bluesky's current state) handle less consistently. It's hard to bucket things into topics when you're ontologically soft and ideas never fit normal categories. Patterns emerge much later. One of UltimApe's threads is over two years old and still growing: a mind embodied, now held captive by the very threads he needed to express.

### The urgency

But UltimApe's memex isn't just on Twitter. It's distributed across Twitter, Bluesky, Mastodon, Are.na, Medium, and his digital garden at wovensoup.com. A tweet links to an Are.na block that links to a garden page that references a Medium post. That's one thought spread across four systems, and the connections between them are where the meaning lives. Hivesong starts with the biggest and most urgent piece (Twitter→Bluesky), but the longer-term vision is a tool that can see and preserve the whole shape: a multi-platform archival system that captures content, media, and cross-references across all the places a distributed thinker actually works.

Even Community Archive, as valuable as it is, doesn't preserve images. If an account is deleted, the images and visual context are gone forever. The text survives but the evidence doesn't. A proper archival layer needs to capture everything (media, linked pages, embedded content), not just the text of the posts.

And accounts *are* being deleted. People deactivate. People die. Twitter has suspended deceased users' accounts, including Robin Williams'. When someone UltimApe has replied to or quoted disappears, his own tweets lose their context: they become statements without referents. The disappearance of thinkers like Pieter Hintjens into the aether is a real loss, not a hypothetical one.

Twitter is UltimApe's main serendipity and idea weaving engine. He's been trying to build the tool to liberate it for years, starting with Goliath in 2018, sketching architectures, collecting references, writing detailed technical plans that kept stalling at the implementation stage. In 2021, he described the situation in an email to someone who'd offered to help him think it through:

> "I basically need small scripts written to do things like pull data down from are.na's api and merge them with metadata layered into my twitter threads. But I can't focus long enough to work on the plumbing on my own, but I can talk with him about it and he's got a knack for it. I have the architecture planned out. It's the elbow grease I can't put into it."

That was four years ago. The architecture has been planned out for longer than most startups exist. A friend put it bluntly: "You probably hold one of the largest collections of knowledge on various cool stuff and the world would benefit greatly if you tried to systematize and publish some of it." They're right. But executive function disability makes sustained solo development brutally hard. What's needed is a collaborator and enough resources to dedicate focused time. That's what this project is asking for.

### For everyone

UltimApe isn't the only person whose thinking lives on Twitter. Independent researchers, open-source scientists, science writers, theorists, doctors and nurses talking about what they see in their wards, harm reduction educators, climate analysts, urbanists, accessibility advocates, the entire tpot constellation of people doing synthesis in public. Many of these accounts are sitting on archives that are equally irreplaceable to their authors. Some have already lost work to suspensions, deactivations, and changes to platform policy. Many more will.

This isn't a niche observation. Conor White-Sullivan, the founder of Roam Research (a for-pay memex tool), [wrote in 2019](https://x.com/Conaw/status/1129105485175480321):

> "Twitter as a company is culturally oriented around NOW. Product reflects this. The Twitter users who search their own and others' timelines for building webs of threads are hacking the platform to build a collaborative Memex (memory extension). Eventually those users will migrate."

That a memex-tool founder identified this pattern in 2019, and named UltimApe specifically as one of the users doing it, is third-party validation that this isn't one person's idiosyncratic problem. Bluesky is now the destination Conor predicted. The AT Protocol is the substrate. Hivesong is the bridge.

For everyone else on Community Archive, Hivesong means the same tool that liberates UltimApe's archive can liberate theirs. Upload your archive, connect your Bluesky account, and Hivesong handles the rest. A potential long-term direction is a hosted service where non-technical users could sync with one click, because most of the people whose work is worth preserving aren't the kind who run their own Linux servers. That's a Phase 3 possibility, not a near-term commitment.

**There's a deeper reason this needs to scale beyond one person.** Social media archives only stay meaningful when other people are around to talk to. The threads UltimApe is preserving aren't monologues; they're conversations with hundreds of other thinkers who replied, quoted, and built on each other's ideas over a decade. If only UltimApe migrates, his archive on Bluesky will be full of dead-end references to other people's tweets that still live on Twitter (and might not for long). The only way to fully preserve a *conversation* is for enough of the participants to migrate together. That's why broad accessibility matters, why a hosted service may eventually make sense, and why the architecture (Section 4.4) supports cross-user reference resolution as more people join.

The ultimate target, available to anyone who wants it, is a **self-hosted Bluesky PDS** (Personal Data Server). UltimApe's will live at [`bsky.wovensoup.com`](https://bsky.wovensoup.com), giving full ownership of the replicated memex in a format with a usable interface (any Bluesky client) on a federated protocol. It's part of a broader sovereignty stack: own hardware, own AI, own data, under nobody else's terms of service. Anyone willing to do the devops can replicate this stack. Anyone who isn't could (eventually) use a hosted version, if Phase 3 gets there.

### Why "Hivesong"?

A hive's song is the collective hum of a colony at work: thousands of individual bees, each pursuing their own task, producing together a steady resonance that only makes sense as a whole. The pitch shifts when the colony is healthy or stressed, when food is found, when a swarm is forming. No single bee carries the song; the song is what emerges when their movements coordinate. A Twitter archive is similar. Individual tweets look like isolated thoughts. The shape only becomes visible when the relationships between them are preserved. The colony is the conversation; the song is the thinking. That's what Hivesong protects.

---

## The Project

> The sections above describe **why** Hivesong should exist. The sections below describe **what's being built and how**. This middle section is the connective tissue: who's involved, what funding makes it real, what licenses apply, how to get involved.

### The team

Hivesong is a small collaboration:

- **UltimApe** (Nicholas Perry): domain owner. Knows what needs to be preserved and why. Sets product direction, dogfoods every milestone (the "actively using it" success criterion is non-negotiable), and maintains the project long-term. Owner of this spec.
- **[`@johnhenry`](https://mastodon.social/@johnhenry)** as developer collaborator, **actively engaged**. UltimApe became friends with them years ago through ongoing conversations about decentralization on Mastodon. They later published [*Mastodon Is Dead In The Water*](https://medium.com/@thejohnhenry/mastodon-is-dead-in-the-water-888c10e8abb1), a sharp, lived assessment of federated social platforms that demonstrates exactly the kind of clear-eyed analysis Hivesong's design needs. They bring 15+ years of professional web development experience and a long-standing intellectual investment in the user-empowerment side of the web.

  Their technical lineage runs through Bret Victor's *Inventing on Principle*, Brad Fitzpatrick's [Camlistore](https://en.wikipedia.org/wiki/Perkeep) (now Perkeep) work on personal data sovereignty, and the early-2010s Meteor.JS scene that took the web-as-application-platform seriously before most of the industry did. They have been actively engaged in shaping Hivesong's technical direction, including the data model decisions, the AT Protocol-shaped canonical record, and the Supabase-backed schema design. UltimApe is currently funding early prototype work directly via existing Patreon income while the funding drive is set up.
- **You, potentially**: the project is open from day one and welcomes contributors, especially around the Community Archive integration, the multi-user work, and the broader anti-rot archival layer (Section 4).

The "track record" argument for this team isn't a list of shipped startups; it's the combination of (a) a substantial corpus that demonstrates the problem is real and (b) a developer with both the relevant production experience and the right intellectual orientation toward the decentralization problem space. UltimApe's ~150,000 tweets, [Are.na channels](https://www.are.na/ultimape/channels), and [digital garden](https://garden.wovensoup.com) represent over a decade of sustained intellectual labor across complex systems biology, neuroimmunology, microbiome research, and open-source medical devices. The developer collaborator's background and credentials are described above. The tool is being built by someone who is the most demanding possible user, with implementation by someone whose taste and priors are aligned with the project's goals.

### Funding model

> **Want to chip in now?** UltimApe is funding the early prototype work directly via [Patreon](https://www.patreon.com/ultimape) (ongoing monthly) or [PayPal](https://www.paypal.com/donate/?hosted_button_id=3BVQAB7VCJJ7U) (one-shot donation; no PayPal account required, debit/credit card option available). Both go to UltimApe's general citizen-science research budget, which is what's currently underwriting Hivesong. The formal funding drive (with Hivesong-specific tracking) is being set up; until then, these channels are the way to throw something in the hat. See the FAQ for [more details on what to expect](#faq).

**Hivesong is a funding drive for a defined open-source deliverable.** UltimApe is already supported via [Patreon](https://www.patreon.com/ultimape) for general work; this project's funding is specifically for paying the developer collaborator to ship the prototype. The ask is scoped to a defined deliverable (the prototype reaching "actively using it"), with public progress visible throughout.

If the prototype goes well and there's appetite for it, UltimApe and the developer collaborator may build it into a small hosted service: a pay-as-you-go platform on top of the open-source core (see "Licensing and business model" below). That would be one way to fund continued development after the initial drive. It's not pre-committed; it's an option that the architecture leaves open. **Backers of this project are funding the open-source foundation that everything else might build on.**

**Funding tiers (matched to project phases):**

The project breaks naturally into three phases. Each phase is independently shippable and produces useful work even if later phases never happen.

| Tier | Phase | What it funds | Outcome |
|---|---|---|---|
| **Phase 1: Dogfooding** | UltimApe's archive | ~3-4 months of developer time, hosting | Hivesong prototype shipped. UltimApe is actively using the Bluesky mirror as a real archive of his ~150,000 tweets. Threading, timestamps, embeds, and media preserved. The architecture has been validated end-to-end on a serious corpus. |
| **Phase 2: Multi-user validation** | Other users' archives | Additional developer time | The pump is generalized beyond UltimApe-specific assumptions. At least one other user (probably someone from the Bluesky / Community Archive community) successfully migrates their archive. Edge cases that didn't show up in a single-user run get surfaced and fixed. |
| **Phase 3: SaaS stretch + open-sourcing** | Hosted service prep | Additional developer time + multi-user infrastructure | Pump engine extracted as a reusable library. Next.js dashboard scaffolding. Optional: a hosted service launches, with the AGPL service-layer code public from day one. The eventual goal is to relicense the entire stack permissively once development is sustainably funded. |

**On amounts**: specific dollar figures are being negotiated with the developer collaborator before any campaign launches. Each phase is roughly the size of a small open-source grant or a modest crowdfunded sum. Phase 1 alone is the meaningful baseline: UltimApe's corpus preserved on infrastructure he controls. Each subsequent phase compounds the value but doesn't gate Phase 1.

**Budget transparency commitments:**

- The final phase amounts will be published before any funding is solicited.
- A breakdown of where money goes (developer time, hosting, infrastructure, contingency) will be public.
- Spending against the budget will be reported at each milestone.
- Budget overages won't be added without backer notice.
- Any unspent funds at the end of a phase will either roll into the next phase (with backer notice) or be refunded proportionally.

The phases are designed to fit a range of funding sources: small microgrants and individual donations toward Phase 1, mid-size grants and sponsorships toward Phase 2, and larger institutional grants or sustainable revenue toward Phase 3.

**No timeline commitment.** The milestones are sequential and demonstrable; each ships when it's ready. Backers aren't buying a delivery date; they're funding the work. Realistic ballpark for Phase 1: 3-4 months of focused part-time effort.

**Funding channels under consideration:**

- **Open-source software grants**: a number of foundations and grant programs fund open-source infrastructure work in the data sovereignty, federated social, and user-agency spaces. Several look like reasonable fits for parts of this project; specifics will be disclosed as applications are submitted.
- **Direct sponsorship**: individuals, companies, or institutions who care about social media data sovereignty and want to fund a concrete deliverable.
- **Individual backers**: anyone who wants to chip in toward shipping the prototype, with paths through Patreon (earmarked contributions to UltimApe) or other small-pledge mechanisms to be set up before any campaign launches.

### Why this attempt is different

A natural question, given that UltimApe's previous Twitter-archive project (Goliath, 2018) stalled at the implementation stage: why won't this one stall too? Several things have changed:

- **There's a paid developer this time.** Goliath was a solo side project, fitting around UltimApe's executive function constraints. Hivesong isn't. The developer collaborator is being engaged to ship this, with funding to support that engagement. Implementation is no longer dependent on UltimApe's capacity to grind through it alone.
- **The data layer exists now.** In 2018, getting tweets out of Twitter required wrestling with rate-limited APIs and incomplete archive exports. Both problems are solved by Community Archive (which didn't exist yet) and the AT Protocol's explicit support for backdated imports (which didn't exist yet). The actual technical lift is dramatically smaller than it was in Goliath's era.
- **Twitter's export format is a known quantity now.** Goliath stalled in part because Twitter kept changing the archive format mid-development. The format has been relatively stable for several years, and the tooling around it is mature.
- **The architecture has been planned out for years.** As the 2021 email above shows, the design has been clear in UltimApe's head since at least then. What was missing was elbow grease, not vision. This project is structured to provide the elbow grease.
- **The scope is bounded.** Goliath aimed at "fix Twitter's broken archive in general." Hivesong aims at "pump tweets to Bluesky, structure intact." The smaller scope means it can actually finish.
- **There's accountability.** A funded developer with shipped milestones on a public repository is a different shape than a solo project on a personal GitHub. Public progress, public budget, and a paying collaboration all create pressure to ship that didn't exist for Goliath.

This isn't a guarantee. Open-source projects stall for many reasons, and the risk is real. But the conditions that caused Goliath to stall have substantially changed.

### Licensing and business model

> **TL;DR for non-developers:** The actual Hivesong tool (the thing that copies your tweets) is free, open-source, and yours to keep, forever. You can run it yourself for free if you can manage a Linux server. *If* a paid hosted version is ever built (so non-technical people can use it without running servers), the hosting code would be open-source too, but with a license that asks commercial competitors to share their improvements back. Either way, the tool itself is always free.

Hivesong uses a **two-tier licensing structure** with a clear and unambiguous boundary:

- **The core self-hosted product**: the pump engine, CLI, and everything in Sections 1-3 of this document, will **always be released under a maximally permissive license** (likely **MIT**, possibly Apache 2.0). Anyone can use, fork, modify, redistribute, sell, or build on it with essentially no restrictions. This is non-negotiable: the actual tool that does the Twitter→Bluesky migration is and will remain genuinely free software in the most permissive sense. People who want to run Hivesong themselves will always be able to do so without anyone's permission.

- **The optional SaaS scaffolding**: *if and when* it gets built (Section 4.3): the multi-tenant dashboard, billing integration, account management, and hosted-service-specific infrastructure, would be released under **AGPLv3**. This layer is what would make a commercial hosted offering possible; it's strictly additional to the core, not a replacement for any of it. Users who self-host never need to touch it. AGPLv3 means the code is still open and inspectable, anyone can self-host the SaaS scaffolding too, and anyone can fork it for their own commercial use. The only requirement is that commercial network operators must publish their modifications. The reciprocity ensures improvements flow back to the ecosystem rather than disappearing into closed forks.

**Why this structure:**

If Hivesong goes well and there's interest, UltimApe and the developer collaborator may build it into a small hosted service: a pay-as-you-go platform that handles the sync for users who don't want to run infrastructure themselves. The revenue from that service would fund continued development, maintenance, security updates, and feature work for everyone, including self-hosters. Without a hosted option, the network effect described in "For everyone" never kicks in. None of this is decided yet; it's the path the architecture leaves room for.

The AGPLv3 license is what would make that funding sustainable, but it's worth being precise about how. AGPL doesn't actually prohibit competing commercial services; anyone can fork the code and run a competing hosted version, they just have to publish their modifications under the same license. What AGPL really does is two things: (1) it requires that anyone running a hosted version make their changes public, so improvements flow back to the ecosystem rather than getting absorbed into private forks; and (2) it creates enough friction that most large cloud companies (Google, for instance, has an explicit policy against AGPL code) won't adopt it. That second effect is what protects small maintainers from being commodified by hyperscalers. AGPL doesn't prevent anyone from self-hosting, contributing, or even running their own commercial fork. It just keeps the code reciprocal and discourages the specific scenario where a giant cloud provider repackages the work without giving anything back.

**What this means for backers:**

- **The core is and always will be free.** No matter what happens to any future hosted service, the prototype's code stays MIT/Apache. You can always self-host.
- **Any hosted service is opt-in.** If you'd rather run Hivesong yourself, you can. A hosted service, if it ever exists, would be a convenience for people who don't want to do devops, not a gate.
- **Backing this project is backing the open-source core.** The funding goes to building the freely-available tool. Any future hosted service is a downstream possibility, not what you're directly supporting.
- **Backers won't be locked in to anything.** Data on a hypothetical hosted service would be just standard Bluesky records on a PDS, fully portable. Users could migrate to self-hosted at any time using AT Protocol's account migration mechanisms.
- **No vendor monopoly.** AGPLv3 doesn't restrict competition. Anyone can fork the SaaS scaffolding and run their own paid hosting service for their own community: a research collective, school, publication, local cooperative, or commercial competitor. The only requirement is publishing modifications under the same license. There's no negotiation with the original maintainers, no "individual license" gatekeeping, no relicensing pathway controlled by anyone but the license itself. This is the model that [Write.as](https://write.as) uses (you can pay for blog hosting *or* self-host the open-source [WriteFreely](https://writefreely.org) engine yourself), and that the Mastodon ecosystem uses (multiple hosts offer paid Mastodon instances using the same open code). If our hosting is bad, slow, or expensive, someone else can compete using the same engine.
- **Exit-ramp commitment with a hard trigger.** If any future hosted service is shut down or abandoned, the service-layer code automatically relicenses to the same permissive MIT/Apache license as the core. The maintainers commit to encoding this in the LICENSE file from the day the AGPL service layer is first published, so it survives company changes, acquisitions, or maintainer attrition. Specifically: **if no commit is made to the service-layer repository for 12 consecutive months, the AGPL license terminates and is replaced by MIT/Apache automatically.** This isn't a verbal promise. It's a clause in the LICENSE.
- **AGPL is temporary, not permanent.** The intent is to relicense the entire codebase to a permissive license once development is sustainably funded by other means (grants, established subscriber base, institutional support). To make this concrete: **the AGPL service-layer code automatically relicenses to MIT/Apache no later than five years after its initial public release**, regardless of project state. The five-year cap is in the LICENSE file and is absolute: it cannot be extended by maintainers, contributors, backers, or any successor entity. The AGPL exists to make the bootstrapping period viable; it's not a destination, and the LICENSE prevents it from becoming one.
- **Scoped CLA.** Contributors grant rights bound to the LICENSE clauses above (MIT/Apache core, optional AGPLv3 service layer, and the documented auto-relicense triggers). This is *not* a CLA granting maintainers unilateral relicensing rights; it grants exactly what the LICENSE already commits to publicly, no more. This addresses the orphan-contribution problem (when the auto-relicense trigger fires, no individual contributor consent is needed) while preserving the structural commitments described above. The CLA also serves as a light contributor filter: with a paid developer and committed maintainer, the project benefits more from a smaller set of considered contributions than from drive-by PRs.

### What backers get

The licensing section above describes structural commitments (auto-relicense triggers, scoped CLA, exit ramps). This section describes what backing the project specifically gets you in practical terms.

- **Public, observable progress.** The repository is public from M0. Commits are visible as they're made. Milestones are announced when reached. A budget tracker shows cumulative spend against the approved phase total. There's no "we'll share an update next quarter" black-box period.
- **A working tool, free forever.** Once Phase 1 ships, anyone can self-host the result. Backers don't unlock the tool; they unlock its existence. The same MIT/Apache code goes to backers and non-backers alike on the day it's published.
- **Influence on the open questions.** Several real decisions are unresolved (Section 5): how to handle tweets exceeding Bluesky's 300-grapheme limit, whether to support video, what the live sync poll interval should be, how aggressively to expand quote-tweets. Backers get a vote on these, not a poll-result vote where everyone clicks a button, but real input into the design discussion via a public channel (likely GitHub discussions or a dedicated thread).
- **First access to a hosted service if it ever launches.** Phase 3 is uncertain, but if a hosted service does launch, project backers get free or discounted access for at least one year (specifics depend on hosting costs at the time). This is a thank-you, not a perk that gates anything: anyone can self-host the same code for free.
- **Recognition where appropriate.** Backers who want to be credited can be listed in the project README or a CONTRIBUTORS file. Anonymous backing is also fine; nobody is required to be named.
- **A receipt and progress reports.** For grant funders specifically: standard reporting (interim and final reports), all milestones tied to public deliverables, full budget breakdown with line items for developer time, hosting, and infrastructure. For individual backers: at minimum, a "shipped" announcement when each milestone lands.

---

## 1. Structural Requirements (What Must Survive)

These are the non-negotiable invariants. If any of these are lost, the migration has failed.

A memex isn't just a collection of posts; it's an environment that supports a specific set of cognitive operations. The design vocabulary for what a user *does* with their memex:

```
Navigate:   search, trace, backtrack, explore, map
Connect:    associate, crosslink, cross-reference, cross-pollinate, nest, embed
Compose:    stitch, weave, narrate, arrange, group, layer
Transform:  remix, digest, elucidate, channel, serendipity
Share:      create, capture, contain, share
```

These are the verbs the migration must preserve the capacity for. Every structural requirement below exists to protect one or more of these operations. Threading preserves the ability to *trace* and *backtrack*. Timestamps preserve the ability to *map* ideas across time. Quote-posts preserve the ability to *crosslink* and *weave*. Links preserve the ability to *explore* and *associate*. If the migration flattens the graph, these verbs stop working, and the memex is dead even if every word survives.

### 1.1 Thread Structure

Twitter threads are not linear. They branch. A thread started in 2016 gets a reply in 2019, a quote-tweet callback in 2021, and a confirmation post in 2022. The reply-chain structure (which tweet is `in_reply_to` which) must be fully reconstructed in Bluesky's reply model.

Bluesky threading requires each reply to reference both the **root** post (the original thread starter) and its **parent** post (the immediate predecessor). The pipeline must create posts in dependency order: roots first, then replies in sequence.

Specific requirements:

- Every `in_reply_to_status_id` relationship must produce a corresponding Bluesky reply with correct `root` and `parent` references.
- Self-replies (the most common thread pattern: me replying to myself) must chain correctly.
- Replies to other users' tweets that are not in the corpus should be handled gracefully: either posted as standalone with a note, or with a placeholder reference.
- The tool must detect and preserve **branching**: cases where multiple tweets reply to the same parent.

### 1.2 Timestamps as Epistemic Record

The `created_at` timestamp on a tweet is not decoration. It is evidence of *when a claim was made*. This matters because the corpus contains prediction/confirmation pairs: a claim at time T₁ and supporting evidence appearing at T₂, sometimes months or years later. Destroying timestamps destroys the evidentiary value of the archive.

Bluesky supports this: the `createdAt` field in `app.bsky.feed.post` accepts arbitrary past dates. Posts with backdated timestamps display a "backdated" indicator in the UI, which is actually desirable: it signals "this is imported historical content" rather than pretending it's new.

Specific requirements:

- Every Bluesky post must carry the original Twitter `created_at` timestamp in its `createdAt` field.
- Posts must be created in chronological order within each thread to ensure reply chains resolve correctly.
- The pipeline should log the mapping: `twitter_id → bluesky_at_uri` for every post, so cross-references can be resolved.

### 1.3 Quote Posts and Embeds

Twitter quote-tweets are a primary mechanism for weaving: connecting ideas across threads and time. A quote-tweet from 2021 referencing a post from 2018 is a semantic link, not just a repost.

Bluesky supports quote posts via the `embed` field with type `app.bsky.embed.record`. This requires the `uri` and `cid` of the quoted post.

Specific requirements:

- If the quoted tweet is **in the corpus**: resolve it to its Bluesky URI and embed it as a proper quote post.
- If the quoted tweet is **not in the corpus** (someone else's tweet): embed the original Twitter URL as a link card / external embed.
- In the pump model, chronological ordering handles most dependency resolution naturally; the quoted tweet was almost always posted before the tweet that quotes it. Edge cases should fall back to the Twitter URL.

Note: quote-tweets are not stored as a structured field in the Twitter archive. They appear as [https://twitter.com/user/status/{id}](https://twitter.com/user/status/{id}) URLs within the tweet's entities. The parser must detect these URL patterns to extract quote-tweet relationships. (See Section 2.2 for details.)

### 1.4 Cross-Platform References

The corpus contains URLs pointing to Are.na channels and blocks, digital garden pages at [`garden.wovensoup.com`](https://garden.wovensoup.com), academic papers, blog posts, and other external sources. These URLs must survive as-is in the Bluesky post text. No special handling needed beyond ensuring they're preserved during text transformation (e.g., if shortening for Bluesky's ~300 char limit).

### 1.5 Media Attachments

Tweets with images should have those images uploaded to Bluesky as blobs and attached via `app.bsky.embed.images`. Bluesky supports up to 4 images per post.

Community Archive does not preserve tweet images. Media must be sourced from the local Twitter archive's `tweet_media/` directory or from Twitter's CDN (`pbs.twimg.com`) if URLs are still live. Image capture should be treated as urgent: images that exist today may not exist next month. See Section 1.6 for the broader anti-rot strategy.

### 1.6 Link and Media Preservation (Anti-Rot)

A tweet that says "this paper shows X" with a dead link is a thought with its evidence amputated. Link rot in a memex isn't just inconvenience; it's brain damage.

Strategies, ordered from most urgent to aspirational:

- **Image capture during pump execution** (prototype priority): When pumping a tweet with media, fetch images from Twitter's CDN and upload to Bluesky. Fall back to local archive. Log any losses. This is the one anti-rot measure that belongs in the prototype.
- **t.co unwrapping** (prototype priority): Twitter wraps all URLs in `t.co` shortened links that redirect through Twitter's servers. If `t.co` ever goes down, every link in every tweet breaks simultaneously. The pump should use the expanded URL from the archive's `entities` field instead of the `t.co` version.
- **Multi-hop URL unshortening** (prototype priority): Twitter's `entities.expanded_url` only resolves one hop past `t.co`. Many archive URLs go through additional shorteners (`bit.ly`, `ow.ly`, `goo.gl`, `tinyurl.com`, vendor-specific shorteners), sometimes 5-7 redirects deep. Each shortener is a single point of failure: when `goo.gl` shut down in 2019, every link through it broke regardless of destination liveness. The pump should resolve URLs through their full redirect chain and store the canonical destination, not a brittle chain. UltimApe's archive is the test corpus. Implementation approach and external-tooling considerations are described in Section 3.10.
- **URL liveness checks** (post-prototype): For each URL in a tweet being published, check if it's still live. Log dead links. Check for archived copies on the Wayback Machine or archive.today. Liveness checks should happen *after* unshortening, since shortener-domain liveness is not the same as destination liveness.
- **Proactive archiving** (post-prototype): Submit outbound URLs to the Internet Archive's Save Page Now API (`web.archive.org/save/{url}`) to ensure snapshots exist. Gwern's archiver-bot ([`github.com/gwern/archiver-bot`](https://github.com/gwern/archiver-bot)) is prior art for this pattern.
- **Periodic health checks** (ongoing maintenance): Re-check outbound URLs for rot after migration. Flag newly dead links and find archived copies.

**Future scope (multi-platform preservation)**: UltimApe's memex spans Twitter, Bluesky, Mastodon, Are.na, Medium, and a digital garden. Each platform has its own fragility. A future evolution of Hivesong could act as a broader archival layer that captures and cross-references content across all of these, ensuring no single platform's failure can sever the connections. This is beyond current scope but the data model should be platform-agnostic where possible.

---

## 2. Data Sources and Formats

### 2.1 Community Archive (Remote API)

The Community Archive ([`community-archive.org`](https://community-archive.org)) is a public Supabase database of volunteered Twitter archives. UltimApe's corpus is already uploaded there under username `ultimape`. It's one of two first-class data sources Hivesong supports (see Section 2.2 for the local archive).

- **Supabase REST API** with public anon key (published in repo docs)
- GitHub: [`github.com/TheExGenesis/community-archive`](https://github.com/TheExGenesis/community-archive)
- API docs: [`github.com/TheExGenesis/community-archive/blob/main/docs/api-doc.md`](https://github.com/TheExGenesis/community-archive/blob/main/docs/api-doc.md)
- Query example: `GET /rest/v1/tweets?account_id=eq.{id}&select=*`
- Also available as bulk JSON download per user
- **Near-real-time updates**: Community Archive has a browser plugin that syncs new tweets as they're posted on Twitter. This enables Hivesong's live sync mode (Section 3.8).

Key advantages of Community Archive as a source:

- No local file management; data is already normalized and queryable via API.
- Other users' tweets in the database can resolve reply context.
- The pipeline can run from anywhere; no need to ship around archive zip files.
- New tweets flow in automatically via the browser plugin, enabling continuous sync.

Key limitation: Community Archive is a small project itself. If it's offline for a period, or its schema changes, Hivesong shouldn't grind to a halt. That's why local archive ingestion (Section 2.2) is a co-equal first-class source rather than just a fallback.

### 2.2 Twitter Archive (Local)

The standard Twitter data export (`twitter-archive.zip`) is the other first-class data source. It contains:

- `tweets.js` / `tweet.js`: Array of tweet objects with full metadata
- Each tweet has: `id`, `full_text`, `created_at`, `in_reply_to_status_id`, `in_reply_to_user_id`, `entities` (URLs, mentions, hashtags, media), `extended_entities` (images, video)
- Media files in `tweet_media/` directory

The local archive provides resilience: if Community Archive is offline or hasn't been updated recently, Hivesong can read directly from the user's downloaded `.zip` file. It also covers tweets that may not be present in Community Archive (deleted tweets, tweets from before the user uploaded their archive) and is the authoritative source for media files (Community Archive doesn't preserve images).

In practice, both sources can be used together: Community Archive for the live-sync polling loop, local archive for the one-time backfill and for media hydration. The pump should be implemented against a `Source` interface so additional sources (Mastodon archives, ActivityPub exports, etc.) can be added without rewriting the pipeline.

**Prior art (historical reference only)**: The `ultimape/goliath` repo ([`github.com/ultimape/goliath`](https://github.com/ultimape/goliath)) is an earlier, **incomplete** project that attempted to parse the Twitter archive. **Goliath does not work with the current Twitter export format**: Twitter changed their archive structure during development, and the project stalled. It should not be used as a code dependency. However, its README documents insights relevant to Hivesong's design:

- **Quote-tweet detection**: Quote-tweets are not a structured field in the archive. They appear as `https://twitter.com/user/status/{id}` URLs within the tweet's URL entities. Any parser must pattern-match these to extract quote-tweet relationships.
- **Retweet dual timestamps**: Retweet proxy tweets carry *your* timestamp (when you retweeted), while the encapsulated original carries the author's timestamp. Both are relevant to the epistemic record.

### 2.3 Are.na (Remote API)

Are.na hosts research channels that tweets frequently reference. The API (now at V3, migrated from V2) supports:

- `GET /v2/channels/:slug`: channel contents with pagination
- `GET /v2/blocks/:id`: individual blocks
- `GET /v2/search?q=:query`: full-text search
- Auth: OAuth or personal access token from [`dev.are.na/oauth/applications/new`](https://dev.are.na/oauth/applications/new)

Key channels for UltimApe:

- [`are.na/ultimape/root-tweets-toots`](https://are.na/ultimape/root-tweets-toots): curated index of important threads (~241 items)
- [`are.na/self-exploration/index`](https://are.na/self-exploration/index): research notes (massive, 1600+ in microbiome sub-channel alone)

Are.na data is **not being migrated**: it stays on Are.na. But the pipeline may need to resolve Are.na URLs in tweets to ensure they're still valid, and could optionally enrich the graph with Are.na block metadata.

### 2.4 Digital Garden (Static Site)

[`garden.wovensoup.com`](https://garden.wovensoup.com): a digital garden hosted on GitHub. Contains long-form versions of ideas that tweets link to. URLs just need to be preserved in post text; no API interaction needed.

### 2.5 Bluesky (Target)

UltimApe's Bluesky handle is `@ultimape.bsky.wovensoup.com`. The initial prototype can target [`bsky.social`](https://bsky.social) (Bluesky's hosted PDS), but the long-term target is a self-hosted PDS (see Section 4.1). The PDS endpoint should be configurable from the start.

- **AT Protocol API** via [https://bsky.social/xrpc/](https://bsky.social/xrpc/) (or the self-hosted PDS endpoint)
- Auth: App Password (created in Bluesky settings), session via `com.atproto.server.createSession`
- Post creation: `com.atproto.repo.createRecord` with collection `app.bsky.feed.post`
- Threading: `reply` field with `root` (first post in thread) and `parent` (immediate predecessor)
- Quote posts: `embed` field with type `app.bsky.embed.record`
- Images: upload blob via `com.atproto.repo.uploadBlob`, reference in `embed` with type `app.bsky.embed.images`
- `createdAt` accepts arbitrary past timestamps (backdating is explicitly supported for imports)
- Rate limits exist; the pipeline needs to respect them with delays between posts
- **SDK: `@atproto/api`** (TypeScript). Hivesong is built in TypeScript / Node. See Section 3 for rationale.

---

## 3. Architecture (Prototype Scope)

This section describes what to build for the working prototype: a self-hosted CLI tool that pumps tweets from Community Archive to Bluesky. Future directions (self-hosted PDS, multi-user service, bidirectional sync, cross-user remapping) are in Section 4.

### Tech Stack

- **Language**: TypeScript on Node (current LTS)
- **AT Protocol SDK**: [`@atproto/api`](https://www.npmjs.com/package/@atproto/api): official Bluesky client
- **State storage**: Supabase (Postgres) via [`@supabase/supabase-js`](https://github.com/supabase/supabase-js). Self-hostable via Docker Compose; cloud-hosted version available for free tier deployments. Same backend as the eventual hosted service (Section 4.3) and as Community Archive itself.
- **HTTP client**: native `fetch` (Node 20+) for Community Archive's REST API
- **CLI framework**: [`commander`](https://github.com/tj/commander.js) or similar for the command interface
- **Testing**: [`vitest`](https://vitest.dev/): fast, TypeScript-native. Highest-value test categories for this project: facet byte-range math, dependency-order edge cases (replies before parents), `t.co` URL unwrapping, multi-hop URL unshortening (handling 5-7 redirect chains, including known-shutdown shorteners like `goo.gl`), retry queue behavior on failure, deterministic `rkey` generation, and idempotency of writes. Test the bits where "successfully publishes wrong things" is the silent failure mode.

**Why TypeScript:**

1. The official AT Protocol SDK (`@atproto/api`) is TypeScript. Bluesky's own team uses it; features and fixes land here first.
2. AT Protocol record types are deeply nested (facets, embeds, replies). TypeScript's type system catches structural errors at compile time rather than during a multi-hour backfill.
3. The multi-user future (Section 4.3) is already committed to Next.js + Supabase. Same language across prototype and service means no rewrite later.

**Why Supabase (not SQLite):**

1. **Same backend as Community Archive.** CA already runs on Supabase. Hivesong's state lives in the same ecosystem, with potential for tighter integration later (shared project, database-level triggers).
2. **Zero rewrite for the hosted service.** The prototype and the multi-user version (Section 4.3) use the same schema, the same client library, the same auth model. The prototype is a single-user instance of the eventual multi-user system, not a separate codebase.
3. **Self-hostable.** Supabase is open source (Apache 2.0) and runs via Docker Compose. UltimApe runs it on his own hardware alongside the self-hosted PDS. No cloud dependencies, full sovereignty.
4. **Postgres beats SQLite for this workload.** Better JSON handling for `retry_queue` and `epoch_counts`, real concurrency, full-text search if ever needed.

The pump engine is pure TypeScript with no UI dependencies. The eventual web dashboard (Section 4.3) wraps the same engine in Next.js without changing it.

### Success Criteria

**Phase 1 (the prototype)** is done when each of these is independently demonstrable:

1. **Full backfill complete**: All ~150,000 tweets from UltimApe's Community Archive corpus are present on the target Bluesky PDS as `app.bsky.feed.post` records. *Verifiable*: a count query against the PDS returns the same number of records as the corpus (within a documented small variance for known-skipped tweets, e.g., DM-related entries).
2. **Structural fidelity preserved**:
   - **Reply chains**: 100% of in-corpus replies have correct `root` and `parent` references when checked via the AT Protocol API. *Verifiable*: a sample of ≥50 multi-tweet threads (including ≥10 with branching) renders correctly when viewed on bsky.app.
   - **Timestamps**: 100% of records carry the original `createdAt` from the Twitter archive, not the pump time. *Verifiable*: a query of `createdAt` distribution shows the expected ~10-year span, not all records clustered at pump-run time.
   - **Quote-tweets**: in-corpus quotes resolve to `app.bsky.embed.record` references; out-of-corpus quotes fall back to `app.bsky.embed.external` link cards pointing at Twitter. *Verifiable*: a sample of ≥20 quote-tweets renders correctly in both cases.
   - **Media**: image-bearing tweets show as `app.bsky.embed.images` with the correct number of blobs (1-4 per post). *Verifiable*: a sample of ≥30 image-tweets shows the right images on the right posts.
   - **Link unwrapping**: zero `t.co` URLs remain in the published text; all are replaced with their expanded forms. *Verifiable*: a search of the published corpus on the target PDS returns zero results matching `t.co/`.
   - **Multi-hop unshortening**: outbound URLs in published posts are resolved to their canonical destination, not intermediate shortener URLs (`bit.ly`, `ow.ly`, etc.). *Verifiable*: a sample of ≥30 outbound URLs from the published corpus, traced manually, shows the published URL matches the final destination of the original URL's redirect chain (or, for known-shutdown shorteners like `goo.gl`, shows the captured destination from the unshortening cache).
3. **Live sync operational**: The pump runs in continuous mode, polling Community Archive every 5-15 minutes for new tweets. *Verifiable*: a tweet posted on Twitter, captured by the Community Archive browser plugin, appears on Bluesky within one polling cycle plus rate-limit slack.
4. **Resumability**: An interrupted backfill (kill -9 mid-run) resumes from the cursor without duplicate posts and without data loss. *Verifiable*: deliberate interruption test mid-backfill shows the same final-record count as an uninterrupted run.
5. **Dry-run produces a useful diff**: The dry-run mode output is human-readable enough that a developer can identify potential issues (overflow tweets, unresolvable references, missing media) before running the real pump.
6. **Actively used**: UltimApe is using the resulting Bluesky archive as a real mirror, not just verifying it as a tech demo. New tweets posted on Twitter appear on Bluesky automatically and remain visible/searchable on his profile.

**Phase 2 (multi-user validation)** is the next phase, with its own success criteria:

- The pump runs successfully against at least one other user's Community Archive corpus.
- UltimApe-specific assumptions are surfaced and addressed (handle conventions, archive size assumptions, edge cases).
- The setup process is documented well enough that a technically comfortable user can self-host without UltimApe's direct help.

Phase 3 (SaaS scaffolding and eventual permissive relicensing) is described in Section 4.3.

### Milestones

Rough sequencing for the build. Each milestone produces something demonstrable. Effort estimates are placeholders; actual time depends on the developer's availability and AT Protocol learning curve.

**M0: Foundation**
- Repo set up, TypeScript + Node project skeleton, CI green
- CLI scaffold with `commander`
- Docker Compose for self-hosted Supabase (or Supabase cloud project) running locally
- Initial schema migration applied (`sync_state`, `sync_failures`, `cid_cache` tables)
- Auth working: can create a Bluesky session, can hit Community Archive's Supabase REST API, can connect to Hivesong's Supabase
- *Demonstrable*: `hivesong status` prints "connected to CA as ultimape, connected to Bluesky as @ultimape.bsky.wovensoup.com, Supabase OK, cursor: not yet started"

**M1: Single Tweet End-to-End**
- Fetch one tweet from Community Archive
- Post it to Bluesky with original `createdAt`
- Verify with `getRecord` using deterministic `rkey`
- **Empirical rate-limit characterization**: post a small batch (e.g. 50-100 tweets) at increasing rates, watch for 429s, document the actual `createRecord` ceiling. This determines viable backfill duration and shapes M5 expectations.
- *Demonstrable*: `hivesong pump --limit 1 --dry-run` shows what would happen; without `--dry-run`, posts a single tweet. Rate limit findings are written up in the repo.

**M2: Threading**
- Reply chains: correct `root` and `parent` references
- Self-replies (the dominant case)
- Replies to non-corpus tweets (graceful degradation)
- Branching threads
- *Demonstrable*: A 10-tweet self-reply chain on Bluesky that matches the Twitter original

**M3: Embeds & Media**
- Quote-tweets within the corpus resolved to Bluesky URIs
- External quote-tweets as link card embeds
- Image upload via `uploadBlob` and attachment to posts
- `t.co` URL unwrapping
- Multi-hop URL unshortening: validate the chosen approach (Section 3.10 covers the implementation choice) against a sample of UltimApe's archive URLs and wire it into the pipeline.
- Facet generation for mentions, links, hashtags
- *Demonstrable*: A quote-tweet with an attached image, rendering correctly on Bluesky, with all outbound URLs resolved to their canonical destinations rather than intermediate shorteners

**M4: Resumability**
- Persistent cursor and mapping state
- Can be killed and restarted without duplicate posts
- Failure queue with retry logic
- Rate limit handling (back off when Bluesky returns 429)
- *Demonstrable*: Kill the pump mid-run; restart picks up at the right tweet

**M5: Full Backfill**
- Successfully pumps all ~150k tweets
- No data loss; failures logged for manual review
- Performance acceptable (e.g., backfill completes in days, not weeks)
- *Demonstrable*: UltimApe's Bluesky account contains the full archive

**M6: Live Sync**
- Polling mode that runs continuously
- New tweets in Community Archive appear on Bluesky within one polling cycle
- Survives network interruptions, API errors, restarts
- *Demonstrable*: Tweet from Twitter → Community Archive picks it up via browser plugin → Hivesong publishes to Bluesky, all within ~15 minutes

### Getting Started

This is a suggested starting point. The repo structure, CLI surface, and config schema below are conventions, not constraints. A developer should feel free to refactor as the design evolves.

**Suggested repo layout:**

```
hivesong/
├── src/
│   ├── cli.ts                       # commander setup, entry point
│   ├── pump.ts                      # main pump orchestration
│   ├── sources/
│   │   ├── community-archive.ts     # CA Supabase REST client
│   │   └── twitter-archive.ts       # local archive parser (for media fallback)
│   ├── targets/
│   │   └── bluesky.ts               # @atproto/api wrapper
│   ├── state/
│   │   ├── db.ts                    # @supabase/supabase-js client setup
│   │   ├── cursor.ts                # cursor read/write
│   │   └── retry-queue.ts           # failure handling
│   ├── transforms/
│   │   ├── facets.ts                # Twitter entities → Bluesky facets
│   │   ├── threading.ts             # reply reference resolution
│   │   ├── embeds.ts                # quote-tweets, media uploads
│   │   └── urls.ts                  # t.co unwrapping + multi-hop unshortening
│   └── types.ts                     # shared TypeScript types
├── tests/
├── supabase/
│   ├── migrations/                  # SQL migration files
│   └── config.toml                  # supabase CLI config
├── docker/
│   ├── Dockerfile                   # for the hivesong container
│   └── docker-compose.yml           # hivesong + supabase together
├── .env.example
├── package.json
├── tsconfig.json
└── README.md
```

**Configuration via environment variables** (Docker-friendly):

```
HIVESONG_CA_USERNAME=ultimape
HIVESONG_BSKY_HANDLE=ultimape.bsky.wovensoup.com
HIVESONG_BSKY_APP_PASSWORD=xxxx-xxxx-xxxx-xxxx
HIVESONG_BSKY_PDS=https://bsky.social         # or self-hosted PDS URL
SUPABASE_URL=http://localhost:54321         # self-hosted or cloud URL
SUPABASE_ANON_KEY=eyJ...                    # from supabase setup
SUPABASE_SERVICE_KEY=eyJ...                 # for migrations and admin ops
HIVESONG_POLL_INTERVAL_SEC=600                # live sync interval
HIVESONG_LOCAL_ARCHIVE_PATH=./twitter-archive # optional, for media fallback
```

**Suggested CLI surface:**

```
hivesong status               # show connection state, cursor position, synced count
hivesong pump                 # run backfill mode (one pass)
hivesong pump --limit N       # pump only N tweets (testing)
hivesong pump --dry-run       # parse and plan, don't post
hivesong sync                 # run live sync mode (long-lived process)
hivesong retry                # drain the retry queue
```

**Quickest possible test (after install + .env setup):**

```
npm run cli -- pump --limit 10 --dry-run
```

This fetches the first 10 tweets from Community Archive, builds the dependency graph, and prints a report of what *would* be posted, without actually publishing anything to Bluesky. It's the single fastest way to verify that credentials, network access, and parsing all work end-to-end before committing to the full backfill.

**State backend options:**

The prototype defaults to Supabase, but a developer working on the pump should not be required to spin up a Supabase instance just to test parsing logic. The `state/` module should support a JSON-file fallback as an alternative backend when `SUPABASE_URL` is unset. This keeps the early-development feedback loop fast (no Postgres needed for unit tests, dry-runs, or local hacking on facet logic) while preserving Supabase as the production backend. The JSON fallback is intentionally not suitable for live multi-user deployments. It exists for development and small-archive single-user runs.

**Local development:**

1. `git clone` and `npm install`
2. Copy `.env.example` to `.env` and fill in credentials
3. `npm run dev` runs the CLI in watch mode via `tsx`
4. `npm test` runs the vitest suite

**Docker self-hosting:**

`docker-compose.yml` should run two services together: Hivesong as a long-lived process (running `hivesong sync`) and a self-hosted Supabase stack for state storage. Supabase's official Docker Compose configuration provides Postgres, Auth, REST API, Realtime, and Studio. Hivesong only needs Postgres and the REST API for the prototype, but the rest comes for free and supports the future hosted service path. Alternatively, point `SUPABASE_URL` at a cloud Supabase project (free tier is plenty for single-user state) and skip the local Supabase containers. Hivesong becomes a single small Node Alpine container.

**Where to start:**

M0 (project skeleton + auth) is the obvious first task. A working `hivesong status` that successfully connects to both Community Archive and Bluesky and reports the cursor position is the smallest meaningful demo. Everything else builds on that.

The first PR should ideally include:
- `package.json` with the dependencies named in Tech Stack
- `tsconfig.json` with strict mode enabled
- `src/cli.ts` with the `commander` skeleton
- A working `status` command that reads `.env`, calls Community Archive's profile endpoint, calls Bluesky's `getSession`, and prints the results
- A `Dockerfile` and `docker-compose.yml` (even if minimal)
- A README with setup instructions

That establishes the project shape. Subsequent PRs implement the milestones.

### 3.1 Core Concept: The Pump

Hivesong is a **pump**: it connects to Community Archive on one end and Bluesky on the other, and pushes tweets through in chronological order. It maintains a cursor (the timestamp of the last successfully synced tweet) and advances forward through the corpus.

```
Twitter ──► [browser plugin] ──► Community Archive DB ──► [MURMUR PUMP] ──► Bluesky
                                        │                      │
                                   Supabase API            AT Protocol API
                                        │                      │
                                   (source of truth)     (target mirror)
                                                               │
                                                        [State: Supabase]
                                                        sync_state, failures, cid_cache
```

Two operating modes, same pipeline:

- **Backfill mode**: Start from the earliest tweet and pump forward chronologically through the entire history. Could take hours/days depending on corpus size and rate limits.
- **Live sync mode**: After backfill, poll Community Archive periodically for new tweets. Pump any new ones to Bluesky. (See Section 3.8.)

### 3.2 Phase 1: Fetch from Community Archive

Query the Supabase API for tweets, ordered by `created_at` ascending, starting from the cursor position. Paginate as needed.

For each tweet, normalize into a common record:

```
{
  twitter_id: string,
  text: string,
  created_at: ISO8601,
  in_reply_to_id: string | null,
  quoted_tweet_id: string | null,  // extracted from URL entities (see Section 2.2)
  media_urls: [string],
  urls: [{ original, expanded }],
  mentions: [{ username, id }],
  hashtags: [string]
}
```

### 3.3 Phase 2: Dependency Check

Before publishing a tweet, check that its dependencies already exist on Bluesky:

- If `in_reply_to_id` is set, the parent's Bluesky URI is deterministic: `at://{user_did}/app.bsky.feed.post/{in_reply_to_id}`. Verify it exists with a `getRecord` call (or trust the cursor: if the parent's timestamp is before the cursor, it should exist). To get the `cid` needed for the reply reference, a `getRecord` call is required unless cids are cached locally.
- If `quoted_tweet_id` is set, same pattern: the URI is predictable from the twitter_id.

Because the pump runs chronologically, **most dependencies will already be satisfied**. The main exceptions:

- **Replies to other people's tweets**: The parent is not in your corpus. Post as a standalone tweet, optionally with context like "(replying to @user)" or a link to the original.
- **Quote-tweets of external tweets**: Embed the Twitter URL as a link card instead of a Bluesky quote-post.
- **Retweets**: Proxy tweets that encapsulate the original. Recommendation: skip pure retweets initially, focus on original tweets and quote-tweets.

### 3.4 Phase 3: Publish to Bluesky

For each tweet that passes dependency check:

1. **Media**: Fetch images from Twitter's CDN or local archive's `tweet_media/` directory. Upload to Bluesky via `uploadBlob`.
2. **Build the post record**:
   - `text`: original tweet text, with `t.co` URLs replaced by expanded URLs from the entities field
   - `createdAt`: original Twitter `created_at` timestamp
   - `reply`: if replying to a tweet in the corpus, compute the parent's AT URI from its twitter_id (`at://{did}/app.bsky.feed.post/{parent_twitter_id}`) and fetch its `cid` via `getRecord`
   - `embed`: if quoting a tweet in the corpus, compute the quoted post's AT URI the same way; if quoting an external tweet, use `app.bsky.embed.external` with the Twitter URL
   - `facets`: convert Twitter entities to Bluesky's byte-range facet format for mentions, links, and hashtags
3. **Create the record** via `com.atproto.repo.createRecord`, using the `twitter_id` as the `rkey`. This makes the mapping deterministic: the Bluesky AT URI for any tweet is predictable from its Twitter ID without needing a lookup table. (See Section 3.7 for why this matters for scaling.)
4. **Advance the cursor** to this tweet's `created_at`.
5. **Rate-limit delay**: pause between posts (start at 1-2 seconds; adjust based on rate limit headers).

### 3.5 Text and Facet Handling

This is the most technically delicate part of the pump. Twitter and Bluesky represent rich text very differently, and getting the conversion wrong produces broken links, mangled mentions, or rejected posts. Most bugs in published Bluesky import tools live here.

#### What facets are

Bluesky doesn't use markup (no Markdown, no HTML). Instead, posts have a plain `text` field and a separate `facets` array: annotations that say "the bytes from position N to M should be treated as a link/mention/hashtag." This separation means the text reads cleanly and the structure is queryable independently.

A facet has two parts:

```json
{
  "index": { "byteStart": 23, "byteEnd": 35 },
  "features": [
    { "$type": "app.bsky.richtext.facet#mention", "did": "did:plc:ewvi7..." }
  ]
}
```

Three feature types currently exist:

- `app.bsky.richtext.facet#mention`: has a `did` field (the user's decentralized identifier)
- `app.bsky.richtext.facet#link`: has a `uri` field (note: `uri` not `url`)
- `app.bsky.richtext.facet#tag`: has a `tag` field (without the `#` prefix)

Facets cannot overlap. The `features` array allows multiple decorations on the same byte range (rare in practice; useful for "hashtag that's also a link").

#### The three indexing pitfalls

Bluesky's text uses three different units of measurement, and confusing them produces bugs:

1. **Byte offsets (UTF-8)**: what facet `byteStart` and `byteEnd` refer to. A two-byte character (most non-ASCII) advances the offset by 2, not 1.
2. **UTF-16 code units**: what JavaScript's `string.length` and `string[i]` return. A character outside the Basic Multilingual Plane (most emoji) takes 2 code units. **Do not use `.slice()` or `.substring()` to compute facet ranges.**
3. **Grapheme clusters**: what the 300-character post limit measures. A flag emoji is one grapheme but multiple code points and many bytes. The `👨‍❤️‍👨` family emoji is one grapheme, several code points, and 25 UTF-8 bytes.

The official `RichText` class in `@atproto/api` handles all three correctly. **Use it.** Hand-rolling byte math is the source of most facet bugs in third-party tools.

```typescript
import { RichText } from '@atproto/api'

const rt = new RichText({ text: 'Check out @atproto.com — https://example.com' })
await rt.detectFacets(agent)  // resolves @mentions to DIDs

const post = {
  $type: 'app.bsky.feed.post',
  text: rt.text,
  facets: rt.facets,
  createdAt: tweet.created_at,
}
```

`RichText` does the regex matching, byte offset calculation, grapheme counting, and (with `detectFacets`) handle-to-DID resolution in one call. The pump should not reinvent any of these.

#### Mapping Twitter entities to Bluesky facets

Twitter's archive provides pre-parsed entities. The pump translates them rather than re-detecting from scratch. The archive's spans are authoritative for what *was* a mention/link/hashtag at posting time.

| Twitter entity | Bluesky facet feature | Notes |
|---|---|---|
| `entities.user_mentions[]` | `#mention` | Requires handle → DID resolution. Skip if unresolvable; falls back to plain text. |
| `entities.urls[]` | `#link` | Use `expanded_url`, never the `t.co` short form. See Section 1.6. |
| `entities.hashtags[]` | `#tag` | Twitter stores without `#`; Bluesky `tag` feature also omits `#`. The byte range *includes* the `#` in the displayed text. |
| `entities.media[]` | (not a facet) | Handled as `app.bsky.embed.images` in the post's `embed` field, not in `facets`. |

Two implementation paths:

**Option A (recommended for prototype):** rebuild the post text with expanded URLs, then run `RichText.detectFacets()` on it. Simplest, leverages the SDK's well-tested logic. Downside: relies on regex matching that may differ slightly from what Twitter recognized as an entity.

**Option B:** use the archive's entity spans directly. Iterate over `entities.user_mentions`, `entities.urls`, `entities.hashtags`; compute UTF-8 byte offsets in the rebuilt text; construct facets manually. More faithful to the original but has to handle the byte math carefully.

Recommend starting with Option A and switching to Option B only if specific tweets render incorrectly.

#### Mention DID resolution

Bluesky mentions reference users by DID, not by handle. `@alice.bsky.social` in text needs a corresponding facet pointing at `did:plc:abc123...`. Resolution is one API call per unique handle:

```
GET /xrpc/com.atproto.identity.resolveHandle?handle=alice.bsky.social
→ { "did": "did:plc:abc123..." } | 400
```

Practical considerations:

- **Cache aggressively.** Cache resolution results in the `cid_cache` or a sibling table. The same handles appear in hundreds of tweets. A 150k-tweet backfill should not make 150k resolveHandle calls.
- **Don't block on missing handles.** If a Twitter `@username` doesn't have a Bluesky handle (no migration, or handle changed), skip the facet. The text remains as plain `@username`. Don't fail the whole post.
- **Handle != Bluesky handle.** Twitter `@alice` is not `@alice.bsky.social`. Without a known mapping, the pump should *not* guess. Treat as unresolvable and post as plain text. A future feature could let users provide an explicit Twitter→Bluesky mapping table for friends/collaborators they want to keep linked.

#### The 300-grapheme limit

Bluesky posts max out at 300 graphemes. A few classes of tweets exceed this:

- Tweets with t.co URLs that expand to long URIs (the displayed length goes from 23 to potentially hundreds)
- Old long-form tweets (Twitter expanded from 140 to 280 in 2017; some "tweetstorm" formatting fits 280 but expanded URLs push over 300)
- Tweets that combine multiple mentions, hashtags, and links

Strategies, in order of preference:

1. **Most tweets fit.** Verify with a dry-run pass during M5; the actual count of overflow tweets in UltimApe's corpus is likely small (sub-1%).
2. **For overflow: split into a self-reply chain.** The first post contains the first ~290 graphemes; the rest becomes a reply. This preserves the content as Bluesky-native text. The split should land at a sentence or word boundary, not mid-word.
3. **For irreducible overflow: append a "(cont.)" link.** If splitting would break meaning (a tweet that's one giant URL, for example), truncate and link to the original Twitter URL.
4. **Rendering as image.** Possible but breaks searchability and accessibility. Last resort.

Decision needed during M3: see Open Questions.

#### Sorting and validation

After building all facets for a post:

- **Sort by `byteStart`.** Required by spec; the appview discards out-of-order facets.
- **Check for overlaps.** If two facets overlap (rare but possible if a hashtag appears inside a URL path), keep the one with longer span and discard the other.
- **Validate byte ranges against the actual UTF-8 length.** A facet with `byteEnd` past `text.length` (UTF-8 byte length) will be silently dropped by the appview.

The `RichText` class handles all of this when used end-to-end. If you build facets manually (Option B above), explicitly sort and validate before publishing.

### 3.6 State Management and Resumability

The pump must be **resumable**: it will be interrupted (network errors, rate limits, machine reboots) and must pick up exactly where it left off.

State lives in a Supabase Postgres database. The schema is **multi-user-capable from day one**, even though the prototype only uses one user. This avoids a painful migration when the hosted service (Section 4.3) is built. The prototype enforces single-user mode at the application layer (at most one row exists in `sync_state` for the prototype's lifetime), but the schema itself doesn't care.

```sql
-- Multi-user-capable schema, used by both prototype (single user) and hosted service
create table sync_state (
  user_id uuid primary key,                      -- in prototype: a constant UUID; in hosted service: auth.users(id)
  community_archive_username text not null,
  bluesky_did text not null,
  bluesky_handle text not null,
  bluesky_pds_endpoint text default 'https://bsky.social',
  cursor_timestamp timestamptz,                  -- resume point
  retry_queue jsonb default '[]'::jsonb,         -- failed twitter_ids awaiting retry
  epoch_counts jsonb default '{}'::jsonb,        -- { "2018-01": 342, ... } for completeness checks
  status text default 'idle',                    -- idle | backfilling | syncing | paused | error
  last_run_at timestamptz,
  updated_at timestamptz default now()
);

-- Detailed failure log for debugging
create table sync_failures (
  id bigserial primary key,
  user_id uuid references sync_state(user_id),
  twitter_id text not null,
  error_message text,
  error_at timestamptz default now(),
  retry_count integer default 0,
  resolved boolean default false
);

-- Optional CID cache to avoid getRecord calls when resolving reply/quote references
create table cid_cache (
  user_id uuid references sync_state(user_id),
  twitter_id text not null,
  bluesky_cid text not null,
  cached_at timestamptz default now(),
  primary key (user_id, twitter_id)
);
```

The deterministic `rkey` (Section 3.7) means the AT URI of any synced tweet is always computable from its `twitter_id`. No mapping table needed. The optional `cid_cache` is a performance optimization to skip `getRecord` lookups when building reply/embed references for new posts.

For the prototype, the application code uses a single hardcoded `user_id` UUID and never creates a second row in `sync_state`. When the prototype is later promoted to the multi-user hosted service, the only changes needed are: enable Row Level Security with `where user_id = auth.uid()` policies, replace the hardcoded UUID with auth-derived values, and add the columns needed for the hosted service (encrypted credentials, billing, etc., shown in Section 4.3). No schema migration of existing rows.

### 3.7 Sync Efficiency (Scaling Sub-Goal)

A naive approach (checking all 150k tweets against Bluesky on every run) is wasteful and won't scale to many users. The sync strategy must minimize API calls to both Community Archive and Bluesky while storing minimal per-user data on the service.

**Core principle: the cursor is (almost) sufficient.** If Community Archive's tweet data is append-only, a single high-water-mark timestamp per user tells Hivesong everything it needs to know. One timestamp per user, essentially zero storage overhead.

**The problem cases the cursor doesn't cover:**

1. **Failed tweets**: The pump advances past a failure, and the failed tweet is behind the cursor.
2. **Retroactive additions**: A user re-uploads their archive, adding old tweets that predate the cursor.
3. **Verification**: Confirming a completed backfill is actually complete.

**Layered sync strategy (cheapest checks first):**

****Layer 1: Cursor** (per-user cost: 1 timestamp)
The happy path. Pump forward from cursor. Handles 95%+ of sync operations.

****Layer 2: Failure queue** (per-user cost: small list of IDs)
When a tweet fails, push its `twitter_id` onto a retry queue instead of advancing past it. Drain the queue first on each run. Stays small under normal operation.

****Layer 3: Epoch checksums** (per-user cost: ~120 integers for 10 years)
Divide the timeline into monthly epochs. Store a tweet count per epoch. Periodically compare against Community Archive's counts. Only re-scan epochs that differ.

****Layer 4: Full reconciliation** (expensive, rare)
Pull all tweet IDs from Community Archive. Pull all record keys from the Bluesky PDS via `com.atproto.repo.listRecords`. Diff the sets. Only needed if epoch checksums suggest problems.

**The key design decision: using `twitter_id` as the Bluesky `rkey`** eliminates the need for a per-tweet mapping table on the service side. The Bluesky PDS itself becomes the ledger of what's been synced.

```
# Creating a post with a deterministic rkey:
com.atproto.repo.createRecord({
  repo: user_did,
  collection: "app.bsky.feed.post",
  rkey: twitter_id,          # ← deterministic, derived from source
  record: { ... }
})

# Checking if a tweet has been synced:
com.atproto.repo.getRecord({
  repo: user_did,
  collection: "app.bsky.feed.post",
  rkey: twitter_id
})
# 200 = exists, 404 = needs syncing
```

**Per-user state for the hosted service:**

| Data | Size | Purpose |
|------|------|---------|
| Cursor timestamp | 1 value | Resume point for forward pump |
| Retry queue | 0-~50 IDs | Failed tweets awaiting retry |
| Epoch counts | ~120 integers | Cheap completeness verification |
| Bluesky credentials | 1 app password | Auth (encrypted at rest) |
| **Total** | **< 1 KB per user** | |

### 3.8 Live Sync Mode

After backfill completes, Hivesong switches to polling:

1. Periodically query Community Archive for tweets with `created_at > cursor` (e.g., every 5-15 minutes).
2. If new tweets are found, pump them through the same pipeline.
3. New tweets arrive in Community Archive via the browser plugin as the user posts on Twitter.

The result is a near-real-time mirror. Latency depends on poll interval and the browser plugin's sync speed.

### 3.9 Dry Run Mode

Before publishing anything, the pipeline should support a dry-run that:

- Fetches the full corpus from Community Archive
- Builds the dependency graph
- Computes publish order
- Reports statistics: total tweets, threads, orphans, quote-posts, media attachments, estimated publish time
- Identifies potential issues: tweets over 300 graphemes, unresolvable reply chains, missing media

### 3.10 Risks, Assumptions and Dependencies

A consolidated view of what could go wrong, what's being assumed, and what the project depends on. Most of these are also referenced in Section 5 (Open Questions) or Section 4 caveats; collected here for grant reviewers and prospective developers.

**Technical risks:**

- **Bluesky rate limits**: `createRecord` rate limits aren't formally documented. Empirical characterization happens in M1; if limits are tight (e.g., 100 records/minute), a 150,000-tweet backfill becomes a multi-day operation. Mitigation: batched cursor-resumable architecture from M0 onward.
- **AT Protocol evolution**: Bluesky's lexicons are still maturing. A breaking change to `app.bsky.feed.post`, `app.bsky.embed.images`, or facet structure during the build period would require pipeline updates. Mitigation: pinning `@atproto/api` version, monitoring the [AT Protocol changelog](https://atproto.com/), and using deterministic rkeys so old records can be re-derived.
- **Twitter archive format changes**: Twitter has changed its archive export format multiple times historically (which is part of why Goliath stalled). Current format is stable but not guaranteed. Mitigation: the pipeline depends on Community Archive's normalization layer when possible (CA absorbs format changes), and the local-archive ingestion path is documented as best-effort against the current export.
- **Backdated post visibility**: It's assumed (but unverified) that Bluesky's appview displays backdated posts in chronological position on the user's profile without spamming followers' Following feeds. If wrong, the user-facing experience of a 150k import becomes problematic. Empirical test happens in M2/M3 with a small batch before full backfill.
- **Long-text overflow**: Twitter's 280-character limit was generous for individual posts but Bluesky's 300-grapheme limit applies to a different unit (graphemes vs. codepoints). Some tweets that fit in Twitter's count won't fit in Bluesky's. Mitigation: open question on the splitting strategy; tested and decided during M3.

**External dependencies:**

- **Community Archive availability**: CA is a small project on Supabase. If CA goes offline, has its anon key rotated without notice, or substantially restructures its schema, the pipeline can fall back to local Twitter archive ingestion (Section 2.2) but loses the live-sync capability that depends on CA's browser-plugin updates.
- **Bluesky PDS availability**: For users on `bsky.social`, downtime of Bluesky's hosted PDS pauses the pump. The retry queue handles transient outages; extended outages require manual intervention. Self-hosted PDS deployments (Section 4.1) shift this dependency to the user's own infrastructure.
- **Twitter media CDN (`pbs.twimg.com`)**: For media not present in the local archive, the pipeline fetches images from Twitter's CDN. If those URLs decay or get rate-limited, some images won't migrate. Mitigation: prefer local archive media when available; document missing-media behavior in dry-run output.
- **Supabase availability** (for hosted-service deployments): the prototype's state can run against self-hosted Supabase, cloud Supabase free tier, or a local SQLite/JSON fallback (see Getting Started). For Phase 3 hosted SaaS, Supabase cloud uptime becomes a service-level dependency.
- **`unshrtn` (URL unshortening microservice)**: Hivesong's multi-hop URL unshortening (Section 1.6) needs a service that can recursively resolve redirect chains. The reference implementation is DocNow's [`unshrtn`](https://github.com/DocNow/unshrtn): a Node/JavaScript Docker microservice with a LevelDB-backed cache, designed to be run alongside the pump as a sidecar. The companion utility for piping tweet JSON through it is [`twarc/utils/unshrtn.py`](https://github.com/DocNow/twarc/blob/main/utils/unshrtn.py). UltimApe used this approach manually in the [Goliath workflow](https://github.com/ultimape/goliath#twarc-workflow) several years ago with mixed results. Status as of 2026: the `unshrtn` repo is lightly maintained and may have bitrotted against current Node versions. M3 includes a validation task: confirm the Docker image still builds and runs, the LevelDB cache works, and the unshortening success rate against a representative sample of UltimApe's archive URLs (10+ years of accumulated shorteners) is acceptable. The fallback if validation fails: reimplement the pattern (stateless service, persistent cache, recursive redirect-following) in TypeScript as part of Hivesong itself. Either way, the architecture is sound: this is a tooling-state question, not an architecture question. Self-hosted Phase 1 users get unshortening on by default with the option to disable; the eventual hosted SaaS (Phase 3) exposes it as a togglable feature.

**Key assumptions:**

- The user's Bluesky account exists and is functional. The pump does not create accounts; users create their own and provide credentials.
- The user has either (a) a Community Archive entry or (b) a local Twitter archive `.zip`. Without a data source, there's nothing to pump.
- Post-creation timestamps via `createdAt` are honored by Bluesky's appview for display ordering. If Bluesky changes this behavior, the migration's epistemic value (knowing when claims were made) is reduced.
- AT Protocol's account migration mechanisms remain functional. Section 4.1 (self-hosted PDS) and the backer-protection commitments in licensing depend on users being able to migrate accounts off any hypothetical hosted service.
- Most quote-tweet URLs in archives use the standard `https://twitter.com/{user}/status/{id}` format. Edge cases (mobile.twitter.com, x.com after the rebrand, archived URLs) need to be normalized.

**Legal & policy considerations:**

- The project moves the user's own public content from Twitter to Bluesky. Both Twitter's TOS and Bluesky's TOS contemplate users having access to and copies of their own posts. Hivesong isn't scraping protected content, isn't accessing other users' private data, and uses official APIs (Bluesky's `@atproto/api`, Community Archive's published anon key).
- The retweets-of-others case may be more legally murky. The current scope treats retweets the user authored as carrying the user's own timestamp (when they retweeted) and the original author's content as a quoted reference. The exact legal classification of "quoting another user's tweet on a different platform" varies by jurisdiction. Mitigation: defer this to Open Questions; in MVP scope, retweets may be filtered or simplified.
- DMs are not in scope and will not be touched even if present in a local archive zip.

---

## 4. Future Directions

These sections describe features beyond the prototype. They are out of scope for the initial build but inform architectural decisions that should be made now.

The sections below cover Twitter→Bluesky-specific extensions: self-hosting, bidirectional sync, multi-user, and cross-user remapping. **The further horizon is multi-platform**: Mastodon and ActivityPub federation, Are.na cross-references, digital garden integration, and the broader anti-rot archival layer described in Section 1.6. The `Source` interface mentioned in Section 2.2 is what makes that future possible without rewriting the pipeline. None of that work is in scope for the prototype, but the architecture is being designed so it isn't precluded.

### 4.1 Self-Hosted PDS (Core Goal)

The long-term target for Hivesong is not [`bsky.social`](https://bsky.social); it's a **self-hosted Bluesky PDS** running on UltimApe's own infrastructure (managed via a local AI agent for everyday operations). The PDS would be accessible at [`bsky.wovensoup.com`](https://bsky.wovensoup.com), and UltimApe's handle would resolve there (`@ultimape.bsky.wovensoup.com`).

Why this matters:

- **Data sovereignty**: The entire memex lives on hardware UltimApe controls.
- **Usable interface for free**: Any Bluesky client can read and interact with the data.
- **Federation**: Posts are visible to the entire AT Protocol network.
- **Full control over rate limits and storage**: No externally imposed limits.

For the prototype, [`bsky.social`](https://bsky.social) is fine: the API is identical, and posts can be migrated between PDS instances later. The pipeline's PDS endpoint should be configurable from the start (just a base URL), so switching is a config change, not a code change.

### 4.2 Bidirectional Sync

The current pipeline is one-directional: Twitter → Community Archive → Bluesky. Eventually, the reverse becomes interesting:

```
Bluesky client ──► Self-hosted PDS ──► [reverse bridge] ──► Twitter
```

This would let UltimApe compose posts in a Bluesky client and have them mirrored back to Twitter. Community Archive doesn't currently support this direction, so it would require a new bot/plugin that watches the PDS for new posts.

Out of scope for the prototype, but the state DB should track directionality (which platform a post originated on) so a future reverse bridge doesn't create infinite loops.

### 4.3 Multi-User Hosted Service

The prototype is a self-hosted CLI for a single user. But the architecture is inherently multi-user: Community Archive is a public database with many users, and the pump logic is user-agnostic. The long-term vision is to offer Hivesong as a service that anyone on Community Archive can use.

**Deployment modes:**

Because the prototype already uses Supabase, the path from single-user CLI to multi-user hosted service is a smooth gradient rather than a rewrite. The same engine runs in any of the following modes:

- **Personal** (prototype scope): Command-line tool talking to a self-hosted Supabase (Docker Compose) or cloud Supabase project. Single user, single row in `sync_state`. No web UI.

- **Self-hosted**: Same backend, plus a Next.js dashboard for status, configuration, and manual controls. Still self-hosted, still single-user (or trusted-multi-user with custom auth). Could run alongside a self-hosted Bluesky PDS on the same machine.

- **Integration**: Submit Hivesong to the Community Archive project as an official feature, potentially as a Supabase Edge Function inside CA's existing project. "Sync to Bluesky" button for users who've uploaded their archive.

- **Hosted SaaS** (hivesong.wovensoup.com or similar): Multi-user web app on Supabase cloud + Vercel. Same `sync_state` schema with `user_id` added and Row Level Security enforced. Small subscription fee to cover hosting.

**Recommended stack: Supabase + Next.js + Stripe**

- **Community Archive already runs on Supabase**: same ecosystem, potential for tighter integration.
- **Supabase is fully open source (Apache 2.0) and self-hostable via Docker.** Users who want sovereignty can self-host everything. `git clone https://github.com/supabase/supabase && cd docker && docker compose up`.
- **Supabase handles multi-tenant auth, Row Level Security, and Postgres** out of the box.
- **Next.js** provides the dashboard UI.
- **Stripe** handles subscriptions (optional; omitted for self-hosted deployments).

Reference templates:
- [`github.com/nextjs/saas-starter`](https://github.com/nextjs/saas-starter): official Next.js SaaS starter
- [`github.com/vercel/nextjs-subscription-payments`](https://github.com/vercel/nextjs-subscription-payments): archived but useful reference
- [`github.com/pclavelloux/template-nextjs-subscription-payments`](https://github.com/pclavelloux/template-nextjs-subscription-payments): maintained community fork
- Supabase self-hosting docs: [`supabase.com/docs/guides/self-hosting/docker`](https://supabase.com/docs/guides/self-hosting/docker)

**Supabase schema for per-user sync state:**

```sql
create table sync_state (
  user_id uuid references auth.users primary key,
  community_archive_username text not null,
  bluesky_did text not null,
  bluesky_pds_endpoint text default 'https://bsky.social',
  bluesky_credential_encrypted text not null,
  cursor_timestamp timestamptz,
  retry_queue jsonb default '[]'::jsonb,
  epoch_counts jsonb default '{}'::jsonb,
  status text default 'idle',  -- idle | backfilling | syncing | paused | error
  last_run_at timestamptz,
  created_at timestamptz default now()
);

alter table sync_state enable row level security;
create policy "Users manage own sync state"
  on sync_state for all
  using (auth.uid() = user_id);
```

**Multi-user architectural implications:**

- Per-user rate tracking against each user's target PDS.
- Credentials encrypted at rest (Supabase Vault or application-level encryption).
- Each sync independently pausable, resumable, and configurable.
- Clean separation between sync engine and hosting/billing layer so Community Archive could adopt it.

**Note on Twitter's response**: This tool uses only the user's own data, accessed through a public API they voluntarily contributed to. Community Archive already exists and operates openly. The AT Protocol explicitly supports backdated imports. The legal surface is minimal; you're moving your own content between platforms.

### 4.4 Cross-User Reference Remapping

**The problem**: When user A migrates their tweets, some reply to or quote user B's tweets. Since user B hasn't migrated yet, those references point to Twitter URLs. Later, when user B also uses Hivesong, user A's posts should ideally be updated to reference user B's real Bluesky posts.

**Can Bluesky posts be updated?**

At the protocol level, `putRecord` exists and the PDS accepts updates. However, Bluesky's appview **currently ignores post updates**: the UI doesn't reflect edits. Some third-party clients (like Skeets) use `putRecord` anyway. On a self-hosted PDS with a custom appview, you'd have full control.

The practical constraint: updating a post's `reply` or `embed` references changes the CID (content hash), which can cascade to downstream posts that reference the old CID.

**Recommended approach: prospective linking**

Rather than retroactively fixing old posts, handle this at pump time:

- When user A's pump encounters a reply to user B's tweet, check: "Has user B already migrated this tweet?" If yes, link to user B's Bluesky post. If no, use the Twitter URL as fallback.
- A periodic reconciliation job could later scan for Twitter URL embeds and check if those tweets have since been migrated, flagging them for optional remapping.

**Identity verification for cross-user linking:**

1. **Community Archive as trust anchor**: CA verified ownership at upload time. Sufficient for the prototype.
2. **Bidirectional linking**: User posts on Twitter with their Bluesky DID, or puts their Twitter handle in their Bluesky profile. Second factor for the hosted service.
3. **Signed attestation**: Cryptographic proof via Bluesky keypair. Strongest but requires tooling.
4. **DID registry**: If a Twitter-to-DID mapping registry emerges in the AT Protocol ecosystem, Hivesong could consume it.

**Shared index for cross-user resolution:**

```sql
create table migrated_tweets (
  twitter_id text primary key,
  bluesky_uri text not null,
  bluesky_cid text not null,
  migrated_by uuid references auth.users,
  twitter_author_id text not null,
  verified boolean default false,
  created_at timestamptz default now()
);
```

---

## 5. Open Questions

Organized by when they need to be resolved.

### Decisions for the prototype build

These need answers before or during M0-M6. The doc has tentative recommendations on some; "open" means the recommendation needs validation.

- **Bluesky character limit handling**: Strategy for tweets exceeding ~300 graphemes? Split into self-reply chain? Truncate with link to original? Post as image of text? Most tweets fit, but the long ones need a policy.
- **Mention resolution**: Should the pipeline attempt to find Bluesky DIDs for Twitter @mentions? Many people have migrated, but handles differ. Cheap fallback: leave as plain text.
- **Selective migration**: Should the pipeline support filtering (date ranges, skip replies to non-self, pattern matching)? Useful for testing, possibly useful for users who want partial migration.
- **Video media handling**: Bluesky supports video via `app.bsky.embed.video`, but at considerable additional implementation cost (size limits, transcoding, blob upload of larger files). For the prototype, three options: (1) skip videos with explicit user warning, (2) extract a thumbnail and post as static image, (3) full video upload. Decision needed during M3 or M4 once image handling is solid.

### Empirical questions (to test during build)

Things you can't know until you try.

- **Pump speed and feed visibility**: How fast can historical tweets be posted? Do backdated posts appear in followers' feeds, or are they silently archived? Test before doing the full 150k backfill (i.e., during M2 or M3, well before M5).
- **Community Archive media**: Confirm whether Community Archive stores any media URLs, or whether all media must come from the local archive / Twitter CDN.

### External dependencies to monitor

Things outside the project's control that could change.

- **Post editing on Bluesky**: The appview currently ignores `putRecord` updates. If Bluesky adds native edit support, retroactive remapping (Section 4.4) becomes simpler.
- **AT Protocol changes**: The protocol is still evolving. Watch for breaking changes in `@atproto/api`.
- **Community Archive API stability**: The CA team controls the schema and API. Major changes would require pump updates.

### Future-work questions (Section 4 territory)

Don't need answers for the prototype but inform architectural decisions.

- **Loop prevention for bidirectional sync**: The state DB should track post origin. Minimal schema for this without overengineering for an unbuilt feature?
- **Multi-user credential handling**: Bluesky app passwords must be encrypted at rest. Supabase Vault, application-level AES, or push for OAuth from Bluesky?
- **Community Archive integration strategy**: PR to the CA repo, standalone companion tool, independent service, or Supabase Edge Function inside CA's project?
- **Pricing model for hosted version**: Flat fee per backfill? Monthly subscription? Free tier with paid live sync? Tiered by archive size?
- **Pump execution model for hosted service**: Supabase Edge Functions (cheap but time-limited), Vercel Cron, or dedicated worker?

### Decisions already made (for reference)

These were open in earlier drafts. Recording them here so they don't get re-asked.

- **Retweets**: Skip pure retweets initially; focus on original tweets and quote-tweets.
- **Media sourcing**: Local Twitter archive `tweet_media/` directory primarily; Twitter CDN (`pbs.twimg.com`) as fallback.
- **Cross-user remapping**: Prospective linking only: link correctly at pump time if the referenced user is already migrated; fall back to Twitter URL otherwise. Retroactive remapping is cascade-prone and out of scope.
- **Poll interval default**: 5-15 minutes for live sync mode. Configurable via `HIVESONG_POLL_INTERVAL_SEC`.
- **Language**: TypeScript on Node.
- **State storage**: Supabase (Postgres) via `@supabase/supabase-js`. Self-hostable via Docker Compose; cloud Supabase free tier also works. Same backend as Community Archive and as the eventual hosted service.

---

## FAQ

For prospective users, backers, and curious readers. The technical sections above cover the *how*; this section covers the *what does this mean for me?*

**Will this work for my Twitter archive, not just UltimApe's?**
That's the goal. Phase 1 dogfoods on UltimApe's corpus; Phase 2 generalizes to other users' archives. The architecture is user-agnostic from day one (Section 3.6). If you've uploaded your archive to Community Archive or have your local Twitter `.zip`, you should be able to use Hivesong once Phase 2 ships. The prototype focuses on one corpus to surface the hard problems; broader applicability is the next phase.

**Will my Bluesky followers' feeds get spammed when I import 10,000 backdated posts?**
Almost certainly not. Bluesky displays a "backdated" indicator on imported posts and they generally don't appear in followers' "Following" feeds. Anyone visiting your profile will see them in chronological order. This needs empirical confirmation during M2/M3 of the build (it's an open question in Section 5), but it's how Bluesky designed the feature.

**What about my replies and quote-tweets to people who left Twitter?**
Hivesong preserves the structure regardless. If the person you replied to is also using Hivesong (or someone has migrated their archive), your reply links to their Bluesky post. If they're not, your post on Bluesky shows a Twitter URL link card pointing at the original tweet. The "conversation" is preserved as far as the available data allows. As more people migrate, more cross-references become live (see Section 4.4).

**What does it cost?**
The core tool (Hivesong itself) is and will always be free, open-source, and self-hostable. If a hosted service eventually exists (a Phase 3 possibility, not a near-term commitment), it would charge a small subscription to cover hosting costs. No price has been set; nothing is locked in.

**Can I support this project as a regular person, not a grant program?**
Yes. The formal funding drive (when it launches) will have small-pledge tiers and a dedicated channel. In the meantime, two channels are available for anyone who wants to throw something in the hat at this early stage:

- [**Patreon**](https://www.patreon.com/ultimape): ongoing monthly support for UltimApe's broader citizen-science research practice, which currently includes Hivesong.
- [**PayPal**](https://www.paypal.com/donate/?hosted_button_id=3BVQAB7VCJJ7U): one-shot donations to the same general research fund. No PayPal account required; debit/credit card payment is available on the donation page.

Honest framing: contributions to either channel go to UltimApe's general research budget rather than to a Hivesong-specific account. That's where the prototype work is currently being funded from. Money received this way helps cover the prototype work while the formal drive is set up. When the formal drive launches, it will have its own dedicated channel and tracking; until then, these are the available paths.

**What about Mastodon? Or other platforms?**
Phase 1 is Twitter → Bluesky only. Mastodon imports, ActivityPub support, and other source/target platforms are described in Section 1.6 as future scope. The architecture is built around a `Source` interface specifically so additional platforms can be added later without rewriting the pipeline.

**What if I delete my Twitter account before the tool ships?**
Save your **official Twitter/X archive `.zip`** before deleting, and keep it. This is the critical step. Twitter's archive download (Settings → Your Account → Download an archive of your data) packages the actual image files alongside the tweet metadata. Once your account is deleted, those images vanish from Twitter's CDN, and any tool that depends on `pbs.twimg.com` URLs (including Community Archive's current implementation) will return broken image references. Community Archive preserves tweet text and structure but not the image bytes themselves; the images still live on Twitter's servers, which means they're tied to your account's existence. Uploading to Community Archive is also worth doing (it preserves the textual record publicly), but it does not substitute for the local archive zip. Hivesong's local-archive ingestion path (Section 2.2) is specifically designed to read images from your archive zip and upload them as Bluesky blobs, so the images survive even if the original Twitter account is gone. **Practical advice if you're about to delete: download your archive zip, store it somewhere durable, and only then delete the account.**

**Will the migrated tweets be exact duplicates of what's on Twitter?**
Structurally yes; visually close. Threading, timestamps, quote-tweet relationships, mentions, links, hashtags, and images are preserved. Bluesky uses a different rich-text format (facets) than Twitter's entities, so the underlying data is different but renders equivalently. Videos are an open question for the prototype (Section 5). Some edge cases (like extremely long tweets exceeding Bluesky's 300-grapheme limit) need a policy decision during the build.

**Can Hivesong see my private DMs or protected tweets?**
No. Hivesong reads from Community Archive (a public database of *publicly visible* tweets that users have voluntarily uploaded) and from your own Twitter archive `.zip` if you choose to provide it. Twitter's archive includes your DMs, but Hivesong ignores that data entirely; only the public tweet stream is processed. There is no Twitter login involved on Hivesong's side, no OAuth, no scraping of protected timelines. If a tweet wasn't public on Twitter, it doesn't reach Bluesky through Hivesong.

**What does Hivesong store about me?**
For the self-hosted version, Hivesong stores its sync state on infrastructure you control: a Supabase instance you run yourself or a Supabase project you own. The state is operational metadata only: your Twitter username, your Bluesky DID and handle, a cursor showing where the sync left off, a queue of tweets that failed to publish (with error messages), and an optional cache of Bluesky CIDs to avoid redundant API calls. Your tweet content lives on Bluesky, not in Hivesong's state DB. If a hosted version is ever offered, the same data plus encrypted Bluesky credentials would be stored on the hosting provider's Supabase, with credential encryption at rest as a baseline.

**What happens to the project if UltimApe gets hit by a bus?**
The code is open-source under MIT/Apache. Anyone can fork it, run it, or take it over. The state schema and architecture are documented. The exit-ramp clauses in the licensing section are designed precisely to ensure the project survives any single contributor.

**Why "Hivesong"?**
A hive's song is the collective hum of a colony at work: thousands of individual bees, each pursuing their own task, producing together a steady resonance that only makes sense as a whole. No single bee carries the song; it's what emerges when their movements coordinate. A Twitter archive is similar. Individual tweets look like isolated thoughts; the shape only becomes visible when the relationships between them are preserved. The colony is the conversation; the song is the thinking.

---

## References and Resources

- Bluesky post creation tutorial: [`docs.bsky.app/docs/tutorials/creating-a-post`](https://docs.bsky.app/docs/tutorials/creating-a-post)
- Bluesky timestamp docs (backdating): [`docs.bsky.app/docs/advanced-guides/timestamps`](https://docs.bsky.app/docs/advanced-guides/timestamps)
- Bluesky rich text & facets: [`docs.bsky.app/docs/advanced-guides/post-richtext`](https://docs.bsky.app/docs/advanced-guides/post-richtext)
- Bluesky posts guide: [`docs.bsky.app/docs/advanced-guides/posts`](https://docs.bsky.app/docs/advanced-guides/posts)
- AT Protocol API reference: [`docs.bsky.app/docs/get-started`](https://docs.bsky.app/docs/get-started)
- Community Archive repo: [`github.com/TheExGenesis/community-archive`](https://github.com/TheExGenesis/community-archive)
- Community Archive analysis notebook: [`github.com/DefenderOfBasic/twitter-archive-toolkit`](https://github.com/DefenderOfBasic/twitter-archive-toolkit)
- Goliath BirdEater (prior art, incomplete, does not work with current export format): [`github.com/ultimape/goliath`](https://github.com/ultimape/goliath)
- Gwern's archiver-bot (prior art, proactive URL archiving): [`github.com/gwern/archiver-bot`](https://github.com/gwern/archiver-bot)
- Gwern on archiving URLs: [`gwern.net/archiving`](https://gwern.net/archiving)
- Internet Archive Save Page Now API: [`web.archive.org/save/`](https://web.archive.org/save/)
- Are.na API (V3): [`are.na/developers/explore`](https://are.na/developers/explore)
- UltimApe's digital garden: [`garden.wovensoup.com`](https://garden.wovensoup.com)
- UltimApe's Are.na thread index: [`are.na/ultimape/root-tweets-toots`](https://are.na/ultimape/root-tweets-toots)
- UltimApe's murmurology research channel: [`are.na/ultimape/murmurology`](https://are.na/ultimape/murmurology)
- UltimApe on Community Archive: [`community-archive.org/user/ultimape`](https://community-archive.org/user/ultimape)
- UltimApe's Bluesky: `@ultimape.bsky.wovensoup.com`
- Future project site: [`hivesong.wovensoup.com`](https://hivesong.wovensoup.com)
- GitHub repository: [`github.com/ultimape/hivesong`](https://github.com/ultimape/hivesong) (currently private)
- Supabase: [`supabase.com`](https://supabase.com)
- Supabase self-hosting: [`supabase.com/docs/guides/self-hosting/docker`](https://supabase.com/docs/guides/self-hosting/docker)
- Next.js SaaS starter: [`github.com/nextjs/saas-starter`](https://github.com/nextjs/saas-starter)
- Vercel subscription payments (archived, reference): [`github.com/vercel/nextjs-subscription-payments`](https://github.com/vercel/nextjs-subscription-payments)

---

## Further Reading

Conceptual foundations and related work that informed Hivesong's design.

**On owning your data:**

- **"Local-First Software"**: Ink & Switch (2019). The essay that articulates why software should work on your device, sync without servers, and survive the death of any cloud service. Hivesong's self-hosted PDS goal is a direct application of local-first principles. [`inkandswitch.com/essay/local-first/`](https://inkandswitch.com/essay/local-first/)

- **"Sad State of Personal Data Infrastructure"**: karlicoss / beepb00p. A diagnosis of why personal data is fragmented across services and why the tooling to unify it barely exists. Hivesong is one answer to the problem described here. [`beepb00p.xyz/sad-infra.html`](https://beepb00p.xyz/sad-infra.html)

- **Human Programming Interface (HPI)**: karlicoss / beepb00p. A project to unify all personal data exports into a single queryable Python interface. The closest existing project to Hivesong's long-term multi-platform archival vision. [`beepb00p.xyz/hpi.html`](https://beepb00p.xyz/hpi.html)

- **"Building a Personal Data Warehouse"**: Simon Willison (2020). The creator of Datasette on using SQLite as a personal analytics backend. The Dogsheep project (twitter-to-sqlite, etc.) applies this to Twitter data specifically. Same philosophical lineage as Hivesong. [`simonwillison.net/2020/Nov/14/personal-data-warehouses/`](https://simonwillison.net/2020/Nov/14/personal-data-warehouses/)

- **"Don't Ask If Artificial Intelligence Is Good or Fair, Ask How It Shifts Power"** and other beepb00p essays on data exports and unnecessary databases. Practical thinking about when and how to extract your data from services, and what to do with it once you have it. [`beepb00p.xyz/exports.html`](https://beepb00p.xyz/exports.html) · [`beepb00p.xyz/unnecessary-db.html`](https://beepb00p.xyz/unnecessary-db.html)

**On threading as a medium of thought:**

- **"The Liminist"**: Alex Singh. A definition of the thinker who specializes in connections between domains rather than within them. "The evidence of their work exists at the intersections between objects." Describes the kind of person whose memex is most damaged by a flat export. [alexsingh.com](https://www.alexsingh.com/)

- **Ted Nelson**: Inventor of hypertext, originator of Project Xanadu. Nelson's lifelong argument that documents are not paper simulations but packages of interconnected ideas is the philosophical foundation of Hivesong. His concept of "intertwingularity" (that everything is deeply interconnected) describes exactly what a memex preserves that a flat archive destroys. Key works: *Computer Lib/Dream Machines* (1974), *Literary Machines* (1981). See also Bret Victor's overview: [`worrydream.com/Engelbart/`](https://worrydream.com/Engelbart/)

- **"Spreading, Threading"**: Aaron Z. Lewis (2019). An analysis of Twitter threading as a distinct compositional form, not just a workaround for the character limit, but a medium that shapes how ideas develop in public. Validates Hivesong's premise that thread structure is *content*, not decoration. [`aaronzlewis.com/blog/2019/05/01/spreading-threading/`](https://aaronzlewis.com/blog/2019/05/01/spreading-threading/)

- **"A Word Game to Communicate in Any Language"**: Ajit Narayanan (TED). On representing meaning as a map of linked question-answer pairs rather than a linear sequence of words. "Meaning is really the underbelly of language. It's what comes after thought but before language." Directly supports why a graph of tweets conveys meaning that a linear export cannot. [`ted.com/talks/ajit_narayanan_a_word_game_to_communicate_in_any_language`](https://www.ted.com/talks/ajit_narayanan_a_word_game_to_communicate_in_any_language/transcript#t-286153)

- **"The Rhetoric of the Hyperlink"**: Ribbonfarm (2009). On how hypertext makes the reader an "extraordinarily active meaning-constructor" who builds their own text through their click-trail. UltimApe's word-association navigation is exactly this: meaning constructed through paths, not pages. [`ribbonfarm.com/2009/07/01/the-rhetoric-of-the-hyperlink/`](https://www.ribbonfarm.com/2009/07/01/the-rhetoric-of-the-hyperlink/)

- **"Where Good Ideas Come From" / "The Slow Hunch"**: Steven Johnson (TED talk, 2010; book, 2010). Ideas aren't epiphanies; they're slow accumulations across months and years, emerging when hunches from different periods finally connect. Darwin thought his theory of natural selection arrived in a flash, but his notebooks show the idea forming months earlier. A tweet archive is where hunches incubate; destroying the timeline destroys the incubation record. This is the temporal argument for why timestamps are epistemic evidence (Section 1.2).

- **Darwin's reading method**: Via JASNA, based on Darwin's *Recollections*. Darwin maintained personal indexes at the back of every book, compiled classified indexes across his entire library, and broke books apart to use as working tools. Howard Gruber called this "a man at work using books as tools for getting knowledge, not as exhibitions of knowledge already crystallised." The same method applied to tweets is the cognitive process that Hivesong preserves. [`jasna.org/persuasions/on-line/vol30no2/bankes.html`](https://www.jasna.org/persuasions/on-line/vol30no2/bankes.html)

- **Dostoyevsky on note-taking**: *Notes From Underground* (1864). "I do not want to hamper myself with anything in preparing my notes. I will not introduce any order or system. Whatever I recall, I will write down." A 19th-century literary articulation of stigmergic, ontologically-soft thinking. The same approach UltimApe takes with tweets. The Underground Man's narrative method is itself a kind of memex.

- **Conor White-Sullivan on Twitter as collaborative memex**: Founder of [Roam Research](https://roamresearch.com), a memex tool. His [May 2019 thread](https://x.com/Conaw/status/1129105485175480321): "Twitter as a company is culturally oriented around NOW... The Twitter users who search their own and others' timelines for building webs of threads are hacking the platform to build a collaborative Memex (memory extension). Eventually those users will migrate." Third-party validation that the "Twitter as memex" pattern is widely recognized, and a prediction Hivesong is now helping fulfill.

- **"Augmenting Human Intellect"**: Bret Victor on Doug Engelbart. The original vision of computers as tools for thought: the intellectual ancestor of the memex concept. [`worrydream.com/Engelbart/`](https://worrydream.com/Engelbart/)

**On the IndieWeb and data portability:**

- **"COPE: Create Once, Publish Everywhere"**: NPR / Daniel Jacobson (2009). NPR's content strategy of separating content from platform-specific display, enabling the same material to be published to websites, RSS, iPods, mobile, and TV. Hivesong does the same thing: content created on Twitter, published to Bluesky with platform-appropriate formatting (Twitter entities → Bluesky facets). The principle that content must be free of platform-specific markup is exactly what the facet conversion accomplishes. [`npr.org/sections/inside/2009/02/clean_content_portable_content.html`](https://www.npr.org/sections/inside/2009/02/clean_content_portable_content.html)

- **"Right to an API Key" / "Right to Be Represented by a Bot"**: Albert Wenger / Continuations (2014-2015). A proposal that individuals should have a legal right to full read/write API access to their own data: "an individual right to be represented by an algorithm." The extended talk includes a telling example: you can't even search your own Facebook wall posts, because the platform decided not to make it easy. A bot acting on your behalf could keep your own archive in your own data store. "It completely inverts the power relationship between networks and their participants." Community Archive is a grassroots implementation of this principle. Hivesong is the algorithm that exercises it. [`continuations.com/post/96355016855/labor-day-right-to-an-api-key-algorithmic`](https://continuations.com/post/96355016855/labor-day-right-to-an-api-key-algorithmic) · [Talk transcript](https://continuations.com/post/108912689660/big-and-bot-policy-proposals-transcript)

- **Aaron Parecki, "Twitter via IndieWeb tools"** (2019). A demonstration of POSSE (Publish on your Own Site, Syndicate Elsewhere) in practice: having a full Twitter conversation without ever visiting Twitter. Hivesong does reverse-POSSE: pulling content *back* from the platform to a self-hosted home. [`aaronparecki.com/2019/02/20/32/twitter`](https://aaronparecki.com/2019/02/20/32/twitter)

- **"Delete Never: The Digital Hoarders Who Collect Tumblrs"**: Gizmodo (2019). People doing for Tumblr what Hivesong does for Twitter: rescuing intellectual work from a platform that could disappear. Shows the need is universal across platforms, not a personal quirk. [`gizmodo.com/delete-never-the-digital-hoarders-who-collect-tumblrs-1832900423`](https://gizmodo.com/delete-never-the-digital-hoarders-who-collect-tumblrs-1832900423)

**On web archiving and link rot:**

- **"Archiving URLs"**: Gwern Branwen. A comprehensive strategy for fighting link rot using a combination of local downloads, Internet Archive submissions, and cryptographic timestamping. Hivesong's anti-rot strategy (Section 1.6) draws from this approach. [`gwern.net/archiving`](https://gwern.net/archiving)

- **Stewart Brand on monk feces** (via Gwern, *The Clock of the Long Now*). A group of monks carved scriptures into stone for posterity, but posterity would value a preserved collection of their feces far more, for what it reveals about global warming and countless other phenomena. The point: the metadata, connections, and context surrounding content may be more valuable than the content itself. Supports the argument that preserving the graph of a tweet archive matters more than preserving the text. [`gwern.net/About#long-site`](http://www.gwern.net/About#long-site)

- **"The Evolving Role of the Wandering Scholar"**: ACM Ubiquity. On how marginal notes and wandering scholars were the original mechanism for knowledge dissemination before print, and how the internet is recreating that pattern. Memes are the return of cat butts drawn in the margins. [`ubiquity.acm.org/article.cfm?id=348784`](https://ubiquity.acm.org/article.cfm?id=348784)

- **"Memory Sites"**: Ed Summers / inkdroid (2018). Reflections on web archiving practices by a co-creator of twarc (the now-deprecated Twitter archiving tool). On the relationship between memory, infrastructure, and the durability of digital records. [`inkdroid.org/2018/06/10/memory-sites/`](https://inkdroid.org/2018/06/10/memory-sites/)

**On legal precedent for data access:**

- **hiQ v. LinkedIn scraping ruling** (2017). A judge ruled that scraping publicly available data does not violate the Computer Fraud and Abuse Act. Relevant to the "Note on Twitter's response": accessing public data you voluntarily contributed to a public archive is legally defensible. [`blog.webhose.io/2017/08/17/a-judge-just-ordered-linkedin-to-allow-scraping-heres-why/`](https://blog.webhose.io/2017/08/17/a-judge-just-ordered-linkedin-to-allow-scraping-heres-why/)

**Academic:**

- **WWW 2013 paper (p1273)**: Academic paper from the 22nd International World Wide Web Conference on social media data practices. [`www2013.w3c.br/proceedings/p1273.pdf`](https://www2013.w3c.br/proceedings/p1273.pdf)

- **"What You Found in 3 Million Russian Troll Tweets"**: FiveThirtyEight. Analysis of a large-scale tweet dataset, demonstrating the analytical value of preserved tweet corpora with full metadata. [`fivethirtyeight.com/features/what-you-found-in-3-million-russian-troll-tweets/`](https://fivethirtyeight.com/features/what-you-found-in-3-million-russian-troll-tweets/)
