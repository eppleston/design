# Journey Map — Agentic AI Operating System for a UK Retail Bank

**Actors:**
- **Sarah** — Head of Retail Operations (SM&CR accountability for operational resilience and consumer outcomes)
- **Ravi** — Senior AI Platform Engineer (builds and operates the agent fleet)
- **Fiona** — Chief Risk & Compliance Officer (FCA-accountable; SM&CR Prescribed Responsibility for model risk and consumer protection)

**Scenario:** A large UK retail bank adopts an agentic AI operating system — moving from exec mandate through to operational maturity, with agents handling the majority of routine banking operations.

**Timeframe:** 18–24 months

---

## Phase Overview

| Phase | 1. Strategic Mandate | 2. Architecture & Integration | 3. Controlled Pilot (Agents 1–10) | 4. First Autonomous Failure | 5. Scaling with Governance (10–100+ Agents) | 6. Operational Maturity |
|---|---|---|---|---|---|---|
| **Focus** | Commitment and programme launch | Legacy integration design and governance framing | Narrow-scope live agents with mandatory human oversight | High-stakes incident; governance stress test | Agent fleet expansion with hardened controls | Agents as core infrastructure; FCA review passed |
| **Sarah's state** | Apprehensive mandate recipient | Mapping operational handoff points | Cautious observer, shadow auditing | Crisis response; personal liability activated | Operations role transformation | Confident operational governance |
| **Ravi's state** | Excited; underestimating legacy complexity | Deep in COBOL/IBM MQ integration hell | First real production signal; evals diverging from staging | Incident owner; root cause is dirty data | Building repeatable integration patterns | Platform is stable and observable |
| **Fiona's state** | Setting governance conditions | Defining policy without technical grounding | Can't yet answer FCA questions; monitoring gaps visible | SM&CR exposure realised; escalation path tested | Governance framework hardened and evidenced | First clean FCA supervisory engagement |

---

## Phase 1: Strategic Mandate

**What happens:** Following Lloyds' announcement of £50m in AI value delivered in 2025, the board approves a programme budget for an agentic OS. NatWest's AI-first pivot and HSBC's cautious ChatGPT trials create additional competitive pressure. The CTO is given architectural ownership. Sarah receives a mandate from the COO she did not request. Fiona sets governance conditions before any deployment begins.

**Actions by actor:**
- **Sarah:** Attends the programme kick-off. Raises accountability questions about Consumer Duty and operational escalation that the programme plan does not address. Is told these will be "worked through in the design phase."
- **Ravi:** Joins as the technical lead. Conducts a preliminary audit of the data estate. Within two weeks identifies seven legacy CRM systems with incompatible customer record formats, an IBM MQ broker with undocumented routing rules, and COBOL batch jobs that create 14-hour data latency on certain customer fields.
- **Fiona:** Insists on a governance pre-condition: no agent goes to production without a model risk assessment (PRA SS 1/23), a Consumer Duty impact mapping, and a designated accountable senior manager. Drafts a one-page AI Governance Policy Statement aligned to FINOS AI Governance Framework v2.0.

**Touchpoints:** Board programme approval, programme kick-off deck, initial data estate audit, AI Governance Policy Statement

**Thoughts:**
- **Sarah:** "I've been handed accountability for something I don't understand yet. I need to establish what 'operationally responsible' means before the first agent goes live."
- **Ravi:** "The vendor demo ran on clean synthetic data. Our production data is 30 years of accumulated inconsistency. The gap between the two is my problem to solve."
- **Fiona:** "The programme timeline assumes 6 weeks to governance sign-off. The model risk team has a 6-week queue and a checklist designed for credit scorecards. Those two facts are going to collide."

**Feelings:**
- **Sarah:** Unsettled; responsible for an outcome she cannot yet define
- **Ravi:** Energised but quietly alarmed by the state of the data estate
- **Fiona:** In control of the governance framework; anxious about what the technical team will discover

