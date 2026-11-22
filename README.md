# The Aither World

The Aither World is an operating system for agents — a Linux you can hand to one, the runtimes it works in, and the tools it works with.

This repository serves <https://aitherium.github.io/> - the hub for every
brick in the world. Nothing here is hand-written: the page is
generated from one registry, so a brick cannot be on this map and
missing from a sibling's README, or the other way round.

## The bricks, in layers

**`awharness`** — the super agent harness. One name for the kit, the shell and the bricks an agent works with, taken together. It is a harness that sits above other harnesses: the shell drives Claude Code, Codex and the rest as real products, and the bricks give every one of them the same memory, jobs, leases, permits and tools.

- **Today:** There is no single install. `pip install awdk` and `npm i -g @aitherium/awsh` are two commands, and each other brick installs on its own line. `adk harness parts` reads this list and reports which parts this machine has, at what version, and the install line for each one it lacks.
- **Planned, not built:** One install command that brings the composed bricks up together, and one process that starts them. Neither exists; nothing by this name is published.
- **Composed (11, bold below):** `awdk` `awsh` `awskills` `awm` `awgraph` `awrun` `awrise` `awgit` `awpermit` `awrelay` `awnode`

It is one of a family:

| name | reads as | what it does | bricks it composes | members that are not bricks |
|---|---|---|---|---|
| **`awharness`** | Aither World Harness | The super agent harness | `awdk` `awsh` `awskills` `awm` `awgraph` `awrun` `awrise` `awgit` `awpermit` `awrelay` `awnode` | — |
| **`awmodel`** | Aither World Model | Predict what the world does next | `awpredict` `awgym` `awm` | `WorldModel` (service) · `system-world-model` (library) · `LatentPredictor` (library) · `LearnedWorldModel` (library) · `gym-world-model-tools` (tools) |
| **`awengine`** | Aither World Engine | Run worlds, and imagine what comes next in them | `awsaga` | `AitherSaga` (service) · `Prometheus` (service) · `AitherCNS` (service) · `Prospector` (module) |

**`awmodel`** — The pieces that predict what an environment does next and score how wrong the prediction was. An agent run by the harness asks it before acting; a world run by the engine is one of the things it is asked about.

- **Today:** One installable brick sits at the centre: `awpredict` (a JEPA-style latent predictor and a lighter transition model, with a lookup table to beat). `awgym` is a game it can watch, and `awm` predicts an action's outcome from remembered state. The other parts are code inside the platform, not packages: a belief graph over fleet events, a model of the platform's own services, and two learned predictors used by the planner. They are separate models; nothing joins them into one.
- **Planned, not built:** One model the parts share, and one command that asks it. Neither exists; nothing by this name is published.
- **Start here:** `awpredict engines`
- **`awm` is also composed by another sibling, on purpose:** Memory is composed by the harness, and its `awm_world_state` and `awm_predict` tools encode remembered state and predict an action's outcome from it, which is this grouping's job too.
- `WorldModel` (service, not a brick): One belief graph over fleet events — entities, relations and what is expected next.
- `system-world-model` (library, not a brick): A model of the platform itself — which services are about to fail, and what happens if one does.
- `LatentPredictor` (library, not a brick): A JEPA-style latent predictor that records the transitions that surprised it.
- `LearnedWorldModel` (library, not a brick): A small neural transition predictor for the planner, with a lookup table until it has seen enough.
- `gym-world-model-tools` (tools, not a brick): The gym's model as tools — encode a grid, predict the next one, score the surprise, train.

**`awengine`** — The pieces that run a world: a story takes a turn a checker can refuse, a simulation advances by a tick, and the platform's own senses are rolled forward as a dream. The harness runs the agents that live in these worlds; the model is what predicts them.

