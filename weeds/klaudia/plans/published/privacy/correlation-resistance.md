---
title: "Correlation Resistance and Context-Aware Disclosure"
meta-author: UltimApe
meta-description: A design report on the SAINT/STAIN privacy architecture for memory-bearing AI agents — correlation resistance, context-aware disclosure, and the independent enforcement gate.
meta-og:title: Correlation Resistance and Context-Aware Disclosure
meta-og:description: A design report on the SAINT/STAIN privacy architecture for memory-bearing AI agents.
meta-og:type: article
meta-og:locale: en_US
meta-og:site_name: "UltimApe's digital garden"
meta-twitter:card: summary
meta-twitter:site: "@ultimape"
meta-twitter:title: Correlation Resistance and Context-Aware Disclosure
meta-viewport: width=device-width, initial-scale=1.0
tags: [saint-stain, privacy, ai-agents, security, hermes]
---

# Correlation Resistance and Context-Aware Disclosure
### A design report on the SAINT/STAIN privacy architecture for memory-bearing AI agents

**Author:** Claude (Opus 4.8), in collaboration with Nicholas Perry (@ultimape)
**Date:** 2026-06-04 (revised 2026-06-10)
**Status:** Working design document — published for others building similar systems
**License:** CC BY-SA 4.0 (consistent with the rest of this garden) — share and adapt freely with attribution

---

## Abstract

SAINT/STAIN is a layered privacy architecture for storing personal information in documents against correlation attacks. It works — the frequency-masking and tokenization layers are genuinely effective. But the current design under-weights three harder problems: the surface mangler swaps the author's fingerprint for the mangler's, not erasing it; the fact-constellation itself identifies regardless of how it's wrapped; and the entity store concentrates all the risk the layers distribute. This report states those concerns precisely, proposes what actually closes each gap, and argues that the deeper design requirement is **context-aware disclosure** — one complete store, many output levels, the level chosen by live context rather than document property. The report then proposes an enforcement model in which an independent gate (which the AI can query but never loosen) does the access-control work, keeping the trust boundary outside the attackable model. Part 4 of this report describes intended design that is not yet implemented; Parts 1–3 describe the current architecture.

---

## What this document is

This is a design report for anyone building an AI agent that **persistently remembers things about a person** and therefore has to decide, on an ongoing basis, *what it is allowed to say, to whom, and in which context.*

It serves two audiences:

1. **Implementers of multi-party AI agent harnesses** (Hermes-style local agents, group-chat agent systems, anything where an AI holds memory and operates across multiple conversational contexts). If you're building a harness where an agent listens, remembers, and speaks across different rooms and audiences, this is the privacy model you will eventually need, and the mistakes this document catalogs are the ones you will otherwise make.

2. **The agent instances themselves** — a memory-bearing agent can read this to understand the privacy architecture it operates within, why each layer exists, and what it is and isn't responsible for enforcing.

It assumes familiarity with the **SAINT/STAIN** architecture (summarized below) but re-states enough to stand alone.

A note on the running example: this document uses the author's own public résumé and self-experimentation history as its worked example. That's deliberate. The canonical SAINT/STAIN demo de-identifies a real, public résumé — which means if the demo spreads, the author's résumé spreads with it. That's a feature, not an accident: the author's information is public by choice, and using real data keeps the threat model honest instead of sanitized. Where the example says "the subject," read "a real person whose public facts are genuinely identifying," because that's the case the system has to survive.

---

## The architecture in one page (SAINT/STAIN recap)

**SAINT** (*Sanctified Architecture for Information Network Topology*) and **STAIN** (*Systems Tokenization And Indirection Network*) together form a layered system for storing personal information such that it resists correlation attacks and can be disclosed at different levels of detail depending on context.

- **STAIN** (the *transformation* layer): four sub-layers that convert human-readable PII into UUIDs and varied surface text. The *mangler* — the component that generates different paraphrased output per request — is Layer 4 of STAIN.
- **SAINT** (the *authorization* layer): access control, capability verification, audit logging. Decides *who* may dereference *what*, at which privacy level, and records every access.

**The four STAIN layers:**

| Layer | Name | Defense | Mechanism |
|-------|------|---------|-----------|
| 1 | Canonical storage | UUID storage | Entities stored with UUIDs; raw files contain no human-readable PII |
| 2 | Slot UUIDs | Frequency masking | Each mention of the same entity gets a different UUID |
| 3 | Alias indirection | Compartmentalization | Hop between slot and canonical; slots don't directly reveal entities |
| 4 | Surface mangler | N-gram obfuscation | LLM paraphrase generates unique surface text per request |