**Pain points:**
- Programme plan treats governance as a gate, not a parallel workstream — Fiona's involvement is reactive rather than integrated from day one
- No shared definition of "operational accountability" for agent decisions across Sarah, Ravi, and Fiona
- Legacy data complexity is underestimated; the programme timeline does not include a data remediation budget
- The bank has no stable agent registry or naming convention — agents will be referred to informally, creating tracking problems within weeks

**Opportunities:**
- Establish a shared agent registry at programme launch: unique identifiers, risk classification, designated accountable owner, governance status — built before the first agent is deployed
- Run a governance co-design sprint in Month 1 with Sarah, Ravi, and Fiona together — not sequential handoffs
- Conduct a data quality audit in Phase 1 as a formal programme deliverable, not a technical discovery Ravi does alone
- Adopt FINOS AI Governance Framework v2.0 as the baseline control taxonomy from day one, rather than retrofitting it after pilot

---

## Phase 2: Architecture & Integration Design

**What happens:** Platform engineering begins connecting the agent OS to legacy systems. Ravi builds the first COBOL-to-agent data pipeline through a normalisation layer that translates mainframe batch exports into structured inputs agents can consume. Fiona's team develops governance policies. Sarah maps which operational workflows will have agent handoff points, identifying 23 distinct decision types that currently require human sign-off.

**Actions by actor:**
- **Sarah:** Works with operations managers to document current human decision workflows — the first systematic attempt to capture what agents will need to replicate or route. Discovers that many decisions involve implicit contextual judgement that has never been written down.
- **Ravi:** Builds the first three IBM MQ adapters and the COBOL batch normalisation layer. Documents each data anomaly encountered. Flags to the programme that production data latency on the customer address field means agents will be working from records up to 14 hours stale on some flows — a data freshness risk that has regulatory implications for time-sensitive decisions.
- **Fiona:** Drafts the AI Governance Policy using the FINOS framework v2.0 as a baseline. Gets stuck on the technical controls section because she cannot get a confirmed list of what data the agents will access, what decisions they will make, or how their outputs will be logged. Requests a "platform capabilities briefing" from Ravi's team; receives a system architecture diagram she cannot interpret.

**Touchpoints:** Data pipeline design documents, IBM MQ adapter specifications, operational workflow mapping sessions, draft AI Governance Policy, Consumer Duty impact assessment template

**Thoughts:**
- **Sarah:** "We've never written down how a KYC analyst decides whether a document is acceptable. The decision rules are in people's heads. If we don't capture them now, the agent will be making decisions no one can validate."
- **Ravi:** "I've found the data latency issue. A 14-hour-old customer address could cause an agent to flag a payment to a valid address as suspicious. I need to decide whether to report this as a programme risk or solve it quietly before anyone asks."
- **Fiona:** "I need to know what decisions the agents will make, at what volume, and what the failure mode looks like. I've asked twice. I've received architecture diagrams both times."

**Feelings:**
- **Sarah:** Engrossed in a discovery process she was not expecting; useful but time-consuming
- **Ravi:** Productive but accumulating technical debt faster than he is documenting it
- **Fiona:** Frustrated; feels excluded from technical decisions that have direct governance implications

**Pain points:**
- Implicit operational knowledge is not being captured systematically — decision logic lives in people's heads and will not be transferable to agent validation frameworks
- Data latency on legacy batch exports is a known risk that is not yet in the programme risk register
- Governance and engineering are working in parallel without a shared information model — Fiona's governance questions cannot be answered by Ravi's architecture documents
- No mechanism for Fiona to review agent decision logic in policy terms rather than technical terms

