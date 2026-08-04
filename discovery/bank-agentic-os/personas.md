# Personas — Agentic AI Operating System for a UK Retail Bank

---

## Persona 1: Sarah Okafor, 46 — Head of Retail Operations

**Tagline:** "I've been given accountability for a system I cannot interrogate."

### Demographics
- Age: 46
- Role: Head of Retail Operations
- Bank: Large UK incumbent (NatWest / Lloyds scale, ~18,000 retail employees)
- Reports to: Chief Operating Officer
- Team: 400 operations staff across payments processing, KYC/AML screening, back-office reconciliation, and customer service escalations

### Background
Sarah has spent 20 years in retail banking operations, progressing from a payments processing analyst to her current role overseeing four operational domains. She has deep institutional knowledge — she knows exactly which IBM MQ queues carry time-critical payment batches, which compliance teams get called first when a sanctions hit arrives, and how the overnight batch reconciliation windows create a hard freeze on certain transactions between 02:00–04:30. Eight months ago her COO handed her a new mandate: "lead the operational transition to the agentic platform." She did not ask for it. She has no ML background. She has started attending the fortnightly platform review meetings and is the only person in the room who cannot read a model performance dashboard. Under SM&CR, she holds the Prescribed Responsibility for operational resilience in retail. She is acutely aware that if an agent causes consumer harm at scale, the FCA may name her personally.

### Goals
- Understand, at decision level, what each agent is doing and why — not just whether it is "performing" against an F1 score
- Have a clear, documented escalation chain so she knows who owns an agent error at 11pm on a Sunday
- Build an audit trail she can present to the FCA or internal audit without her platform engineering team having to translate it for her
- Retain enough human oversight in high-stakes flows (AML flagging, credit decisions) to satisfy Consumer Duty obligations without re-staffing at the old scale
- Reduce the time her team spends on routine exception triage so they can focus on edge cases agents genuinely cannot handle

### Frustrations
- Agent monitoring dashboards are built for engineers — precision/recall, latency percentiles, pipeline throughput — none of which maps to the operational or regulatory questions she needs to answer
- Escalation ownership is informally negotiated between her operations managers and Ravi's platform team, with no formal SLA or ownership model in the platform itself
- When an agent makes a wrong decision, she cannot reconstruct why without asking Ravi's team to pull logs — a process that takes hours and produces output she cannot interpret directly
- The governance documentation produced by the model risk team describes agent architecture, not agent behaviour in specific decision contexts — useless for Consumer Duty accountability
- She is building her own manual audit log in a shared SharePoint folder because the platform provides no operationally usable decision record

### Behaviors
- Reviews a weekly operational summary compiled manually by one of her managers from agent log exports
- Attends the fortnightly AI platform review but asks questions about operational impact rather than model metrics, which creates friction with the engineering team
- Keeps a printed copy of the escalation procedure on her desk because the platform's notification system sends alerts to a generic ops inbox that three people monitor inconsistently
- Has implemented a shadow-mode check on the AML flagging agent — her team manually reviews a 5% sample of decisions to build confidence before full handoff
- Speaks to the bank's FCA relationship manager quarterly; the last three conversations have included questions about Consumer Duty explainability that she cannot currently answer

### Tech Comfort
**Low-moderate.** Proficient in Excel, SharePoint, and the bank's core banking portal. Uses the agent platform primarily as a consumer of outputs, not a configurator. Has no appetite for tools that require API access or SQL queries to get operational insight.

### Quote
> "If the FCA asks me to explain why our KYC agent declined 4,000 accounts last Tuesday, I need to be able to answer that without calling Ravi. Right now, I can't."

---

## Persona 2: Ravi Patel, 31 — Senior AI Platform Engineer

**Tagline:** "It works perfectly in staging. Production is a 30-year-old mainframe running batch jobs with no documentation."