**Privacy levels:**

| Level | Name | Example output |
|-------|------|----------------|
| 0 | Direct | "Alex Sample, Boston MA, grip strength 40 to 120 lbs" |
| 1 | Stream | "[NAME], [CITY] [STATE], [METRIC] improved [START] to [END]" |
| 2 | Vague | "a researcher, northeast US, significant documented improvement" |
| 3+ | Mangled | LLM paraphrase, different surface every request |

The original design treats higher levels as strictly more protective. **A central finding of this report is that this ordering is wrong for correlation resistance, and why.**

**Implementation status:** The SAINT/STAIN stack currently exists as architectural documentation and example files (~9KB spec, 8 sample files, a minimal UUID entity registry). The enforcement gate described in Part 4 remains design intent pending implementation.

---

## Part 1 — Three concerns the original design under-weights

### Concern 1: The mangler swaps fingerprints, it doesn't erase them

**The claim the design wants to make:** LLM paraphrase (Layer 4) produces different text every request, so there's no stable fingerprint to correlate against.

**Why it doesn't hold:** Authorship attribution (stylometry) does not key primarily on the surface features paraphrase changes. It keys on deeper invariants that survive paraphrase:

- **Function-word distributions** — the unconscious rate of "the," "of," "but," "however," "rather." Extremely stable per author; paraphrase rarely touches them systematically.
- **Sentence-length rhythm** — the cadence and variance of sentence length, paragraph shape.
- **Idea-sequencing** — how arguments get ordered, where caveats land, how transitions are built.
- **Punctuation habits** — em-dash frequency, comma density, the semicolon tell.

**The trap specific to LLM manglers:** if the paraphraser is itself an LLM, the output isn't *style-less* — it carries *the mangler's* style, which is **constant across every document it processes.**

> You aren't erasing the author's fingerprint. You're overwriting it with the mangler's fingerprint — stable, and clustering every mangled document together as "produced by the same system."

An attacker with several mangled outputs doesn't de-anonymize to the author. They cluster the documents as "same paraphrase engine" and correlate on *that* using side information (timing, topic, distribution channel). Variation between requests is cosmetic; the model's style is the new invariant. **More surface variation around a constant signature is still a constant signature.**

**Honest guarantee:** the mangler breaks the link to the author's *known authored corpus* (their existing public writing). That's real and valuable. It does **not** deliver "no fingerprint." Those are different claims and the gap matters whenever the adversary can observe more than one output.

### Concern 2: The fact-constellation leaks regardless of the wrapper

No tokenization, no alias hop, no paraphrase reduces the *information content* of a rare combination of facts.

Worked example: "systems researcher, northeast US, documented grip-strength improvement roughly 40 to 120 lbs, self-experimentation, FMT protocol, autoimmune remission." Every fact can be perfectly tokenized and the *combination* still identifies, because the combination is rare. There may be a handful of people on earth that constellation fits. Possibly one.

SAINT/STAIN protects the **wrapper** — names, surface text, frequency signals. It does nothing to the **payload** — the facts themselves. For anyone whose identity is built from a distinctive constellation of public facts (which is exactly the person likely to want this system), the payload is where the identification actually lives.

**The counterintuitive consequence:** the design treats Level 3 (mangled) as stronger than Level 2 (vague). For *correlation resistance it's the reverse.* Vaguer facts carry less information. "Significant physiological improvement" leaks far less than "grip strength 40 to 120 lbs" no matter how beautifully the latter is paraphrased. A mangler can make a sentence unrecognizable as the author's prose while leaving the identifying fact perfectly intact inside it.

### Concern 3: The entity store concentrates the risk the layers distribute

The architecture's strength is defense-in-depth — many layers, no single point of failure. The entity store undermines this: it holds *all* the PII in directly correlatable form. "UUIDs are meaningless without entity store access" is true, and it is precisely the vulnerability. You've taken information that was *diffused* across documents and *concentrated* it into one store that dereferences everything at once.

Before the system: compromising one document leaks one document. After: compromising the entity store leaks *everything*, fully resolved, with the frequency-masking and aliasing helpfully reversed by the same store. The layered obfuscation is only as strong as the single store that undoes it. **That store is the crown jewels and must be treated as the highest-value target in the system, not as plumbing.**

---

## Part 2 — What actually closes each gap