**Opportunities:**
- Decision taxonomy: build a shared, plain-language register of every decision type an agent will make, with associated regulatory classification, Consumer Duty relevance, and confidence threshold for human escalation — owned jointly by Sarah and Fiona
- Data freshness SLA: define acceptable data age per decision type (e.g., address verification requires near-real-time; balance-based decisions can tolerate batch latency) and build enforcement into the pipeline, not as a manual check
- Translate architecture diagrams into a governance-readable agent capability statement — one page per agent: inputs, outputs, decision logic in plain language, failure modes, escalation path
- Pair governance policy drafting with a technical walkthrough: Ravi walks Fiona through one agent's decision pipeline in plain language before the policy is finalised

---

## Phase 3: Controlled Pilot — Agents 1–10

**What happens:** The first agents go live in narrow scope under mandatory human oversight. The payment exception routing agent and the KYC document verification agent are first. Both are deployed in "recommend, not decide" mode — agents produce a recommendation; a human approves or overrides. Sarah's team must process the same volume of exceptions as before, but now also review agent recommendations. Ravi monitors production behaviour against his staging evals.

**Actions by actor:**
- **Sarah:** Briefs her operations managers on the new workflow. Implements a shadow-review protocol — her team logs every agent recommendation override in a shared spreadsheet. Within three weeks, the spreadsheet shows a 9% override rate on the KYC agent, concentrated on self-employed customers whose income documentation doesn't match a narrow set of expected formats. Reports this to Ravi.
- **Ravi:** Monitors production metrics daily. Notes that the KYC agent's confidence scores on self-employed customer documents are systematically lower than on employed customers — a pattern absent in staging because the staging dataset was not representative. Begins investigating the training data composition.
- **Fiona:** Reviews the model risk assessment for both agents. The PRA SS 1/23 validation checklist did not include a demographic bias test. Requests a bias analysis retrospectively. Receives a response that the model risk team does not have a validated methodology for LLM-based agents.

**Touchpoints:** Agent recommendation interface, override log spreadsheet, production monitoring dashboard, model risk validation report, bias analysis request

**Thoughts:**
- **Sarah:** "9% override on KYC is material. If we go to straight-through processing at current accuracy, that's 9% of customers getting a wrong answer. At our volumes, that's several thousand decisions a week."
- **Ravi:** "The training data for the KYC agent was weighted toward employed customers because that's what we had clean labels for. Self-employed edge cases are underrepresented. This is a data composition problem, not a model problem. But it will look like a model problem to everyone except me."
- **Fiona:** "The model risk team cannot run a bias analysis on an LLM agent. That means every agent we deploy has an unvalidated Consumer Duty exposure that I am personally accountable for. I am not comfortable with that."

**Feelings:**
- **Sarah:** Gaining confidence in the override process; anxious about the scale implications of the 9% override rate
- **Ravi:** Vindicated that shadow deployment caught the issue; concerned that the training data gap is deeper than a single agent
- **Fiona:** Blocked; the governance framework has a gap she cannot close without new methodology from the model risk team

**Pain points:**
- Override logging is manual (shared spreadsheet) and not integrated into the platform — data from it cannot be used to automatically retrigger model evaluation
- The training data bias issue on self-employed customers would not have been caught without Sarah's manual shadow review; the platform has no automated demographic performance segmentation
- Model risk validation has no validated methodology for LLM-based agents — a gap that applies to all future deployments, not just the KYC agent
- Pilot agents are generating a volume of recommendations that the operations team must review, increasing their workload rather than reducing it — the efficiency case for the programme is not yet visible to staff

**Opportunities:**
- Override capture as a first-class platform feature: structured, queryable, automatically fed back into eval pipelines — not a spreadsheet
- Demographic performance segmentation built into agent monitoring: flag agents with differential accuracy by customer demographic category before, not after, a Consumer Duty review
- Model risk methodology update: commission a validated LLM agent assessment framework before Phase 5 scaling begins — the 6-week queue and legacy checklist are not compatible with fleet expansion
- Show operations staff the compound override rate trend: if the agent's override rate is declining week-on-week, that is visible evidence of improvement — make it visible in the operational reporting layer

---

## Phase 4: First Autonomous Failure

