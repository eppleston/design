# Empathy Map — Agentic AI Operating System for a UK Retail Bank

**User archetypes covered:**
1. **Operations leads** — accountable for outcomes, not technical (represented by Sarah, Head of Retail Operations)
2. **Platform engineers** — building and running the agent fleet against a legacy estate (represented by Ravi, Senior AI Platform Engineer)
3. **Risk & compliance officers** — FCA-accountable under SM&CR; need governance visibility (represented by Fiona, CRCO)

---

## SAYS
*Direct quotes across archetypes — things said in stand-ups, steering committees, incident reviews, and supervisory meetings*

- **[Sarah]** "The model risk dashboard tells me precision is 94%. I have no idea what that means for the 6% of customers my team has to clean up after."
- **[Sarah]** "Under Consumer Duty, I need to demonstrate that the outcome was fair. The agent's outcome report doesn't use the word 'fair' anywhere."
- **[Ravi]** "It's not the model that's failing — it's the data. The COBOL batch export truncates the postcode field at 6 characters. The agent has never seen a valid 7-character postcode in training."
- **[Ravi]** "Staging is clean. Production has 30 years of dirty data and IBM MQ routing rules nobody wrote down. The gap between the two is where all my incidents live."
- **[Fiona]** "GDPR Article 22 requires us to explain automated decisions to affected consumers. Show me where in the platform that explanation is generated."
- **[Fiona]** "I have a Prescribed Responsibility under SM&CR for this. If an agent causes consumer harm at scale, that's a personal accountability matter, not just an operational one."
- **[Sarah]** "There's no escalation SLA in the platform. My manager Slacks Ravi when something looks wrong. That's not a governance model."
- **[Ravi]** "I've built seven custom IBM MQ adapters. Three of them are critical dependencies. I am the only person who fully understands how they work. That is a risk I flag every quarter."
- **[Fiona]** "The FCA asked us to demonstrate how we would explain an AI-driven credit decision to a customer who challenged it. We could not demonstrate it."
- **[Sarah]** "My team is shadow-reviewing 5% of AML agent decisions manually. Not because the platform told us to — because I don't trust it enough not to."

---

## THINKS
*Internal beliefs and fears users hold but rarely say in formal settings*

- **[Sarah]** "If there's a major agent incident and the COO asks me what happened, I am going to have to say 'I don't know — I'd need to ask the engineering team.' That is an unacceptable position for someone with my accountability."
- **[Sarah]** "The ops team knows how to run the old process. If agents go wrong, I'm not sure they know what to check first."
- **[Ravi]** "The hallucination rate in the vendor's benchmark is 0.7%. On our production data — which is full of NULLs, truncated fields, and stale batch exports — it's closer to 4–6% on some flows. I haven't told anyone that number yet because I don't have a clean dataset to validate it against."
- **[Ravi]** "If I leave this job, the mainframe integration layer falls apart within six months. Nobody else has the institutional knowledge. That's a systemic risk the platform creates, not resolves."
- **[Ravi]** "The model risk validation process was designed for credit scorecards in 2015. It doesn't know what to do with an LLM agent that updates its prompt context dynamically."
- **[Fiona]** "Every 'green' status on the model risk dashboard is a negotiated position. It doesn't mean the risk is gone. It means the risk has been deemed acceptable by someone who may not fully understand it."
- **[Fiona]** "When something goes wrong at scale — 3,000 accounts incorrectly flagged, a payment cascade, a fair lending breach — the FCA's first question will be: what did the accountable senior manager know and when? My answer right now is: not enough and not in time."
- **[Fiona]** "Multi-agent workflows scare me most. When the AML agent and the payments routing agent interact, who owns the combined decision? The platform doesn't have an answer to that."
- **[Sarah]** "I am building my own audit log in SharePoint because the platform doesn't give me one I can use. That means I have two records that will eventually diverge."
- **[Ravi]** "The governance process is a bottleneck, but not because Fiona is obstructing — because her team is asking the right questions and the platform genuinely cannot answer them yet."

---

## DOES
*Observable behaviours — how users interact with the system, each other, and regulators*

- Operations managers manually compile a weekly agent decision summary by exporting CSV logs and annotating them in Excel — because the platform has no operational reporting layer
- Ravi runs daily automated evals against a golden dataset (maintained in a private Notion workspace) to detect model drift before it appears in production; these evals catch regressions the platform's own monitoring misses
- Fiona's compliance function samples agent decision logs fortnightly using a manual triage process designed for rules-based systems — it is not calibrated for probabilistic agent outputs
- Sarah's team maintains a shadow-mode review of 5% of AML agent decisions, logging disagreements in a shared spreadsheet that no automated process reads
- The platform engineering team uses an informal Slack channel as the primary escalation path for operational agent issues — there is no formal incident triage workflow in the platform itself
- Fiona commissions external legal opinions on AI-related regulatory exposure because she cannot get structured risk assessments from the internal platform or model risk team
- Ravi deploys all new agents in shadow mode for a minimum of two weeks, generating a decision log that is compared manually against human decisions to validate alignment — a process that is not standardised or integrated into the platform governance workflow
- Sarah attends platform engineering stand-ups periodically to get operational context that does not appear in any formal report
- The model risk team runs PRA SS 1/23 validation exercises using a checklist designed for statistical credit models — they add a narrative addendum for LLM agents but the addendum is not formally reviewed or standardised
- The FCA supervisory engagement log is maintained manually by Fiona's EA; platform agents are referenced by informal names ("the KYC doc agent", "the payments exception bot") with no stable identifiers that map to the platform's own agent registry