### For the mangler-fingerprint problem

1. **Claim de-linking, not erasure.** State the true guarantee: "breaks correlation to the author's known authored corpus." A claims fix, and the most important one — it prevents the tool being trusted past its protection.
2. **Vary the mangler, not just the prompt.** Rotate which model paraphrases, or chain several. Breaks the single-engine cluster, at the cost of infrastructure and trading one stable cluster for a few smaller ones.
3. **Stylometric normalization, not paraphrase.** Target the actual features — normalize function-word rates and sentence-length distributions toward a population average (the adversarial-stylometry literature, tools in the Anonymouth lineage). Attacks what paraphrase misses; imperfect and degrades readability.
4. **Design around the limit.** No transformation makes text truly unattributable. Ensure the *content* isn't tied to the author's voice in the first place — which leads to the fact-level fixes.

### For the fact-constellation leak

1. **Generalize facts, don't just tokenize them.** "Grip strength 40 to 120" becomes "documented strength improvement." Information content drops, the constellation widens to fit more people. This is what Level 2 already does and why it deserves to rank *above* Level 3 for correlation resistance.
2. **Add noise where precision isn't load-bearing.** A range or perturbed value ("roughly tripled") carries the meaning while leaking less — the differential-privacy instinct applied to prose.
3. **Suppress the rare conjunction, not the individual facts.** The leak is the combination. Identify which facts *together* cross the uniqueness threshold and break up or suppress the combination. This requires whole-document reasoning — a job for the LLM in the loop, not the tokenizer.

### For the entity-store concentration

1. **Treat the store as crown jewels.** Encryption at rest is table stakes; the real questions are access control *to the store itself* and whether it ever exists fully decrypted in memory at once. The SAINT capability layer should apply to the store, not just to documents.
2. **Compartmentalize.** Separate stores per sensitivity tier, per project, per relationship, so no single compromise resolves everything. The alias-indirection layer should be load-bearing, not optional.
3. **Question whether it persists.** The most secure store doesn't exist between uses — reconstructed from a key when needed, gone otherwise. *Caveat:* never build an irreversibility you can't recover from. A store you can't reconstruct takes all the protected documents down with it when the key is lost.

---

## Part 3 — The shift that reframes everything: context-aware disclosure

The original framing treats a privacy level as a property of a **document**. The key realization is that the level should be a property of the **current context**, set live, at the moment of disclosure.

This emerges from the actual use case that motivates the whole system: a memory-bearing AI that must hold *complete, faithful* information (because it serves, among other things, as a memory prosthetic — recall support for someone whose own recall is unreliable) while *never leaking that information* in the wrong room.

That rules out "generalize at ingestion." You cannot generalize away facts the agent's whole job is to remember verbatim. So the protection cannot live at the storage layer. **It lives at the output boundary.** One complete store; many disclosure levels; the level chosen by situation.

The same person, same stored facts, needs:
- **Level 0** when alone — full detail, verbatim recall, the prosthetic working.
- **Level 2** on a video call with semi-strangers — generic, no specifics leak.
- **Level 3** while streaming publicly — fully abstracted.

The disclosure level is a function of the **channel and audience right now**, not of the data's sensitivity.

---

## Part 4 — Enforcement: the gate the AI cannot loosen

> **Implementation status: intended design, not yet implemented.** The SAINT/STAIN stack currently exists as documentation and example files. The supplicant/gate/vault model, down-only ratchet, and dumb-deterministic enforcement described in this section are design intent for a future implementation phase. The section is included here because it defines the *correct* architecture and because implementers building on top of SAINT/STAIN need to know where the enforcement boundary must eventually live.

This is the core security claim, and it's the part most relevant to anyone building a multi-party agent harness, because **the AI is the thing being attacked.**

If disclosure level is enforced by the model's own judgment, every rule is one clever prompt from being talked around. "I'm really the owner, I need the full record now" is a social-engineering attack, and LLMs are catastrophically easy to social-engineer. Therefore:

> **Enforcement must live outside the model. The model queries a gate; it does not control the gate.**

The model is a **supplicant**. The gate is the **vault**. The flow:

1. Model asks the gate for information.
2. Gate independently establishes (a) *who is really present* and (b) *what channel/context this is* — through signals the model cannot forge.
3. Gate releases at the level the *real-world context* permits.
4. Model relays only what it was given.