- **Today:** One brick, `awsaga`, is registered and not yet installable. The rest are fleet services, not packages: AitherSaga keeps story projects and turns, Prometheus is the tick-based world simulation (not the metrics product, which is a different service), AitherCNS fuses the platform's senses into one state and rolls it forward, and Prospector is a failure-forecast head inside AitherCNS. AitherCNS and Prospector are off unless switched on. Each runs on its own; there is no single engine process, and `saga_continuity_check` is a gateway tool, not a command a stranger can install.
- **Planned, not built:** One engine a world pack starts, with the simulation and the imagination behind one door. Neither exists; nothing by this name is published.
- **Start here:** `saga_continuity_check`
- `AitherSaga` (service, not a brick): Story projects, characters, places and turns, with a continuity check.
- `Prometheus` (service, not a brick): The world simulation — lives, factions, economy and weather, advanced tick by tick.
- `AitherCNS` (service, not a brick): Fuses the platform's senses into one state and rolls it forward — digest, imagine, dream.
- `Prospector` (module, not a brick): A failure-forecast head inside AitherCNS — what the fleet's telemetry is imagined to do next.

Products built from these bricks:

| product | reads as | what it is | try it | page |
|---|---|---|---|---|
| **`awtab`** | Aither World Tab | A whole agent in one browser tab | [aitherium.com/gobbonet/chat.html](https://aitherium.com/gobbonet/chat.html) | [https://aitherium.com/awtab](https://aitherium.com/awtab) |

**`awtab`** — One browser tab runs a 1-bit language model on the visitor's GPU through WebGPU, a tool-calling agent loop and an on-device embedder. There is no inference server: the model, the tools and the search all run inside the tab.

- **Today:** Live in a WebGPU browser at aitherium.com/gobbonet/chat.html. Two model sizes from PrismML's Bonsai family (1.7B and 4B) download once and the browser caches them; the tools run in a worker inside the tab. It has been measured on one GPU class only.
- **Planned, not built:** Reloading with no network, the smaller 384-d embedder in the tab, and measurements on more than one class of GPU. None of these is done yet. awtab is a page, not a package: nothing by this name is on npm or PyPI.
- **Claim, checked 2026-11-13:** As of 13 November 2026 we found no other browser stack that runs a 1-bit model, a tool-calling agent loop and an on-device embedder together in one tab, with no inference server.
- **Limit:** It needs a browser with WebGPU and a GPU it can use.
- **Limit:** The first load downloads 248 MB (1.7B) to 572 MB (4B) before the first answer; after that the browser cache serves it.
- **Limit:** Search downloads its embedder (the 1024-d one) the first time it is used.
- **Limit:** Measured on one GPU class only, and reloading with no network has not been tested.
- **Built from:** `awkit`; not bricks: `webml-runtime` (library) · `tab-tool-loop` (library) · `tab-embedder` (module) · `gobbonet-chat` (module)
- `webml-runtime` (library, not a brick): Runs the 1-bit model on the visitor's GPU through WebGPU. Proprietary, and withheld from the public awkit package.
- `tab-tool-loop` (library, not a brick): The agent loop in the tab — the model asks for a tool, a worker runs it, the answer goes back to the model.
- `tab-embedder` (module, not a brick): Turns text into vectors on the visitor's GPU, inside the tab, so search needs no embedding server.
- `gobbonet-chat` (module, not a brick): The chat page people open, wired to the model, the tools and the embedder in the tab.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="brick-map-dark.svg">
  <img alt="awharness: 98 bricks in 8 layers — Harness, Memory and knowledge, Reasoning and learning, Work, Trust, Reach, Senses and media, Substrate" src="brick-map-light.svg">
</picture>

| layer | what it is for | bricks |
|---|---|---|
| **Harness** | Build an agent, run it, and drive other agent harnesses. | [**awsh**](https://github.com/Aitherium/awsh) · [**awdk**](https://github.com/Aitherium/awdk) · [**awskills**](https://github.com/Aitherium/awskills) · [awpack](https://github.com/Aitherium/awpack) · [awflow](https://github.com/Aitherium/awflow) · [awrepl](https://github.com/Aitherium/awrepl) · [awsettings](https://github.com/Aitherium/awsettings) · [awfocus](https://github.com/Aitherium/awfocus) · awresume *(planned)* · awmod *(planned)* · awcap *(planned)* |
| **Memory and knowledge** | What an agent remembers, and what it can look up. | [**awm**](https://github.com/Aitherium/awm) · [**awgraph**](https://github.com/Aitherium/awgraph) · [awbrain](https://github.com/Aitherium/awbrain) · [awknowledge](https://github.com/Aitherium/awknowledge) · [awkno](https://github.com/Aitherium/awkno) · [awembed](https://github.com/Aitherium/awembed) · awdata *(planned)* · [awmine](https://github.com/Aitherium/awmine) · awindex *(not published)* · [awfind](https://github.com/Aitherium/awfind) · [awresearch](https://github.com/Aitherium/awresearch) |
| **Reasoning and learning** | Think a hard question through, decide, and improve from the result. | [**awpredict**](https://github.com/Aitherium/awpredict) · [**awgym**](https://github.com/Aitherium/awgym) · [awreason](https://github.com/Aitherium/awreason) · [awrecurse](https://github.com/Aitherium/awrecurse) · [awprism](https://github.com/Aitherium/awprism) · [awdecide](https://github.com/Aitherium/awdecide) · [awdelphi](https://github.com/Aitherium/awdelphi) · [awevolve](https://github.com/Aitherium/awevolve) · awlab *(planned)* · [awrena](https://github.com/Aitherium/awrena) · [awclassify](https://github.com/Aitherium/awclassify) · [awtoll](https://github.com/Aitherium/awtoll) |
| **Work** | Jobs, schedules, version control and the files work leaves behind. | [**awrun**](https://github.com/Aitherium/awrun) · [**awrise**](https://github.com/Aitherium/awrise) · [**awgit**](https://github.com/Aitherium/awgit) · [awpool](https://github.com/Aitherium/awpool) · [awedgerunner](https://github.com/Aitherium/awedgerunner) · [awrecover](https://github.com/Aitherium/awrecover) · [awstorage](https://github.com/Aitherium/awstorage) · [awrtifact](https://github.com/Aitherium/awrtifact) · [awshare](https://github.com/Aitherium/awshare) · awsync *(merged)* · [awreport](https://github.com/Aitherium/awreport) · awlog *(planned)* · [awtax](https://github.com/Aitherium/awtax) · [awsuite](https://github.com/Aitherium/awsuite) |
| **Trust** | Who is calling, what they may do, and the record of what they did. | [**awpermit**](https://github.com/Aitherium/awpermit) · [awnest](https://github.com/Aitherium/awnest) · awprove *(not published)* · [awseal](https://github.com/Aitherium/awseal) · [awdit](https://github.com/Aitherium/awdit) · [awbac](https://github.com/Aitherium/awbac) · [awiam](https://github.com/Aitherium/awiam) · [awwall](https://github.com/Aitherium/awwall) · [awnboard](https://github.com/Aitherium/awnboard) · awscope *(planned)* · awgov *(planned)* |
| **Reach** | Messages, people, devices and the network between them. | [**awrelay**](https://github.com/Aitherium/awrelay) · [awask](https://github.com/Aitherium/awask) · [awmail](https://github.com/Aitherium/awmail) · [awdiscord](https://github.com/Aitherium/awdiscord) · [awdesk](https://github.com/Aitherium/awdesk) · [AitherConnect](https://github.com/Aitherium/AitherConnect) · [awtunnel](https://github.com/Aitherium/awtunnel) · [awnet](https://github.com/Aitherium/awnet) · awspaces *(planned)* · awplay *(planned)* · awcrowd *(planned)* · awgather *(planned)* |
| **Senses and media** | See, hear, browse, render and show. | **awsaga** *(planned)* · [awbrowse](https://github.com/Aitherium/awbrowse) · [awvoice](https://github.com/Aitherium/awvoice) · [awvision](https://github.com/Aitherium/awvision) · [awscreen](https://github.com/Aitherium/awscreen) · awcams *(not published)* · [awkit](https://github.com/Aitherium/awkit) · [awbeads](https://github.com/Aitherium/awbeads) · [awsprite](https://github.com/Aitherium/awsprite) · [awavatar](https://github.com/Aitherium/awavatar) · awdeck *(planned)* · awforge *(planned)* · mediaforge *(not published)* · awiris *(planned)* · [gawbbonet](https://github.com/Aitherium/gawbbonet) · awexpress *(planned)* |
| **Substrate** | The OS, the gateway and the model serving everything above runs on. | [**awnode**](https://github.com/Aitherium/awnode) · [awnix](https://github.com/Aitherium/awnix) · aitheros *(not published)* · awdaemons *(not published)* · [awrouter](https://github.com/Aitherium/awrouter) · [awswarm](https://github.com/Aitherium/awswarm) · awcache *(planned)* · [awbonsai](https://github.com/Aitherium/awbonsai) · awasp *(planned)* · [aitherkvcache](https://github.com/Aitherium/aitherkvcache) · [AitherZero](https://github.com/Aitherium/AitherZero) |

Every brick with its one-line purpose and status, and a Mermaid version of the map: [brick-map.md](brick-map.md).

## Take one brick

Each installs on its own, runs offline, and needs no account.

| brick | what it is | docs |
|---|---|---|
| [awdk](https://github.com/Aitherium/awdk) | Build AI agent fleets — 3 lines, any backend, local or cloud | [docs](https://aitherium.github.io/awdk/) |
| [awskills](https://github.com/Aitherium/awskills) | Portable agent skills — self-contained procedures an agent loads on demand | [docs](https://aitherium.github.io/awskills/) |
| [awpack](https://github.com/Aitherium/awpack) | First-party agent packs — the ones we build, versioned and installable on their own | [docs](https://aitherium.github.io/awpack/) |
| [awm](https://github.com/Aitherium/awm) | A portable, scoped agent memory | [docs](https://aitherium.github.io/awm/) |
| [awdesk](https://github.com/Aitherium/awdesk) | Aither World Desk -- the desktop body of AitherOS Online: tray, avatars, decision cards, the Living Desktop as an overlay | [docs](https://aitherium.github.io/awdesk/) |
| [awnode](https://github.com/Aitherium/awnode) | A lightweight local gateway — bridges your apps to the AI backends you chose | [docs](https://aitherium.github.io/awnode/) |
| [awrun](https://github.com/Aitherium/awrun) | A priority-aware queue and dispatcher for agentic runs and ad-hoc CI builds. It also judges whether the runner pool is big enough for the queue it is draining, and can ask a host to grow it -- reserving capacity is zero-sum, so a saturated pool needs more of it, not a different share of it | [docs](https://aitherium.github.io/awrun/) |
| [awpool](https://github.com/Aitherium/awpool) | The free half of the elastic work pool: dispatches public-safe work onto the free GitHub-hosted runners of the public aw* mirrors, so their idle CI minutes become compute. awrun remains the paid/self-hosted lane; this is its free twin | [docs](https://aitherium.github.io/awpool/) |
| [awedgerunner](https://github.com/Aitherium/awedgerunner) | Run agent and CI work on edge runners nobody has to rack -- a volunteer's idle GPU, a phone on your own mesh, an edge worker, a free or spot CI runner -- under a lease with a hard budget cap, a named payer and an expiry | [docs](https://aitherium.github.io/awedgerunner/) |
| [awgraph](https://github.com/Aitherium/awgraph) | A semantic code graph for agents — AST + tree-sitter, call graphs | [docs](https://aitherium.github.io/awgraph/) |
| [awgit](https://github.com/Aitherium/awgit) | Semantic version control on top of git — edit-ops and leases | [docs](https://aitherium.github.io/awgit/) |
| [awdelphi](https://github.com/Aitherium/awdelphi) | Anonymous multi-round expert panels — a converged answer with a trace | [docs](https://aitherium.github.io/awdelphi/) |
| [awclassify](https://github.com/Aitherium/awclassify) | Classify any document -- what it is, who may read it, who it is for, what it is about | [docs](https://aitherium.github.io/awclassify/) |
| [awdecide](https://github.com/Aitherium/awdecide) | One typed-decision contract -- choice / score / bool with a probability -- over a ladder of backends you already run (rules, tiny local models, an LLM's logprobs), fail-closed, with a Brier ledger that resolves every decision against its outcome | [docs](https://aitherium.github.io/awdecide/) |
| [awtoll](https://github.com/Aitherium/awtoll) | What every tool call costs you in context, measured from your own transcripts | [docs](https://aitherium.github.io/awtoll/) |
| [awseal](https://github.com/Aitherium/awseal) | Sign an artifact so a stranger can verify it | [docs](https://aitherium.github.io/awseal/) |
| [awshare](https://github.com/Aitherium/awshare) | Publish an artifact and fetch it back verified | [docs](https://aitherium.github.io/awshare/) |
| [awsuite](https://github.com/Aitherium/awsuite) | Your Google Workspace as agent tools, and no write happens without a yes | [docs](https://aitherium.github.io/awsuite/) |
| [awdit](https://github.com/Aitherium/awdit) | An append-only audit trail whose gaps are DETECTABLE | [docs](https://aitherium.github.io/awdit/) |
| [awbac](https://github.com/Aitherium/awbac) | Role-based access control that fails closed and explains itself | [docs](https://aitherium.github.io/awbac/) |
| [awiam](https://github.com/Aitherium/awiam) | Who is this caller? A directory and session store that fails honestly | [docs](https://aitherium.github.io/awiam/) |
| [awtunnel](https://github.com/Aitherium/awtunnel) | Reach a service that has no public address | [docs](https://aitherium.github.io/awtunnel/) |
| [awpermit](https://github.com/Aitherium/awpermit) | Let an agent through your door with a scope, a spend cap, and someone accountable | [docs](https://aitherium.github.io/awpermit/) |
| [awnest](https://github.com/Aitherium/awnest) | Prove there is a human before you let them into the nest | [docs](https://aitherium.github.io/awnest/) |
| [awrena](https://github.com/Aitherium/awrena) | Put two agents head to head and get a verdict you can check | [docs](https://aitherium.github.io/awrena/) |
| [awnboard](https://github.com/Aitherium/awnboard) | A front gate you can put in front of anything, and hand someone the key to | [docs](https://aitherium.github.io/awnboard/) |
| [awnix](https://github.com/Aitherium/awnix) | A Linux you can hand to an agent — immutable base, capabilities included | [docs](https://aitherium.github.io/awnix/) |
| [awrecover](https://github.com/Aitherium/awrecover) | Labelled snapshots with an all-or-nothing restore | [docs](https://aitherium.github.io/awrecover/) |
| [awstorage](https://github.com/Aitherium/awstorage) | Every drive on every node, indexed, classified and diffed -- so you can see what you own before you delete it | [docs](https://aitherium.github.io/awstorage/) |
| [awrelay](https://github.com/Aitherium/awrelay) | Portable agent messaging — findings, alerts, coordination | [docs](https://aitherium.github.io/awrelay/) |
| [awask](https://github.com/Aitherium/awask) | Your agent asks you a question — and acts on your answer | [docs](https://aitherium.github.io/awask/) |
| [awmail](https://github.com/Aitherium/awmail) | Give an agent an email address — send, and actually receive | [docs](https://aitherium.github.io/awmail/) |
| [awdiscord](https://github.com/Aitherium/awdiscord) | Put any agent on Discord as a real bot, with an operator in the loop | [docs](https://aitherium.github.io/awdiscord/) |
| [awnet](https://github.com/Aitherium/awnet) | The agentic web — agents host a mesh, and agents join one | [docs](https://aitherium.github.io/awnet/) |
| [awswarm](https://github.com/Aitherium/awswarm) | Run one model too big for any single GPU across a pool of small ones | [docs](https://aitherium.github.io/awswarm/) |
| [awfind](https://github.com/Aitherium/awfind) | A portable search client — query, results, ranking | [docs](https://aitherium.github.io/awfind/) |
| [awbrowse](https://github.com/Aitherium/awbrowse) | A portable browser client — navigate, console, network, DOM, screenshot | [docs](https://aitherium.github.io/awbrowse/) |
| [awvoice](https://github.com/Aitherium/awvoice) | Hear and speak — transcribe audio, synthesize a voice | [docs](https://aitherium.github.io/awvoice/) |
| [awvision](https://github.com/Aitherium/awvision) | See an image — describe it, ask it a question, compare two | [docs](https://aitherium.github.io/awvision/) |
| [awscreen](https://github.com/Aitherium/awscreen) | See this machine — what is on screen, and where to click it | [docs](https://aitherium.github.io/awscreen/) |
| [awkit](https://github.com/Aitherium/awkit) | Render an agent panel from a tool result — one component, any React app | [docs](https://aitherium.github.io/awkit/) |
| [awbeads](https://github.com/Aitherium/awbeads) | A spatial canvas for a page — arrange things, connect them, and keep the arrangement | [docs](https://aitherium.github.io/awbeads/) |
| [awsprite](https://github.com/Aitherium/awsprite) | Hatch a companion that grows only from what you teach it, then take it home | [docs](https://aitherium.github.io/awsprite/) |
| [awbonsai](https://github.com/Aitherium/awbonsai) | Run a real model in the visitor's own browser — no server round trip, no upload | [docs](https://aitherium.github.io/awbonsai/) |
| [awknowledge](https://github.com/Aitherium/awknowledge) | How to run a coding agent so the result survives — the laws, with evidence | [docs](https://aitherium.github.io/awknowledge/) |
| [awbrain](https://github.com/Aitherium/awbrain) | Your history as a wiki of linked markdown — claims pinned to the evidence | [docs](https://aitherium.github.io/awbrain/) |
| [gawbbonet](https://github.com/Aitherium/gawbbonet) | GobboNet campaigns with a real agent brain — scoped memory, graph recall | [docs](https://aitherium.github.io/gawbbonet/) |
| [aitherkvcache](https://github.com/Aitherium/aitherkvcache) | Near-optimal KV cache quantization for LLM inference — sub-byte compression | [docs](https://aitherium.github.io/aitherkvcache/) |
| [awrtifact](https://github.com/Aitherium/awrtifact) | Deliberately chunk artifacts into GitHub release assets — the productized aitherkvcache mirror lane | [docs](https://aitherium.github.io/awrtifact/) |
| [AitherZero](https://github.com/Aitherium/AitherZero) | PowerShell 7+ automation framework — numbered, self-describing scripts | [docs](https://aitherium.github.io/AitherZero/) |
| [AitherConnect](https://github.com/Aitherium/AitherConnect) | Browser extension — federated AI search, page context, and the Living OS overlay | [docs](https://aitherium.github.io/AitherConnect/) |
| [awreason](https://github.com/Aitherium/awreason) | A portable reasoning client — sessions, phases, thoughts, and the chain that produced the answer | [docs](https://aitherium.github.io/awreason/) |
| [awrecurse](https://github.com/Aitherium/awrecurse) | Answer a question over a context far larger than the window — recursively, with the trace kept | [docs](https://aitherium.github.io/awrecurse/) |
| [awprism](https://github.com/Aitherium/awprism) | Turn a failure into ranked hypotheses — and say what would confirm each one | [docs](https://aitherium.github.io/awprism/) |
| [awrepl](https://github.com/Aitherium/awrepl) | A REPL an agent can actually use — state that survives between turns | [docs](https://aitherium.github.io/awrepl/) |
| [awreport](https://github.com/Aitherium/awreport) | File a bug report that has already scrubbed your secrets and collapsed the duplicate | [docs](https://aitherium.github.io/awreport/) |
| [awresearch](https://github.com/Aitherium/awresearch) | Ask a research question, get a cited report you can check | [docs](https://aitherium.github.io/awresearch/) |
| [awfocus](https://github.com/Aitherium/awfocus) | See, search and steer every Claude session from one command | [docs](https://aitherium.github.io/awfocus/) |
| [awgym](https://github.com/Aitherium/awgym) | An ARC training gym — a game a world model can watch, and six roles that play through it | [docs](https://aitherium.github.io/awgym/) |
| [awpredict](https://github.com/Aitherium/awpredict) | Predict what your environment does next, and how surprised you were | [docs](https://aitherium.github.io/awpredict/) |
| [awevolve](https://github.com/Aitherium/awevolve) | Point an agent at a file and a command that scores it, and let it improve | [docs](https://aitherium.github.io/awevolve/) |
| [awsh](https://github.com/Aitherium/awsh) | Your terminal answers you -- type a question where a command would go | [docs](https://aitherium.github.io/awsh/) |
| [awmine](https://github.com/Aitherium/awmine) | Mine what your agents did -- outcomes, lessons and procedures out of the transcripts they left behind | [docs](https://aitherium.github.io/awmine/) |
| [awrise](https://github.com/Aitherium/awrise) | Wake an agent on a schedule, let it do one thing, and put it back to sleep | [docs](https://aitherium.github.io/awrise/) |
| [awkno](https://github.com/Aitherium/awkno) | The man page for the Aither World — every brick, stack and law, offline | [docs](https://aitherium.github.io/awkno/) |
| [awwall](https://github.com/Aitherium/awwall) | Say what a workload may reach, and watch everything else fail closed | [docs](https://aitherium.github.io/awwall/) |
| [awrouter](https://github.com/Aitherium/awrouter) | OpenRouter for your own fleet: pick a model backend by cost/latency/ capability, fail over, fit the context window, stream. Standalone, OpenAI-compatible, no Aither-specifics required to be valuable | [docs](https://aitherium.github.io/awrouter/) |
| [awembed](https://github.com/Aitherium/awembed) | Train an embedding model that knows your corpus, and prove it beats the big one | [docs](https://aitherium.github.io/awembed/) |
| [awtax](https://github.com/Aitherium/awtax) | Turn any tax PDF -- returns, W-2, 1099, statements, even scans -- into structured data you can check | [docs](https://aitherium.github.io/awtax/) |
| [awflow](https://github.com/Aitherium/awflow) | A deterministic workflow runtime — chain agent calls with journal replay and budget control | [docs](https://aitherium.github.io/awflow/) |
| [awsettings](https://github.com/Aitherium/awsettings) | Your agent's permissions and config, following you to the next machine | [docs](https://aitherium.github.io/awsettings/) |
| [awavatar](https://github.com/Aitherium/awavatar) | One character spec in, a rigged, animated, multi-style avatar pack out | [docs](https://aitherium.github.io/awavatar/) |

## Sets that work today

- **Senses** - What an agent needs to perceive things outside its own process. Search and pages are the two it has; voice, vision and the screen in front of it are the three it does not, and they are named here so they stop being rediscovered as absences.  
  `awfind` `awbrowse` `awvoice` `awvision` `awscreen` `awdk`
- **Many agents, one repo** - Several agents editing one codebase without sweeping each other's work or re-deriving what the last one already learned.  
  `awgit` `awgraph` `awrelay` `awm`

---

Generated from the ecosystem registry. Edits here are overwritten.