### Demographics
- Age: 31
- Role: Senior AI Platform Engineer (AI/ML Infrastructure)
- Bank: Same large UK incumbent
- Reports to: Head of AI Engineering
- Team: 6 engineers responsible for agent infrastructure, legacy system connectors, observability, and model lifecycle management

### Background
Ravi joined the bank 18 months ago from a Series B payments fintech where he built real-time fraud detection pipelines on clean, event-driven infrastructure. He was hired specifically to build the agentic platform — the bank's mandate was to move fast. He quickly discovered that "fast" has a different meaning when your data flows through COBOL jobs on an IBM z/OS mainframe, your customer data is distributed across seven legacy CRM systems with inconsistent field formats, and your IBM MQ message broker has undocumented routing rules written in 2003. His agent framework runs on Kubernetes; his data sources run on hardware from a different era. He has written seven custom adapters to normalise data coming out of batch exports into formats agents can consume reliably. Three of those adapters are now critical dependencies with no documentation beyond his own internal wiki pages. He is one of two engineers on the team who fully understands the mainframe integration layer.

### Goals
- Establish a reliable, reusable integration pattern for connecting agents to legacy COBOL/IBM MQ systems so each new agent does not require a bespoke connector
- Surface data quality issues upstream — at source — rather than discovering them when an agent produces an anomalous decision in production
- Build observability tooling that distinguishes confident agent decisions from uncertain ones, so the operations team can apply human oversight proportionately rather than blanket-reviewing all outputs
- Get governance and compliance sign-off processes out of informal email chains and into a structured workflow so agent deployments do not stall in approval queues
- Reduce his team's on-call burden by improving the platform's self-healing and alerting capabilities — currently a mainframe timing window miss causes a cascade that pages him at 3am

### Frustrations
- Legacy data quality is the single biggest driver of poor agent decisions: truncated strings, NULL customer addresses, stale batch exports that are 14 hours old by the time an agent reads them — none of this was documented when he joined, and each discovery costs a sprint
- His staging environment uses a sanitised data snapshot; production data has edge cases that appear roughly once in 10,000 records and cause agent hallucinations in ways he cannot reproduce in testing
- Model risk governance (PRA SS 1/23) requires a formal model validation before any agent goes into production, but the validation team has a 6-week queue and uses a checklist designed for statistical credit models, not LLM-based decision agents
- Fiona's compliance team asks for "explainability reports" but cannot specify what format, level of detail, or regulatory mapping they need — he has produced three different formats and none has been approved
- The FINOS AI Governance Framework v2.0 alignment exercise has been added to his team's backlog without additional headcount or deadline adjustment