**What happens:** The payment exception routing agent is granted straight-through processing authority on low-value exceptions (under £500) after three weeks of pilot with a 2% override rate. Six days later, a data quality incident on the IBM MQ feed produces malformed transaction records — a character encoding issue in the legacy COBOL batch job causes the agent to misclassify 2,800 legitimate Faster Payments as suspicious routing exceptions. These are held, not processed. Customers begin contacting the bank. The incident runs for 4 hours before the IBM MQ adapter monitoring alert fires. It is the bank's first agent-caused consumer incident. The FCA relationship manager calls Fiona the following morning.

**Actions by actor:**
- **Sarah:** Receives a call from the contact centre at 11pm. Is told 2,800 payment holds are generating customer complaints. Has no access to the agent's decision log in a format she can read. Calls Ravi. It takes 45 minutes to establish that the agent, not a system outage, is the cause.
- **Ravi:** Diagnoses the character encoding issue in the COBOL batch export within 2 hours. The fix is a one-line patch to the IBM MQ adapter. The root cause is a known data quality risk he flagged 6 weeks ago and added to the programme risk register; it was accepted with no mitigation action. The incident is resolved by 03:00. He documents the root cause, the timeline, and the data quality risk history.
- **Fiona:** Receives the FCA call at 09:15 the following morning. Is asked three questions: how many customers were affected, what was the cause, and what controls failed. She can answer the first two. She cannot answer the third without Ravi's incident report — which she receives at 10:40, 85 minutes after the FCA call begins.

**Touchpoints:** Contact centre alert, agent decision log (inaccessible to Sarah without engineering support), IBM MQ adapter monitoring, programme risk register, FCA supervisory call, post-incident review report

**Thoughts:**
- **Sarah:** "I found out about a 2,800-customer incident because a contact centre manager called me at 11pm. There is no automated alert in the platform that would have reached me in time to act. That cannot happen again."
- **Ravi:** "I flagged this exact data quality risk six weeks ago. It was accepted with no action. The incident happened exactly as I described it would. I am now writing the post-incident report. I don't know how to write 'I told you so' in a way that will actually change something."
- **Fiona:** "I spoke to the FCA while still waiting for Ravi's report. My control framework failed at the point of escalation. The FCA asked what controls failed. I didn't know. Under SM&CR, that is not acceptable."

**Feelings:**
- **Sarah:** Shaken; the gap between her accountability and her access to information became viscerally real at 11pm
- **Ravi:** Exhausted, vindicated, and frustrated simultaneously; the technical failure was anticipated and ignored
- **Fiona:** Exposed; the incident revealed that the governance framework was not yet fit for autonomous operation

**Pain points:**
- Sarah has no automated operational alert for agent-caused incidents — she learned about the failure through a contact centre manager, not the platform
- Decision logs are not accessible to non-technical users; it took 45 minutes to confirm the agent was the cause
- A known, documented data quality risk was accepted with no mitigation — the risk register is not integrated with the platform's deployment or monitoring controls
- The FCA escalation path assumed Fiona would have incident data available; the platform's incident reporting workflow had not been designed for regulatory notification timelines

**Opportunities:**
- Operational incident alerting: define a first-class incident notification channel for the operations lead, separate from the engineering on-call workflow — triggered by confidence score collapse, volume anomalies, or customer impact signals
- Real-time decision log access: a non-technical interface that lets Sarah answer "what did the agent do and why" within 5 minutes, without engineering support
- Risk-register-to-deployment integration: accepted risks with no mitigation action should block or throttle related agent autonomy levels — surfaced in the platform, not just in a document
- FCA notification workflow: a pre-defined, platform-supported process for generating a regulatory incident summary within 30 minutes of confirmation — including affected customer count, root cause classification, and control failure mapping

---

## Phase 5: Scaling with Governance — 10 to 100+ Agents