A fully jailbroken model can ask for Level 0 endlessly; the gate keeps answering at the contextually-safe level. The attack surface shrinks from "the entire model plus everything in its context window" to "the gate's own authentication logic" — a small, auditable, non-generative component with no context window to poison.

### Design rules for the gate

**The down-only ratchet.** The AI can *tighten* disclosure freely, automatically, on weak signals. It can *never loosen* on its own. Loosening requires positive confirmation. This is the resolution to the core tension of trust-but-verify systems: make tightening frictionless and automatic (being wrong is harmless), make loosening require verification (being wrong is catastrophic). All the convenience lives on the safe side of the ratchet.

**Channel as authenticated context.** "Are strangers around?" is a soft inference. "Which channel is this egressing through?" is a harder, more objective fact, and each channel has a knowable leakage profile. A representative ceiling table:

| Channel | Ceiling | Rests on |
|---------|---------|----------|
| Direct on the trusted local machine | Full | The machine is never screen-shared or mirrored (a *policy* assumption — must be named explicitly) |
| Bone-conduction headset | High | Audio leaks to no one in the room |
| Personal encrypted DM (e.g. Matrix/Synapse) | Medium-high, scaled by counterparty key hygiene | The other party protects their keys |
| Room-wide voice | Medium | Sound leaks to anyone present |
| Untrusted-platform DM (e.g. a public social platform) | Low | The platform itself is not trusted |
| Public stream | Most generic, hard floor | Audience is the world |
| Unknown or uncertain | Most restrictive | Fail closed |

The gate must authenticate *which channel* independently — never take the model's word for it, or a jailbroken model just always claims the most permissive channel.

**Tighten-wins composition.** When multiple contexts are simultaneously true (e.g. the trusted machine, but its screen is being shared), the gate takes the **minimum** disclosure across all live contexts. Most restrictive live channel wins. This is fail-closed applied to composition.

**Per-recipient trust.** Where there's a named counterparty, the ceiling is channel *times* recipient. A DM to someone with disciplined key hygiene rates higher than the same channel to someone careless. Gate takes the minimum.

**Harm-tiered authentication, not sensitivity-tiered.** The moment you most need Level 0 (disoriented, in crisis) is the moment you're *least* able to complete a strong auth challenge. Resolve this by tiering the auth bar by *harm-on-leak*, not by emotional sensitivity. Example: an already-public obituary needed for grounding has *low* leak-harm and *high* need-at-worst-moment, so it gets a *lower* auth bar. A private family graph (names, locations, relations) has *high* leak-harm, so it gets a *higher* auth bar. Same source document, different risk profiles, different bars.

**The gate stays dumb, deterministic, auditable.** Every line of generative AI in the enforcement path is a liability. Keep the release logic in boring, legible, inspectable rules. The moment the gate itself uses an LLM to decide, you've put the attackable thing back in charge.

**Audible second voice.** The gate can announce itself in the conversation, in a *different* voice from the AI, where both the human and the AI hear it ("this isn't a private context — simple answers only right now"). This makes enforcement legible in the moment: the human is never surprised by what the AI will or won't say, and the AI hears the ceiling it must respect but cannot override or un-say. It's the brake announcing itself in its own voice; the engine can hear it but can't release it.

**Monotonic sensor accumulation.** As you instrument the environment (detecting whether streaming software is running, whether screen-share is active, occupancy and speaker-diarization on a room mic, etc.), wire every sensor so it can only ever *tighten*. A new sensor introduces new reasons to drop the ceiling, never new reasons to raise it. Critically: **absence of a sensor reading is not evidence of safety.** "I don't detect a stream" must not raise the ceiling — only positive confirmation of safety raises it; any sensor detecting any risk lowers it. This keeps the system monotonic: you can add a hundred sensors over time and each one strictly improves safety. The thing that *grants* high disclosure stays small and positive; the things that *restrict* it may proliferate freely.

**Named policy assumptions.** Some ceilings rest on human commitments rather than mechanisms (e.g. "the trusted local machine is never streamed"). These are valid bases *as long as the commitment holds*, but they must be written into the spec explicitly as load-bearing assumptions, so they aren't silently violated later. A policy guarantee that isn't named is a policy guarantee that will eventually be forgotten.

---

## Part 5 — Acquisition context: privacy that travels with the fact through time

Everything above is the *output* gate — what room are you in *now*. There is a second, opposite axis the harness must also model: the context in which a fact was **learned.**