---

## FEELS
*Emotional states specific to the challenge of running autonomous agents in a regulated UK banking environment*

- **Accountability vertigo [Sarah, Fiona]:** Responsible under SM&CR for outcomes they cannot directly audit; the gap between formal accountability and actual information access is experienced as a constant, background anxiety
- **Invisible labour fatigue [Ravi]:** The work of keeping legacy integrations alive — undocumented IBM MQ adapters, manually patched COBOL export normalisation, bespoke NULL-handling logic — is operationally critical but invisible to programme governance; it accumulates and is never resourced
- **Explainability deficit [Fiona]:** The inability to answer the three FCA questions (what decision, on what basis, what counterfactual) for any given agent decision generates a specific, recurring anxiety every time a supervisory meeting approaches
- **False confidence pressure [all three]:** The programme's steering committee needs green RAG statuses; the platform's dashboards are calibrated to produce them; all three users know the green status conceals unresolved risk but feel organisational pressure not to escalate it
- **Control loss proportional to scale [Sarah]:** As agents handle more decisions per hour — from hundreds in pilot to tens of thousands in full production — the sensation of operational control diminishes even as system performance metrics improve
- **Isolation in complexity [Ravi]:** Being the primary carrier of institutional knowledge about legacy integration creates a specific professional vulnerability — if something breaks, he is the single point of failure; if he leaves, the platform degrades
- **Regulatory exposure asymmetry [Fiona]:** The pace of agent deployment is driven by commercial pressure and exec mandate; the pace of governance framework maturation is slower; the gap between them is experienced as personal regulatory exposure
- **Distrust of confidence [all three]:** Having observed the gap between vendor benchmark performance (0.7% hallucination rates) and real-world production behaviour, all three have internalised a scepticism about any system-generated confidence indicator
- **Anticipatory dread of the first major incident [all three]:** Not if but when — the awareness that a large-scale agent failure is likely before the governance framework is mature enough to contain it

---

## PAINS
*Concrete obstacles, grouped by domain*

### Regulatory
- PRA SS 1/23 model validation processes are designed for statistical models with defined input distributions; they have no validated methodology for LLM-based agents with emergent behaviours, dynamic prompt contexts, or multi-agent interaction effects
- GDPR Article 22 requires explainability for automated decisions with legal or significant effect; the platform has no mechanism to generate a consumer-facing explanation at decision time
- Consumer Duty requires demonstrable fair outcomes; the platform's output metrics are system performance indicators, not consumer outcome indicators — the translation between them does not exist
- SM&CR Prescribed Responsibilities create personal liability for accountable senior managers; the platform's information architecture does not produce the evidence needed to discharge that liability

### Technical
- Legacy data quality is a primary driver of agent errors: NULL fields, truncated strings, stale batch exports (up to 14 hours old), and inconsistent formats across seven legacy CRM systems create conditions that produce hallucinations in production that are absent in staging
- Agent drift and performance decay are detected retrospectively — the platform's monitoring is calibrated to alert on threshold breaches, not on the gradual degradation that precedes them
- Multi-agent orchestration creates emergent accountability gaps: when two or more agents interact to produce a combined decision, the platform does not assign a responsible owner to the outcome
- IBM MQ connectors and COBOL batch adapters are maintained by a small number of engineers with non-transferable institutional knowledge — a systemic concentration risk the platform has not addressed

### Organisational
- Escalation ownership is informally negotiated between operations and engineering with no formal SLA, ownership model, or platform-enforced triage workflow
- The model risk validation queue (6 weeks) is a deployment bottleneck that does not correspond to risk reduction — agents queue regardless of risk profile
- Governance documentation describes agent architecture rather than agent behaviour in specific decision contexts — it is not usable for Consumer Duty accountability or FCA supervisory engagement
- Knowledge concentration in one or two engineers creates a platform fragility that is structurally invisible to programme governance

---

## GAINS
*What success looks like — desired outcomes for each archetype*

- **[Sarah]** A decision log she can read without engineering support: structured, searchable, and mapped to the operational domain — not raw API call logs or model performance metrics
- **[Ravi]** A validated, reusable legacy integration pattern so each new agent does not require a bespoke COBOL/IBM MQ adapter that becomes a single-point-of-failure dependency
- **[Fiona]** The ability to walk into an FCA supervisory meeting and describe the control framework accurately, completely, and without managed omission of known gaps
- **[all three]** Confidence scoring surfaced as a first-class operational signal — so human oversight is applied proportionately (high uncertainty = human review; high confidence = straight-through processing), not as a blanket audit of all decisions
- **[Ravi]** Data quality issues detected and flagged upstream — at source, before agents consume them — rather than discovered through anomalous decision outputs in production
- **[Sarah]** A formal escalation workflow built into the platform with defined ownership, SLAs, and notification routing — not an informal Slack channel
- **[Fiona]** Consumer-facing decision explanations generated at decision time for any agent decision that affects a consumer outcome, satisfying both GDPR Article 22 and Consumer Duty explainability requirements
- **[all three]** A single, authoritative agent registry with stable identifiers, current production status, risk classification, governance sign-off state, and assigned accountable owner — the source of truth for both operational management and regulatory reporting
- **[Sarah, Fiona]** Multi-agent decision attribution: a clear record of which agent contributed which input to a combined decision, enabling both operational root-cause analysis and regulatory accountability mapping