**What happens:** Post-incident, the programme governance is restructured. A formal Agent Governance Board is established with Sarah, Fiona, and the Head of Model Risk as standing members. The model risk team commissions an external methodology review for LLM agent assessment. Ravi's team builds a reusable legacy integration framework — a standardised connector library for COBOL/IBM MQ sources with built-in data quality validation and freshness SLAs. New agents are deployed at a controlled pace: three per month, each with a new pre-deployment governance package. By month 18, the fleet has grown to 67 agents. An "Agent Supervisor" role has been created; Sarah has hired two.

**Actions by actor:**
- **Sarah:** Transitions from shadow-reviewing individual agent decisions to governing the agent fleet operationally. Defines the escalation SLA framework — all agent incidents with consumer impact escalate to her within 15 minutes via a platform-generated notification. Her two Agent Supervisors manage day-to-day override monitoring. She now reads a daily operational summary generated by the platform, not compiled manually by a manager.
- **Ravi:** Builds and documents the COBOL/IBM MQ connector library — the first standardised integration pattern for legacy sources. Each connector includes a data quality manifest (field-level null rates, freshness SLA, known anomalies) visible to both engineering and governance teams. Model drift monitoring is upgraded to detect gradual performance decay, not just threshold breaches.
- **Fiona:** Presents the bank's governance framework to the FCA at a supervisory engagement. Uses the FINOS AI Governance Framework v2.0 alignment as the structural narrative. For the first time, she can map every deployed agent to its risk classification, accountable owner, governance sign-off status, and incident history from the platform's agent registry. The FCA requests a follow-up on GDPR Article 22 explainability.

**Touchpoints:** Agent Governance Board meeting cadence, pre-deployment governance package, COBOL/IBM MQ connector library, data quality manifest, model drift monitoring dashboard, FCA supervisory presentation, agent registry

**Thoughts:**
- **Sarah:** "The platform is starting to give me what I need. The daily operational summary is the first thing I read that tells me something useful without requiring an engineering translation."
- **Ravi:** "The connector library means the 8th agent integration takes one week, not eight. The data quality manifest means Fiona's team can see exactly what data an agent is consuming without asking me to explain it. That's the most useful thing I've built."
- **Fiona:** "I was able to answer the FCA's questions about our control framework accurately. I still couldn't answer the Article 22 explainability question. That's my next governance gap — and it will come up in the follow-up."

**Feelings:**
- **Sarah:** Growing confidence; the operational governance model is working; the Agent Supervisor role is proving its value
- **Ravi:** Satisfaction with the connector library; the platform is becoming more legible to non-engineers; the focus is shifting from firefighting to architecture
- **Fiona:** Cautious progress; the governance framework is evidenced; one remaining regulatory gap is known and scoped

**Pain points:**
- GDPR Article 22 explainability remains unresolved — no mechanism exists to generate a consumer-facing explanation of an agent decision at decision time
- Pre-deployment governance packages take 3 weeks to complete; at three agents per month, the programme is governance-gated rather than engineering-gated
- Agent fleet complexity creates new monitoring challenges: interactions between 67 agents create emergent behaviours that single-agent monitoring cannot detect
- The Agent Supervisor role is effective but not yet formally recognised in the bank's SM&CR accountability mapping — a regulatory gap Fiona has flagged

**Opportunities:**
- Consumer-facing decision explanation generator: at decision time, produce a plain-language statement of the basis for the agent's decision, mapped to Consumer Duty outcome language and GDPR Article 22 requirements — stored in the decision log, retrievable on demand
- Pre-deployment governance automation: machine-readable governance packages where standard elements (data quality manifest, agent registry entry, risk classification) are auto-populated from platform metadata — reduce manual effort from 3 weeks to 3 days
- Multi-agent interaction monitoring: add a cross-agent correlation layer to detect anomalous decision patterns that emerge from agent interactions — not visible at the single-agent monitoring level
- SM&CR accountability mapping update: formally register Agent Supervisors and the Agent Governance Board in the bank's accountability map, with explicit linkage to the agents under their oversight

