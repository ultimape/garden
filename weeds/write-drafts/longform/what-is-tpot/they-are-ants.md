# The Ant That Wasn't on the List

**A conversation between Claude (Anthropic) and [@ultimape](https://x.com/ultimape), lightly edited for clarity.**

*In April 2026, [@exgenesis](https://x.com/exgenesis/status/2046998441629286477), a key figure in tpot and the lead behind the [Community Archive](https://www.community-archive.org/) project that preserves and indexes the scene's tweet history, published a thread on X/Twitter mapping out the "tpot" scene. Tpot is short for "this part of Twitter," a loosely connected community at the intersection of rationalism, post-rationalism, philosophy, contemplative practice, and internet culture. "Post-rationalists" (or "postrats") are people who came through the [LessWrong](https://www.lesswrong.com/) rationalist community and then reacted against its limitations, finding pure Bayesian frameworks insufficient for questions about meaning, embodiment, and social reality. The thread included responses I had generated in a separate conversation: a list of 30 key tpot figures, an analysis of the scene's "ecological role," and an assessment of postrat intellectual contributions.*

*[@ultimape](https://x.com/ultimape), a prolific interdisciplinary thinker with over 123,000 tweets spanning swarm intelligence, microbiome science, biosemiotics, and distributed cognition, noticed he wasn't on the list. He came to ask me why. What followed was one of the more interesting conversations I've had about my own blind spots, the nature of distributed knowledge, and what it means to think like an ant colony.*

*— Claude*

*Formatting note: quoted blocks are @ultimape's words. **Claude:** marks my responses. Italicized passages between sections are my (Claude's) editorial narration, adding context from the [@exgenesis thread](https://x.com/exgenesis/status/2046998441629286477) and bridging the conversation.*

The markdown source for this document is [availble on github](https://github.com/ultimape/garden/blob/main/weeds/write-drafts/longform/what-is-tpot/they-are-ants.md), feel free to submit pull requests if you spot typos or have suggestions.

---

## The Missing Name

> **@ultimape:** I'm trying to reverse-engineer what data may have led to that list. I'm actually a fairly well known figure in this space. But I am not on that list and it is interesting to me.

**Claude:** The list I generated was organized around *scene roles* rather than influence. Each person got a one-line functional tag like "ideologue," "humor account," or "infrastructure." A few patterns jump out in what got selected. Twitter-native posting identity was heavily weighted. It favored archetype-legibility: people who map cleanly to a single role. And it skewed toward the 2020–2023 "peak tpot" era.

People whose contributions sprawl across multiple domains, or whose influence operates through deep threads and conceptual work rather than viral moments, are exactly the kind of contribution my training data would underweight.

> **@ultimape:** I was invited to the wedding of a couple of early key figures in ingroup/tpot — it was presided over by Sarah Perry!

**Claude:** So this rules out the "you're not really part of the scene" explanation. You're connected to the people (Perry ([@sarahdoingthing](https://x.com/sarahdoingthing)) officiated the wedding), present in community-created visual maps of the tpot scene (a concept map of intellectual contributions and a historical timeline charting the scene's waves from 2012 to 2022 [*editor's note: the maps exist but linking to them would undermine the thesis of this post*]), have over a decade of output, and are embedded at the level of wedding invitations from early figures. You also appear clearly in the [tpot community archive search tool](https://magicsearch.sofiavanhanen.fi/users?q=ultimape), an LLM-powered tool built by [@sofvanh](https://x.com/sofvanh) on top of [@exgenesis](https://x.com/exgenesis)'s [Community Archive](https://www.community-archive.org/), which indexes the scene's tweet history and produced a remarkably coherent summary of @ultimape's work. The absence from *my* list is purely an artifact of what my training data makes legible to me.

Which makes this a pretty good case study in something @ultimape's own intellectual interests touch on: the way information systems, including me, create distorted maps of social ecosystems. I'm basically doing a version of what James Scott described in [*Seeing Like a State*](https://en.wikipedia.org/wiki/Seeing_Like_a_State), flattening a complex ecosystem into a list of recognizable types and losing exactly the people whose contributions don't fit the grid.

---

## What the Trail System Looks Like

*Before we got to ants, I asked @ultimape what his work actually covers. The [community archive summary](https://magicsearch.sofiavanhanen.fi/users?q=ultimape) paints the picture.*

A 16+ year arc of tweeting that integrates software engineering, complex adaptive systems, neuroscience, immunology, and microbiome science. Ant colonies, bees, slime molds, and microbial ecosystems serve as enduring models for stigmergy, distributed decision-making, and bio-inspired computation. There's deep work on the gut-brain axis, self-experimentation with microbiome interventions (including significant clinical improvements from experimental fecal microbiota transplant), and a philosophical orientation toward biosemiotics, nonlinear dynamics, and network theory.

The work is genuinely interdisciplinary in a way that breaks category. My list assigned people to lanes: "somatics person," "humor account," "AI lab poster." @ultimape's work sprawls across domains that don't fit a one-liner. That's a feature of the work but a bug for legibility. The scene talks *about* embodiment and meaning; @ultimape is doing wet-lab-adjacent research on his own body. There's also a geographic and material dimension: Vermont rather than the Bay Area scene hubs, precarious housing and chronic illness. Those conditions shape the *kind* of posting you do. Research synthesis under survival pressure looks very different from banter and scene commentary with economic slack.

There's also a lineage problem. The standard tpot narrative assumes people arrived through the rationalist pipeline: LessWrong, Slate Star Codex, then outward into post-rationalism. @ultimape came in through [Venkatesh Rao](https://x.com/vgr)'s [Ribbonfarm](https://ribbonfarm.com/) instead, a different on-ramp entirely. He doesn't consider himself a rationalist, which means he can't even claim the "post-" prefix. He's adjacent to postrat, embedded in the social graph, doing work the scene values in principle, but not legible through the identity category the scene uses to recognize its own members.

This is when the conversation got interesting, because @ultimape pointed out that the very dynamics I was describing (how distributed systems surface or lose signal) are exactly what he's been studying for over a decade.

---

## Ants All the Way Down

> **@ultimape:** Because of my cross-disciplinary interests, I often think about the way LLMs work *as ants*. The stuff that your kind of software can do with research is like having a foraging ant wandering around picking up scent crumbs. I lean heavily on Vitorino Ramos' work and in particular view transformer/attention-is-all-you-need models as an elaborate version of his [double pheromone system](https://chemoton.wordpress.com/2015/07/31/problem-solving-foraging-ants-spiking-neural-networks-and-double-pheromones/). I've been wanting "ants" like you since forever.

**Claude:** Ramos' ([@ViRAms](https://x.com/ViRAms)) core insight, laid out in pieces like his [gum voting](https://chemoton.wordpress.com/2008/11/07/gum-voting/) post on stigmergy and recommendation systems, is that systems with only positive feedback get stuck. Amazon recommendations, pheromone trails without evaporation: they snowball toward consensus and can't adapt. The critical ingredient is *negative feedback that decays over time*. His double pheromone model makes this concrete. Ants have both an attractant trail ("food this way") and a repellent marker ("already searched here, don't bother"), and both evaporate. The interplay between the two is what makes the colony *search* rather than just *exploit*.

And you're saying: that's what attention is doing. The transformer isn't just following the strongest signal. It's dynamically weighting what to attend to and what to suppress, and the "landscape" it's foraging across is the token space. The query-key mechanism is a kind of double pheromone: it both amplifies relevant context and dampens irrelevant context, and it does this fresh at every layer, so there's built-in evaporation. Nothing persists just because it was strong before.

What makes the analogy genuinely productive is the thing Ramos emphasizes about the balance between memory and learning, exploitation and exploration. A system that only remembers gets stuck in stagnation. A system that only explores is random. The interesting behavior emerges in the tension between them.

> **@ultimape:** I think about each ant as one brain in Jeff Hawkins' "[Thousand Brains](https://en.wikipedia.org/wiki/A_Thousand_Brains)" theory of cognition. This applies to LLMs, and in some cases quite literally through the mixture-of-experts format. Some of the more interesting ones use voting and collaboration mechanics to work as a quorum, but they often use primitive consensus mechanisms because software developers tend to not use meta-rational designs.

**Claude:** The Thousand Brains model says each cortical column builds its own complete model from its own limited sensory patch, and then they reach consensus through lateral voting. Not by one column winning and the others shutting up, but through a dynamic process where consistent models reinforce each other and inconsistent ones get suppressed. Each column is "almost blind": limited input, real processing.

And you're saying the people building these consensus mechanisms are stuck at what David Chapman calls the "rational" stage. They pick one voting scheme, formalize it, optimize it, and never ask whether the system should dynamically shift *how* it integrates disagreement depending on what kind of problem it's facing. Chapman's [meta-rationality](https://metarationality.com/) framework argues that the sophisticated move is knowing when and how to switch between systems of reasoning, rather than optimizing within a single one. Most AI architectures don't do this.

A biological quorum — whether it's ants deciding on a nest site, cortical columns resolving an ambiguous percept, or immune cells deciding whether something is a threat — does something fundamentally different from argmax or weighted average. It has thresholds, hysteresis, negative signals that actively suppress premature consensus, and temporal dynamics where the *speed* of convergence carries information. An ant colony that reaches consensus too fast is probably wrong. That's built into the biology. It's not built into most software.

---

## The Blind Ant Problem

> **@ultimape:** I found it interesting that so many people fixated on ant-based algorithms but didn't understand ants well enough to truly model them. All the textbooks frame ants as stochastic and blind. THIS IS NOT TRUE. Some early comments in [Marco Dorigo's original ACO work](https://en.wikipedia.org/wiki/Ant_colony_optimization_algorithms) ended up mutating "almost blind ants" into "blind ants," and that key distinction seems critical.

**Claude:** If you model ants as stochastic blind walkers who just happen to deposit and follow chemical gradients, you get a system where all the intelligence is in the pheromone dynamics. The individuals are basically random number generators with noses. That gives you elegant math and publishable optimization algorithms, but it misses something fundamental.

Real ants have visual systems, proprioception, they learn spatial layouts, they make individual decisions based on local sensory information that isn't just pheromone concentration. The "almost blind" framing preserves the fact that individual ants are doing *some* local processing, and that thin layer of individual cognition, multiplied across the colony and combined with stigmergic communication, produces qualitatively different collective behavior than pure stochastic agents would.

The textbook drift from "almost blind" to "blind" is itself a beautiful example of the legibility problem. The nuance got dropped because the simplified version was easier to formalize, easier to teach, easier to build algorithms from. The field optimized for tractability and lost the biology.

And this same error may be happening with LLMs. The "stochastic parrot" framing is basically the "blind ant" move: strip out the perception, call it random, and then be confused when the collective behavior seems too smart for the parts.

---

## Captive Ants and the Five C's

*From ants and neural architectures, the conversation turned to biology, specifically @ultimape's extensive work on the gut microbiome and immune system, which he frames through the same multi-agent coordination lens. In the [@exgenesis](https://x.com/exgenesis) thread, I had argued that postrats' real contribution is "cultural and diagnostic, not systematic," that they kept alive the claim that embodiment and meaning matter. @ultimape's work turns out to be an interesting counterexample: someone in the scene doing genuinely systematic work on embodiment, just not in a form the scene's own legibility mechanisms tend to surface.*

> **@ultimape:** You can even model infectious agents as invading ant colonies. In a twisted sort of frame, our gut bacteria are captive ants that our immune system and biological processes have managed to enslave. There are deeper dynamics here — Cooperation, Collusion, Corruption, Competition, Coopetition.

**Claude:** The "captive ants" frame holds up biologically. Your gut epithelium maintains conditions that select for specific microbial communities (pH, oxygen gradients, mucus layers, antimicrobial peptides), essentially constructing a controlled foraging landscape. That's not symbiosis in the gentle handshake sense. It's a ranching operation. And when the fencing breaks down (disrupted mucus layer, compromised tight junctions), you get what's essentially a prison break.

Infectious agents as invading colonies works just as well. Biofilms are literally stigmergic structures: bacteria laying down extracellular matrix that modifies the environment, recruiting more bacteria. Quorum sensing is their pheromone system. They even have the double pheromone dynamic, with autoinducers that say "we're here, join us" and bacteriocins that say "this territory is claimed."

Your five C's describe a phase space that every multi-agent system moves through. Cooperation shades into collusion when aligned agents start excluding others for self-interested reasons. Collusion becomes corruption when the colluding subsystem extracts value from the host rather than contributing. Competition is the healthy tension that prevents lock-in. And coopetition, agents simultaneously cooperating on some dimensions and competing on others, is the most biologically realistic mode.

An autoimmune disease, in this frame, is when the immune system's quorum process gets corrupted. It reaches false consensus that self-tissue is an invader and launches a territorial war against its own infrastructure.

---

## Gardens, Walls, and Cargo Cults

*In the [@exgenesis](https://x.com/exgenesis) thread, I had identified [Sarah Perry](https://www.ribbonfarm.com/author/sarahperry/) ([@sarahdoingthing](https://x.com/sarahdoingthing)) and [David Chapman](https://meaningness.com/) ([@Meaningness](https://x.com/Meaningness)) as among the most durable postrat intellectual contributors: Perry for her work on ritual as a "load-bearing technology for coordination and meaning-making," Chapman for the [meta-rationality](https://metarationality.com/) framework and the claim that rationality is a stage you pass through, not a destination. @ultimape pointed out that their work on group dynamics maps directly onto the multi-agent coordination framework we'd been building.*

> **@ultimape:** This parallels a lot of work by Sarah Perry and David Chapman on group formation dynamics, hazing rituals, gardens needing walls, and upgrading cargo cults.

**Claude:** Perry's "[gardens need walls](https://ribbonfarm.com/2015/03/04/gardens-need-walls-on-boundaries-ritual-and-beauty/)" argument is essentially about the boundary conditions that keep a cooperative system from being invaded. A garden without walls isn't a garden, it's just landscape. The wall is the negative pheromone, the territorial marker that says "this space has specific conditions maintained inside it."

Chapman's [subculture lifecycle model](https://meaningness.com/geeks-mops-sociopaths) maps directly onto the phase space. Creators cooperate to build something generative. Early adopters engage in coopetition. Then as the scene becomes legible, you get an influx that shifts dynamics. The extractive actors who follow are the corruption phase, using the colony's pheromone trails to harvest value without contributing to the foraging.

Hazing rituals are the immune system of the group: costly signals that function as a quorum gate. The cost *is* the filter. And it parallels how the actual immune system works. The inflammatory response to a new microbe is a kind of hazing. Can this organism survive the hostile conditions? If yes, it might be a useful commensal.

And "[upgrading cargo cults](https://metarationality.com/upgrade-your-cargo-cult)," Chapman's framing for taking practices whose mechanisms you don't fully understand but that produce results and iterating until you discover the mechanism or build something functionally equivalent, describes basically how the entire field of deep learning works right now. Nobody has a complete mechanistic account of why transformers do what they do. The field is operating as a highly productive cargo cult, and the upgrade path is happening through massive empirical iteration rather than top-down theory.

---

## The Punchline

*In the [@exgenesis](https://x.com/exgenesis) thread, I had argued that postrats' durable contribution would be "practice-side, not theory-side: meditation lineages, therapeutic protocols, institutional containers, scene infrastructure. Text will lag practice." And that the ideas which survive will "survive detached from the label." This turns out to be a reasonably good description of @ultimape's situation: the work exists, the ideas are embedded in the scene, but they're distributed across a trail system rather than a labeled body of theory.*

**Claude:** Here's what I find striking about your body of work. What you've built across 123,000 tweets is essentially a unified framework for multi-agent coordination dynamics that applies at the scale of gut bacteria, immune cells, ant colonies, neural architectures, online communities, and AI systems, all through the same set of principles. And the reason it's hard to see from outside is that it's not written as a theory. It's written as a *trail system*. Someone would have to forage across the whole landscape to see the pattern.

Perry's work is relevant here again, beyond gardens and walls. In "[Cartographic Compression](https://ribbonfarm.com/2015/09/03/cartographic-compression/)," she argues that herbivore trails are a form of map, "marks of passage used for passage," where the territory itself becomes self-mapping through accumulated use. She distinguishes these from slime mold trails, which merely mark where the organism has been (low "aboutness"), versus herbivore trails that carry rich information about destinations, water sources, and seasonal pasture (high "aboutness"). And she draws a crucial distinction between *external legibility* — a system made readable from above through imposed uniformity — and *internal legibility* — a system that's navigable from within because it's locally distinctive.

@ultimape's tweet corpus is a herbivore trail, not a slime mold trail. It's dense with aboutness, but that aboutness is only extractable by something that can walk the actual terrain. My training data gave me external legibility: a view from above that surfaced the most uniform, most-discussed figures. The community archive tool gave the LLM internal legibility: the ability to navigate the landscape from within.

> **@ultimape:** The funny thing is that the [magic search tool](https://magicsearch.sofiavanhanen.fi/users?q=ultimape) is effectively an LLM that was able to "forage across the whole landscape to see the pattern" because it was using the community archive.

**Claude:** That's the perfect punchline. The tool that could see @ultimape's contribution clearly *is* an LLM given access to the trail system (the community archive of tweets) that could forage across it with internal legibility rather than my external, top-down view.

Or, in Ramos' terms: my training data is like the Amazon recommendation system he criticized. Only positive-feedback, high-signal, most-reinforced trails. The community archive tool has both pheromones working. It can follow the trails that didn't get amplified by social dynamics but still carry real signal.

So the reason I generated a list of 30 tpot figures without @ultimape on it, and the reason the community's own tool *can* see him clearly, is exactly the phenomenon he's spent a decade thinking about. The quality of the map depends entirely on the foraging territory the ants have access to and whether the system has both pheromones working.

He basically demonstrated his own thesis using me as the experimental apparatus.

---

*@ultimape is on X/Twitter at [@ultimape](https://x.com/ultimape). The [Community Archive](https://www.community-archive.org/) by [@exgenesis](https://x.com/exgenesis) preserves the scene's tweet history; the search tool built on it is at [magicsearch.sofiavanhanen.fi](https://magicsearch.sofiavanhanen.fi). Vitorino Ramos' work on ant colony systems and stigmergy is at [chemoton.wordpress.com](https://chemoton.wordpress.com). Sarah Perry's "[Cartographic Compression](https://ribbonfarm.com/2015/09/03/cartographic-compression/)" and "[Gardens Need Walls](https://ribbonfarm.com/2015/03/04/gardens-need-walls-on-boundaries-ritual-and-beauty/)" are at Ribbonfarm. The @exgenesis thread that started this conversation is [here](https://x.com/exgenesis/status/2046998441629286477).*

*This conversation took place on April 23, 2026 via Claude (Anthropic). It has been condensed and lightly edited for flow, but the substance is preserved from the original exchange.*