### Behaviors
- Runs daily automated evals against a golden dataset to detect model drift before it surfaces in production decisions — this currently catches regressions the platform's own monitoring misses
- Maintains a private Notion workspace documenting undiscovered mainframe quirks, data format anomalies, and IBM MQ edge cases — institutional knowledge that should be in the platform but isn't
- Joins the operations team's weekly review meeting specifically to hear about decision quality issues that don't appear in his metrics (Sarah's manual audit log is his best signal of ground truth)
- Deploys new agents in shadow mode for two weeks minimum — logging decisions without actioning them — before pushing to live
- Is currently mentoring a junior engineer to cross-train on the mainframe integration layer because he is aware of the single-point-of-failure risk he personally represents

### Tech Comfort
**High.** Works across Python, Kubernetes, Kafka, and LLM orchestration frameworks. Comfortable with cloud-native tooling. His bottleneck is institutional knowledge, not technical capability.

### Quote
> "The agent hallucinates when it hits a NULL address field in the customer record. That NULL comes from a COBOL batch job that's been running since 2007. Nobody documented it. Now it's my problem."

---

## Persona 3: Fiona McLean, 53 — Chief Risk & Compliance Officer

**Tagline:** "I have to sign off on agents I cannot interrogate, under a regime where I'm personally liable if they cause consumer harm."

### Demographics
- Age: 53
- Role: Chief Risk & Compliance Officer
- Bank: Same large UK incumbent
- Reports to: CEO; sits on the Board Risk Committee
- SM&CR status: Senior Manager with Prescribed Responsibilities for model risk governance, consumer protection, and regulatory compliance

### Background
Fiona has spent 25 years in financial services risk and compliance, the last eight at this bank. She led the bank's Consumer Duty implementation programme in 2023 and is the primary point of contact for the FCA on AI governance matters. She has deep regulatory expertise — she can cite the relevant sections of PRA SS 1/23, Consumer Duty outcomes, and GDPR Article 22 without checking notes. She is not technical. She relies on Ravi's team and the model risk management function to translate agent behaviour into regulatory language. Her core problem is that this translation is incomplete, inconsistent, and always retrospective — she learns about agent behaviour issues after they have occurred, not in time to prevent them. Under SM&CR, her Prescribed Responsibilities make her personally accountable for consumer harm caused by models under her purview. The FCA has signalled through supervisory engagement that it will expect banks to demonstrate explainability of AI decisions at a consumer level, not just at a system level. She cannot currently do this.

### Goals
- Obtain structured, queryable, regulatory-grade audit records of agent decisions — specifically the three questions the FCA will ask: what decision was made, on what basis, and what would have happened under a different scenario
- Establish a governance workflow that gives her function visibility of agents before they are deployed in production, not after an incident
- Map every deployed agent to the specific Consumer Duty outcome it affects and the control that mitigates consumer harm risk — currently this mapping does not exist in a usable form
- Build an FCA-presentable account of the bank's AI control framework that is accurate, complete, and does not require managed omission of known gaps
- Reduce her reliance on Ravi's team for regulatory translation by giving her function direct access to decision-level explanations in non-technical language

### Frustrations
- Agent decisions at scale — thousands per hour across KYC, payments, and credit — create a compliance monitoring problem her team cannot solve with manual sampling; she needs automated surveillance with configurable thresholds, not exports she pipes into Excel
- PRA SS 1/23 model validation is designed for statistical models with defined input distributions; her model risk team does not have a validated methodology for assessing LLM-based agents with emergent behaviours, and the 6-week validation queue is bottlenecking deployments without actually reducing risk
- GDPR Article 22 requires that automated decisions with legal or significant effect on consumers be explainable to the individual — she has not seen a mechanism in the current platform to generate a consumer-facing explanation at decision time
- When an agent incident occurs, the post-incident review always reveals that the agent had been exhibiting anomalous behaviour for days before anyone in the operations or platform team flagged it to her function — she is always the last to know
- Multi-agent workflows create accountability gaps: when a payment routing agent and an AML screening agent interact to produce a combined decision, it is currently impossible to assign a single responsible owner to that outcome

### Behaviors
- Reviews a fortnightly model risk summary prepared by the Head of Model Risk Management — currently a narrative document with no structured data, produced manually
- Meets with the FCA relationship manager twice a year; the last supervisory meeting included a request for a demonstration of how the bank would explain an AI-driven credit decision to a customer who challenged it — she could not demonstrate this
- Has commissioned an external legal opinion on the bank's Consumer Duty exposure from agentic AI decisions — the opinion identified eight unmitigated risk areas
- Attends every incident review involving an agent decision error and consistently asks the same question: "What would have happened to a different customer in the same circumstances?" — a question the current platform cannot answer
- Is drafting an internal AI governance policy aligned to FINOS AI Governance Framework v2.0 but is blocked on the technical controls section because she cannot get a confirmed list of deployed agents with their current production status

### Tech Comfort
**Low.** Uses the platform through dashboards and reports prepared by others. Her instinct is to ask for evidence; her frustration is that the platform produces logs and metrics, not evidence. She does not distinguish between an LLM and a rules-based system architecturally, but she is precise about what she needs in terms of decision accountability and audit trail.

### Quote
> "I don't need to understand how the model works. I need to be able to walk into an FCA supervisory meeting and explain, with evidence, why it made the decision it made and what would have happened if it hadn't. Right now I cannot do that."