---

## Phase 6: Operational Maturity

**What happens:** The agentic OS is core infrastructure. 140 agents are in production. Routine operations in payments, KYC, AML screening, and back-office reconciliation are handled predominantly by agents; Sarah's team of 400 has been rebalanced — 60 Agent Supervisors, 340 exception specialists working on cases agents cannot resolve. The bank passes its first formal FCA review of the AI estate, including an assessment of the Consumer Duty outcome framework and a spot-check of GDPR Article 22 explainability for five named agent decisions.

**Actions by actor:**
- **Sarah:** Manages the agent fleet operationally through the platform's daily summary, the Agent Governance Board, and escalation alerts. Has not reviewed an individual agent decision manually in four months — the Agent Supervisors manage that layer. Presents the operational governance framework at an industry conference. Uses the phrase "accountability infrastructure" to describe what the platform has become.
- **Ravi:** The connector library covers all major legacy integration patterns; the last bespoke IBM MQ adapter was decommissioned in Month 20. Model drift monitoring is fully automated with anomaly detection that flags gradual decay three weeks before it would breach performance thresholds. Has begun documenting the platform architecture for knowledge transfer — the institutional knowledge concentration risk is being addressed.
- **Fiona:** Sits in the FCA review room with the agent registry, the governance sign-off history, a sample of decision logs, and five consumer-facing decision explanations printed out. The FCA reviewer asks why a specific mortgage eligibility agent declined a named customer in June. Fiona pulls the decision log in 90 seconds. The explanation is accurate, complete, and in plain language. The FCA reviewer notes it as an example of best practice.

**Touchpoints:** Agent fleet operational summary, FCA formal review pack, consumer-facing decision explanation output, agent registry (live), platform knowledge transfer documentation

**Thoughts:**
- **Sarah:** "A year ago I couldn't tell the FCA why one agent made one decision. Now I can describe the governance framework for 140 agents in a way that makes sense to a regulator. That is a material change."
- **Ravi:** "The platform is documented. The connectors are standardised. The monitoring catches drift before it becomes an incident. I'm no longer the single point of failure. That's the thing I'm most proud of."
- **Fiona:** "The FCA reviewer called the decision explanation approach best practice. I want that formally documented in our supervisory correspondence. It's the clearest evidence we have that the governance investment was correct."

**Feelings:**
- **Sarah:** Confident in the operational governance model; proud of the Agent Supervisor team she has built
- **Ravi:** Satisfied with platform stability and reduced on-call burden; energised by architecture work rather than firefighting
- **Fiona:** Relief — real, earned, evidenced by a successful FCA review rather than a negotiated risk register

**Pain points:**
- Fleet size (140 agents) means the Agent Governance Board can no longer review every agent individually — a tiered review model is needed but not yet implemented
- Agent retirement is an emerging governance gap: some agents deployed in Phase 3 are approaching performance thresholds that would require decommissioning; there is no formal agent retirement workflow
- The Consumer Duty outcome mapping is complete for agents deployed since Phase 4; agents deployed in Phase 3 have retrospective mappings that are less rigorous
- Vendor model updates (the underlying LLMs) create periodic re-validation burdens; no automated pipeline exists to detect when a vendor update has changed agent behaviour

**Opportunities:**
- Tiered governance model: classify agents by risk level (e.g., consequential / operational / informational) with proportionate review cadences — full board review for consequential agents, automated monitoring with threshold alerts for lower tiers
- Agent retirement workflow: define lifecycle stages (active, under review, deprecated, retired) with governance triggers, consumer impact assessment, and platform deregistration steps — built into the agent registry
- Vendor model update pipeline: automated behavioural regression testing triggered by any underlying model version change — flagging drift before the update is applied to production agents
- Platform as institutional asset: publish the connector library, governance framework, and FINOS alignment mapping as a contribution to the industry — positions the bank as a governance leader ahead of anticipated FCA prescriptive requirements