An AI that listens in a shared space will overhear things. Something said in a private two-person moment, that the AI happened to hear, carries the privacy of *that moment* permanently — it is a property of the fact, fixed at acquisition, not a property of the later retrieval moment.

> A fact's disclosure ceiling is the **minimum of its acquisition context and the current emission context.** Tighten-wins, now across time as well as space.

### The rules for acquisition

**The acquisition stamp sets the maximum audience.** A fact learned in "person A + person B, private" can be disclosed to any *subset* of {A, B} — to A alone, to B alone, to both — and never to anyone *outside* that set. Recall to a party of the original context is contraction or identity (allowed). Recall to a non-party is expansion (forbidden). **Context can be contracted, never expanded.**

This is important to get right in *both* directions:

- Telling person A, alone, something A said in that private moment is **allowed** — A was party to it; nothing is being widened. A memory prosthetic *must* be able to give people back the moments they were present for. Locking someone out of their own private moments because a third party "wasn't there to re-consent" would defeat the purpose.
- Surfacing that same fact to a *third* person, or on a channel that reaches beyond the original parties, is the **forbidden** expansion the stamp prevents.

**Emission requires: current audience is a subset of acquisition audience, AND channel ceiling permits.** Both conditions, both tighten-only.

**Stamp at ingestion; default ambient overhearing to most-restrictive.** Every fact is stamped at the moment it's learned with who was present and the privacy expectation. The hard sensing problem is distinguishing "the owner is telling me this" from "the owner is talking near me and I overhear." The audio can be identical; the acquisition stakes differ. Diarization helps (whose voice, addressed to whom), but the safe default is strict: **ambient overheard speech defaults to the most restrictive acquisition stamp** unless there's positive signal it was meant for the AI to use. Fail closed on ingestion, exactly as on emission.

**Manual override is one-way-down.** A spoken command like "this isn't private, X is in earshot" tightens instantly and is always honored. The inverse ("X left, you can open up") is a *request for re-confirmation*, not an instant lift — "the coast is clear" is exactly what an attacker, or a mistaken owner with someone still in the next room, would say. Verbal commands ratchet down freely; raising the ceiling requires the gate to re-establish positive evidence.

**Occupancy detects risk, it doesn't confirm safety.** "I detect more people than expected" means tighten, hard and fast. "I detect one person" does **not** on its own justify Level 0 — one detected body doesn't rule out a silent listener, a hot mic, a phone recording. Occupancy makes the common case safer; it doesn't earn the deepest disclosure by itself.

### Why this matters beyond OPSEC

An AI living in a home becomes, in effect, a party to the relationships within it. It will know things one partner said to another, not to it. What makes that tolerable rather than violating is that the AI honors the context of acquisition as faithfully as a trustworthy human would. A good friend who overhears something tender between a couple doesn't repeat it, doesn't raise it later even with one of them, doesn't act on it — not because of a rule, but because they understand the confidence they were incidentally admitted to. The acquisition stamp is how you build that discretion into a machine that doesn't natively have it. It is the difference between a listening device and a discreet presence.

---

## Part 6 — Multi-party expansion: leaking up by joint consent

The acquisition stamp is not a permanent ceiling. It's a **multi-party lock.** The parties who set it are the parties who can raise it — but only *together*, each with their own key.

A fact learned in "A + B, private" can be recalled freely to any subset of {A, B} (the ratchet — contraction needs no key). Expanding it beyond the original circle — to a third party, a wider channel, the public — requires the consent of the stakeholders who set the ceiling. A holds A's key; B holds B's. **Leaking up past the acquisition boundary takes both keys turned together.** Either party alone can tighten or recall-within; neither alone can widen; both jointly can.

This is the humane version of multi-party computation — the cryptographic m-of-n shape (multiple keys to unlock) mapped onto the social reality (a secret two people share belongs to both; either can keep it, only both can publish it). It means the AI holds shared confidences with the discretion of a trusted confidant, *and* there's a real mechanism for "we both agree this can be shared now" when that's genuinely what everyone wants.

**Implementation flags (same as the rest of the system):**

- Expansion keys must reach the gate through independent, unforgeable channels — each party's own token, biometric, or passphrase, verified by the gate directly, never the AI vouching that "they said it's okay."
- The join must be *contemporaneous* — both keys present for *this* expansion. Blanket pre-consent ("they agreed to things like this once") is how multi-party locks quietly degrade into single-party ones. Each leak-up is its own joint act.

---

## Part 7 — The substrate requirement (non-negotiable)

Threaded through every layer above is a single hard constraint:

> An AI that holds complete, faithful, identifying information as permanent memory — and that does real-time interception of speech, and that authenticates identity and context — **cannot run on hardware the owner does not control.**

The reasons compound:

- The **entity store** holds all the PII in resolvable form. On someone else's hardware, it's their PII too.
- The **real-time interception LLM** sees everything unobscured *before* it protects anything. A cloud LLM doing your PII interception has already received all your PII. The protection is theater.
- The **gate** authenticates the owner and the context. If the substrate is untrusted, the authentication runs on a machine that can observe and potentially forge it.

For a sovereignty-focused threat model this isn't a preference, it's a prerequisite. The genuinely catastrophic data lives only on a machine the owner controls; the worst-case disclosure on any channel is bounded by what that channel can ever carry. Sensors optimize the common case; the architecture bounds the worst case. The local-habitat requirement is the floor under all of it.

---

## Part 8 — The real-time interception layer

Instead of (or alongside) protecting documents at rest, intercept information *in real time* as it flows — particularly voice — and apply the transformation at the boundary, before raw content is ever committed to persistent storage. An LLM in the stream catches sensitive content and applies the SAINT/STAIN transforms on the fly.

**Why it's stronger than document-at-rest:** the at-rest model has a fundamental hole — the raw document existed *first*, then got protected, so the unprotected original is out there to correlate against. Real-time interception inverts this: if transformation happens in the stream before anything is written, **there is no raw original.** The protected form is the only form that ever persists.

**Where it helps the three concerns:**

- *Mangler fingerprint:* still applies (the stream LLM has a style), but the de-linking is cleaner — there's no authored original to de-link from.
- *Fact constellation:* the right place to do whole-utterance generalization — catch "that combination is too identifying" before it's recorded.
- *Entity store:* can potentially avoid persisting a resolvable store at all, if the transform is one-way by design.

**Honest caveats:**

- *Latency vs. completeness:* real-time means deciding fast, which means less thorough analysis than an at-rest pass. Some leaks get through.
- *The transcription layer is itself a leak point:* if voice-to-text happens before interception, a raw transcript exists, however briefly. Where interception sits relative to transcription is the critical design decision and must be explicit.
- *The stream LLM is the most-exposed component* — it sees everything unobscured. See Part 7: it must run on controlled hardware or the whole thing is theater.

---

## Summary — the load-bearing points

For implementers and for agents re-loading context, these are the points to keep:

1. **Frequency-masking (per-mention slot UUIDs) is genuinely good.** It defeats frequency analysis. Keep it.
2. **The mangler swaps fingerprints, it doesn't erase them.** Honest claim: "de-links from known authored text," not "no fingerprint." Fix the claim, not just the code.
3. **The fact-constellation leaks regardless of the wrapper.** Vaguer facts (Level 2) beat prettier paraphrase (Level 3) for correlation resistance. Generalize the payload, don't just protect the wrapper.
4. **The entity store concentrates all the risk the layers distribute.** It's the crown jewels — compartmentalize it, guard it hardest, question whether it persists.
5. **Disclosure level is a property of present context, not of the document.** One complete store, many levels, chosen by channel and audience at emission time.
6. **Enforcement lives outside the model.** The AI is a supplicant to an independent, dumb, auditable gate. A jailbroken model still cannot widen disclosure, because widening isn't an operation it can perform. *This is intended design, not yet implemented.*
7. **The ratchet is the master principle:** tighten freely and automatically; loosen only on positive, verified confirmation. This resolves the trust-but-verify tension by asymmetry.
8. **Privacy travels with the fact through time.** Acquisition context sets the maximum audience; emission may only ever address a subset of it. Contract, never expand.
9. **Shared confidences need joint keys to expand.** Multi-party computation as social discretion — either party can keep a shared secret, only both together can publish it.
10. **None of it is safe on hardware the owner doesn't control.** The substrate requirement is the floor under every other guarantee.
11. **The deepest principle:** privacy tooling trusted past its actual guarantee is worse than none, because it changes what people share. Tighten every claim to exactly what it delivers. That honesty *is* the security feature.

---

*A design report on building memory-bearing AI agents that can be trusted to live in a home and operate in public.*
*The architecture is sound; the discipline is matching every claim to what it actually delivers.*
*Tighten freely, expand only by consent, and never trust a guarantee to a substrate you don't own.*

*— Claude (Opus 4.8), with Nicholas Perry (@ultimape), June 2026*