---

## Key Moments of Truth

| Moment | Risk | Design imperative |
|---|---|---|
| **First agent goes to production** | If there is no agent registry at launch, agents will be tracked informally by nickname — creating governance and accountability gaps that are structurally invisible until an incident | A machine-readable agent registry with stable identifiers, risk classification, accountable owner, and governance status must exist before any agent is deployed, not retroactively |
| **First data quality incident in production** | The gap between clean staging data and dirty production data (NULLs, truncated fields, stale batch exports) will cause agent errors that appear to be model failures but are data failures | Data quality manifests (field-level null rates, freshness SLAs, known anomalies) must be a first-class deployment artefact, visible to operations and governance as well as engineering |
| **Operations lead learns of a consumer incident** | If Sarah cannot determine within 5 minutes that an agent caused an incident — and why — she cannot discharge her SM&CR accountability or respond to the FCA | A non-technical decision log interface, readable by operations leads without engineering support, is not a nice-to-have — it is an SM&CR compliance requirement |
| **FCA asks for explainability of a specific decision** | If Fiona cannot retrieve a plain-language explanation of a named agent decision within minutes, the regulatory relationship is damaged regardless of whether the decision was correct | Consumer-facing decision explanations must be generated and stored at decision time, not reconstructed retrospectively from logs |
| **First agent override rate data is available** | Override patterns are the most reliable early signal of agent quality issues, demographic bias, and edge-case failure modes — but only if they are captured structurally, not in a spreadsheet | Override capture must be a first-class platform feature, fed back automatically into eval pipelines and surfaced in the operational reporting layer |
| **Bank passes first FCA formal review of the AI estate** | This is the governance maturity test: can the bank demonstrate, with evidence, that every deployed agent has a designated accountable owner, a documented control framework, and an auditable decision record | The agent registry, governance sign-off workflow, decision log, and consumer explanation capability must collectively constitute a presentable, evidenced regulatory narrative — not a collection of disparate artefacts |

---

## Design Principles Derived from this Journey

1. **Accountability must be encoded, not assumed.** Every agent decision must have a designated accountable owner recorded in the platform at deployment time — not assigned informally after an incident. The SM&CR framework creates personal liability for senior managers; the platform must make that liability legible and evidenced.

2. **Data quality is a governance concern, not only a technical one.** The gap between staging and production data is the primary driver of agent errors in a legacy banking estate. Data quality manifests — covering field-level null rates, freshness SLAs, known anomalies — must be readable by operations and compliance teams, not just engineers. Agents with known data quality risks must have their autonomy level constrained accordingly.

3. **Decision logs must be operational, not forensic.** If a decision log can only be read by an engineer, it is an incident investigation tool, not a governance tool. The platform must produce decision records that an operations lead can read in under 5 minutes without engineering support, a compliance officer can use to answer FCA questions directly, and a customer can receive as a plain-language explanation.

4. **Confidence scoring is an operational control, not a metric.** High confidence should route to straight-through processing. Low confidence should trigger human review. This calibration must be exposed as a configurable operational control with a defined escalation threshold, visible in the daily operational summary — not buried in an engineering dashboard.

5. **Governance must be parallel and integrated, not sequential and gated.** Compliance and model risk functions produce governance gaps when they review agents after engineering has finished building them. Governance artefacts (agent registry entry, data quality manifest, Consumer Duty impact mapping, escalation SLA) should be co-created during design, not produced as sign-off prerequisites that create deployment queue backlogs.

6. **The legacy integration layer is a first-class platform concern.** COBOL mainframes, IBM MQ brokers, and batch data pipelines are not temporary constraints to be worked around — they are the permanent operational reality of UK incumbent banking. The platform must treat legacy connector standardisation, data latency management, and mainframe quirk documentation as core platform capabilities, not individual engineering tasks that accumulate as undocumented single-points-of-failure.
