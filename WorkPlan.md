# Wind Energy Ontology (WEO) — Extended Work Plan

**Period:** April 2026 – December 2027 (~21 months, ~42 biweekly meetings)  
**Cadence:** Biweekly meetings  
**Methodology:** Community-driven lifecycle drawing on Methontology, Ontology Development 101, UPON, and the BFO Community method — aligned with IEA Wind Task 43 Recommended Practice (Feb 2026)  
**Upper-level ontology:** BFO 2020 (ISO/IEC 21838-2)  
**Semantic neighbours:** OEO, IOF-Core, CCO  
**Related WEO-ecosystem artefacts:** WINDPROFILE (lifecycle phases & activities taxonomy)  
**Resources to re-engineer:** RDS for WPS, IEC 61400 series, WindIO, IEA Task 43 WRA Data Model & Glossary, SCADA alarm taxonomies  

---

## Design Philosophy: A Bridge, Not an Encyclopedia

WEO is a **mid-level domain ontology**. Its purpose is to provide **comprehensive coverage** of the wind energy domain at a level of abstraction that bridges upward to BFO / OEO / IOF-Core and downward to specialised task-level and application-level ontologies (RDS for WPS, WINDPROFILE, FMECA4WT, future application ontologies, etc.).

This means:

- **Comprehensive:** Every major conceptual area of the wind energy domain should have a home in WEO — no white spots on the map.
- **Not granular:** WEO defines the top-level classes and key relations for each area, but does NOT drill down into detailed component hierarchies, exhaustive failure-mode lists, or application-specific data schemas. That granularity is the scope of downstream ontologies that import and extend WEO.
- **Bridge function:** For each class, WEO establishes (a) its BFO classification, (b) its alignment to OEO / IOF-Core where overlap exists, and (c) clear extension points where specialised ontologies can attach.

---

## Plan Overview

| Phase | Period | Meetings | Focus |
|-------|--------|----------|-------|
| **1 — Initiation & Foundation** | Apr–Jun 2026 | 1–6 | Governance, ORSD, alignment survey, BFO backbone |
| **2 — Core Modelling I** | Jul–Dec 2026 | 7–18 | Blocks B1–B5 (material entities, sites, roles, lifecycle, processes) |
| **3 — Core Modelling II** | Jan–Jun 2027 | 19–30 | Blocks B5 (continued), B6–B8 (process attributes, qualities, information content entities) + inter-block relations & alignment |
| **4 — Validation & Use Cases** | Jul–Sep 2027 | 31–36 | CQ testing, pilot use cases, expert review, axiom enrichment |
| **5 — Release & Outreach** | Oct–Dec 2027 | 37–42 | v1.0.0 release, PURL, documentation, archiving, dissemination, future roadmap |

---

## The 8 Modelling Blocks

These are the major conceptual directions WEO must cover. They roughly correspond to branches of BFO and key OEO patterns. The blocks are thematic workstreams — not necessarily separate OWL files. Whether to split into modules is decided during Phase 2.

Each block will be worked to a **"bridging depth"** — enough structure to be useful for alignment and annotation, with explicit hooks for future refinement by specialised ontologies.

| # | Block | BFO / OEO anchor | Bridging depth (indicative) | Key open question |
|---|-------|-------------------|----------------------------|-------------------|
| **B1** | Material entities | `bfo:material entity` | Top-level physical things: wind turbine, wind farm, substation, met mast, grid infrastructure, environmental media. Component breakdown only to the level needed for processes and qualities to attach (e.g. rotor, nacelle, tower, foundation — but NOT individual bolt types). | Where to draw the line vs. RDS for WPS |
| **B2** | Sites & spatial | `bfo:site`, `bfo:spatial region` | Wind farm site, offshore/onshore distinction, grid connection point, resource assessment area, spatial regions relevant to meteorology. | Relationship between `site` and `spatial region` in BFO |
| **B3** | Roles & agents | `bfo:role`, `oeo:agent` (+ possibly "resource") | Stakeholder roles (developer, operator, OEM, TSO, regulator, insurer), organisational agents. Possibly "resource" as a role bearer. | Whether "resource" is a role, a disposition, or something else; alignment with OEO's agent hierarchy |
| **B4** | Lifecycle stages | WINDPROFILE taxonomy | Wind energy project lifecycle phases (development, construction, commissioning, operation, maintenance, decommissioning, repowering). WINDPROFILE already sketches these — WEO provides BFO-anchored classes that WINDPROFILE activities can attach to. | How to align lifecycle phases with BFO — are they temporal parts of a process, process profiles, or something else? Open design question. |
| **B5** | Processes | `bfo:process` (3 sub-directions) | **(a) Planned processes** — installation, inspection, repair, energy yield assessment, etc. **(b) Natural phenomena** — wind flow, wake propagation, icing, lightning, etc. **(c) Agent activities** — curtailment decisions, trading, permitting acts. Detailed activity taxonomies belong in WINDPROFILE; WEO provides the bridging classes. | The tripartite split needs validation against BFO's process hierarchy. Are "phenomena" just processes with no agent? Is "agent activity" a subclass of planned process? |
| **B6** | Process attributes | `oeo:process attribute` pattern | Capacity factor, availability, efficiency, load factor — attributes that characterise processes but are not qualities of material entities. Follow OEO's existing pattern. | Ensure consistency with OEO; clarify which wind-specific attributes belong here vs. in B7. |
| **B7** | Qualities | `bfo:quality` | Wind speed, wind direction, turbulence intensity, air density, power output, energy yield, noise level. Qualities inhering in material entities or sites. | Alignment with OEO's treatment of energy as a quality. Which measured quantities are qualities vs. process attributes (B6) vs. information content entities (B8). |
| **B8** | Information content entities | `iao:information content entity` | SCADA data point, alarm event, power curve, load case, wind resource map, digital twin model, certification document, simulation result. The largest and most heterogeneous block. WEO defines **top-level categories** and their key relationships, NOT every possible data schema. | Scope management — this block could explode. Need a clear "stop rule": WEO defines genus-level ICE classes and `isAbout` links; everything below is application-level. |

---

## Iterative Micro-Cycle

Every modelling block follows the same workflow, adapted from established ontology engineering methods:

1. **Engage domain specialists** — Gather competency questions, seed terms, and open modelling questions for the block.
2. **Prototype / improve** — Sketch concepts (spreadsheets, Figma/Miro), then formalise in Protégé. Stay at bridging depth.
3. **Publish / deploy** — Merge via PR, tag sprint release, update TechnoPortal.
4. **Evaluate & refine** — Review in the biweekly meeting; test against CQs; run reasoner checks (HermiT/ELK).

---

## Phase 1 — Initiation & Foundation (Apr–Jun 2026 · Meetings 1–6)

### 1.1 Governance, Tooling & Working Group (Meetings 1–2)

Establish a governance model with clear roles: Ontology Lead (stewardship, tie-breaking), Knowledge Engineering Reviewer (logical/OWL correctness), Domain Reviewers (semantic correctness), and Module Sub-Group Leads (assigned per block in Phases 2–3). If KG use cases are planned, include a data engineer.

Set up the working environment:
- **GitHub repository** (WeDoWind/wind-energy-ontology) as the single source of truth — `dev` → `main` branch workflow, PR-based changes, CHANGELOG, Architecture Decision Records in the wiki.
- **WeDoWind Information Modelling Community** for broader engagement and communication.
- **CI pipeline** (GitHub Actions): automated reasoner check + ROBOT reports on every PR.
- **Toolchain agreement:** Protégé (desktop) + WebProtégé (lightweight review) + shared spreadsheets for term collection + Figma/Miro for concept sketches.
- **IRI scheme:** Agree on naming conventions (e.g. `https://w3id.org/weo/WEO_00000001`), annotation standards (rdfs:label, IAO definition, term tracker annotation). PURL registration deferred to Phase 5.

### 1.2 Ontology Requirements Specification (Meetings 2–4)

Before modelling, identify 3–5 concrete use cases WEO must support (FAIR annotation, SCADA harmonisation, KG construction, RAG grounding, etc.) — these drive the competency questions and help determine bridging depth per block.

For each use case, assess the required expressiveness level along the spectrum: controlled vocabulary / SKOS → lightweight ontology → full OWL DL → OWL DL + SWRL rules. WEO targets OWL DL for its core, but some branches (e.g. alarm taxonomies in B8) may start as SKOS and be promoted later.

Produce the **Ontology Requirements Specification Document (ORSD)**:
- Purpose & scope statement formalising WEO's four objectives as measurable goals.
- ≥50 competency questions, tagged by block (B1–B8).
- Scope boundary matrix: what is WEO vs. what is delegated to WINDPROFILE, RDS for WPS, application ontologies.
- Pre-glossary: ~200 seed terms extracted from IEC 61400, Task 43 Glossary, WindIO, literature.

### 1.3 Alignment Survey & BFO Backbone (Meetings 4–6)

Survey and document existing artefacts to decide the import/alignment strategy for each:
- **Import directly:** BFO, RO subset, IAO subset, UO (Unit Ontology)
- **Align via bridging axioms:** OEO (`owl:equivalentClass` / `rdfs:subClassOf`), IOF-Core
- **Re-engineer:** RDS for WPS taxonomy, WindIO schemas, Task 43 Glossary terms
- **Reference:** Dublin Core, Schema.org, SKOS, CCO

Create stub classes for each of the 8 blocks at the first level below BFO. This is the skeleton that all subsequent modelling will flesh out. Circulate the backbone diagram for community feedback.

### Phase 1 Deliverables
- [ ] Governance charter & CONTRIBUTING.md
- [ ] GitHub repo with CI
- [ ] ORSD v1.0 (≥50 CQs, use cases, scope boundaries, per-block expressiveness)
- [ ] Alignment survey document
- [ ] **WEO v0.1.0** — BFO backbone with stub classes for B1–B8, on TechnoPortal

---

## Phase 2 — Core Modelling I (Jul–Dec 2026 · Meetings 7–18)

Six months dedicated to blocks B1–B5. Each block gets ~2 meetings of focused attention, but blocks overlap and inform each other.

### Block B1: Material Entities (Meetings 7–8)

Define the top-level physical things and their part-of / located-in relationships, at bridging depth.

- Wind turbine, rotor, nacelle, tower, foundation (onshore + offshore variants: monopile, jacket, floating platform).
- Wind farm as an aggregate of turbines + balance-of-plant.
- Substation, transformer, cables (inter-array, export).
- Meteorological mast, lidar, sodar.
- Grid infrastructure at the connection point level.
- Environmental media: atmosphere, seabed, soil (as relevant to wind resource).

**Bridging principle:** Component breakdown goes only as far as needed for processes (B5) and qualities (B7) to attach meaningfully. E.g. "gearbox" exists because "gearbox failure" is a key process and "gearbox oil temperature" is a key quality. Individual gear teeth do not.

Map to OEO's `wind energy converting unit` and related classes. Identify extension points for RDS for WPS.

### Block B2: Sites & Spatial (Meetings 9–10)

Represent where things are and what characterises locations.

- Wind farm site (BFO `site`), offshore vs. onshore as disjoint subclasses.
- Grid connection point, point of common coupling.
- Resource assessment area, exclusion zone.
- Spatial regions relevant to meteorology (measurement height, rotor-swept area, atmospheric boundary layer).

**Open question to resolve:** BFO distinguishes `site` (a three-dimensional immaterial entity) from `spatial region` (a purely geometric entity). Decide which WEO concepts fall under each, document as an ADR.

### Block B3: Roles & Agents (Meetings 11–12)

Represent who does what in the wind energy domain.

- Agent types: organisation, person — aligned with OEO's agent hierarchy.
- Roles: developer, owner, operator, OEM, independent service provider, TSO/DSO, regulator, insurer, certifier, investor, researcher.
- The concept of "resource" (technician, crane, vessel as a resource for a maintenance process). Discuss whether this is a BFO `role`, a `function`, or a separate pattern. Resolve and document as an ADR.

### Block B4: Lifecycle Stages (Meetings 13–14)

Provide BFO-anchored classes for major lifecycle phases, to which WINDPROFILE activities can attach.

- Lifecycle phases: development, permitting, financing, construction, commissioning, operation, maintenance, decommissioning, repowering.
- WINDPROFILE already defines these as a taxonomy. WEO's job is to determine their BFO classification and provide the semantic backbone.

**The core open question — what are lifecycle stages in BFO?** Options to evaluate:
1. **Temporal parts of a process:** The wind farm project is a BFO `process`; each stage is a temporal part.
2. **Process profiles:** BFO 2020's `process profile` for phase characterisation.
3. **Defined classes via temporal constraints:** Stages defined by temporal relation to the project and the types of activities occurring within them.

Dedicate meeting 13 to a structured design discussion. Pick an approach, document as an ADR, then formalise in meeting 14 and map WINDPROFILE phases onto it.

### Block B5: Processes — First Pass (Meetings 15–18)

Define the top-level process classes, organised into three sub-directions. This is a large block; the first pass establishes high-level structure, with refinement continuing in Phase 3.

**(a) Planned processes** (Meetings 15–16)  
Processes intentionally initiated by agents: wind resource assessment, energy yield assessment, micrositing, installation, grid connection procedure, commissioning test, inspection, scheduled maintenance, corrective repair, repowering, decommissioning. These are the processes WINDPROFILE activities will detail. WEO provides genus-level classes.

**(b) Natural phenomena** (Meeting 17)  
Processes occurring without intentional agent involvement: wind flow, atmospheric turbulence, wake propagation, icing event, lightning strike, wave loading, tidal action, seismic event, erosion.

**Open question:** In BFO all of these are `process` instances. The planned-vs-natural distinction is pragmatically useful but not BFO-native. Discuss whether to formalise as a subclass distinction or to capture the difference via roles/dispositions. Check OEO and IOF-Core patterns.

**(c) Agent activities** (Meeting 18)  
Decisions, transactions, and regulatory acts: curtailment decision, PPA execution, permit issuance, grid dispatch instruction, insurance claim. Keep at bridging depth — detailed activity taxonomies belong in WINDPROFILE or domain-specific ontologies.

### Phase 2 Deliverables
- [ ] **WEO v0.2.0** — Blocks B1–B4 formalised + B5 first pass
- [ ] ADRs for: component-depth boundary (B1), site vs. spatial region (B2), "resource" concept (B3), lifecycle-stage BFO classification (B4), planned-vs-natural process split (B5)
- [ ] Alignment notes for OEO / IOF-Core / WINDPROFILE per block
- [ ] Updated CQ coverage matrix
- [ ] Published on TechnoPortal

---

## Phase 3 — Core Modelling II (Jan–Jun 2027 · Meetings 19–30)

Six months dedicated to the remaining blocks, continued refinement of B5, and cross-cutting inter-block relations.

### Block B5 (continued): Process Refinement (Meetings 19–20)

Revisit the three sub-directions in light of B6 and B7 now being modelled. Ensure processes have the right attachment points for attributes and qualities. Formalise any remaining top-level process classes.

### Block B6: Process Attributes (Meetings 21–22)

Represent measurable characteristics of processes (not of material entities), following OEO's established `process attribute` pattern.

- Key wind-specific process attributes: capacity factor, availability (time-based, energy-based), load factor, curtailment rate, mean time between failures, mean time to repair.
- Clarify the boundary with B7: "rated power" is a quality of a turbine, but "capacity factor" is an attribute of the power generation process.

Consult with OEO developers if ambiguities arise.

### Block B7: Qualities (Meetings 23–25)

Represent measurable properties that inhere in material entities and sites.

- Wind speed, wind direction, wind shear, turbulence intensity (qualities of sites / atmospheric regions).
- Air density, temperature, humidity, pressure (qualities of atmosphere).
- Power output, reactive power, voltage, frequency (qualities of electrical entities).
- Noise level, vibration amplitude (qualities of turbines).
- Energy yield / AEP — may be process attributes (B6) rather than qualities; resolve the boundary.
- Weibull parameters — qualities of a site, of a dataset (B8), or of a statistical model? Discuss and resolve.

Adopt OEO's convention of treating energy as a quality and document the reasoning. Import or align with UO / QUDT for units.

### Block B8: Information Content Entities (Meetings 26–30)

Represent digital artefacts and information objects — at bridging depth. This is the largest block, so five meetings are allocated.

**Scope management rule:** WEO defines ICE classes at the level of "what kind of information object is this?" and "what does it describe?" (via `iao:is_about` links to B1–B7 entities). It does NOT define internal structure, field names, file formats, or vendor-specific schemas. Those are application-level.

**Proposed top-level ICE categories (to be refined):**

- **Measurement data:** SCADA data point, met mast measurement, lidar measurement, power curve measurement.
- **Alarm / event records:** SCADA alarm, condition monitoring alert, grid event notification.
- **Assessment & analysis artefacts:** wind resource map, energy yield assessment report, load case definition, structural analysis result.
- **Models & simulations:** simulation model, digital twin model, wake model, aero-elastic model.
- **Normative / regulatory documents:** technical standard (IEC 61400-x), certification document, type certificate, permit.
- **Operational records:** work order, inspection report, maintenance log, availability report.
- **Contractual / commercial documents:** power purchase agreement, grid connection agreement, insurance policy.

**Strategy:**
1. Meetings 26–27: Agree on the top-level ICE taxonomy. Define `iao:is_about` links to B1–B7 entities.
2. Meetings 28–29: Formalise classes and key object properties. For each category, identify where downstream ontologies would attach.
3. Meeting 30: Review against CQs. Enforce the scope boundary.

IAO provides the upper hierarchy. OEO's `oeo-model` module has patterns for data and model entities.

### Inter-Block Relations & Alignment (woven through Meetings 19–30)

As all 8 blocks take shape, explicitly define the key cross-block object properties:

- `hasPart` / `partOf` (B1 internal, B1↔B2)
- `locatedIn` / `hasSite` (B1↔B2)
- `hasRole` / `roleOf` (B3↔B1, B3↔agents)
- `occursIn` (B5↔B4 lifecycle stage)
- `hasParticipant` / `participantIn` (B5↔B1, B5↔B3)
- `hasProcessAttribute` (B5↔B6)
- `hasQuality` / `qualityOf` (B7↔B1, B7↔B2)
- `isAbout` (B8↔everything)
- `isOutputOf` / `isInputOf` (B8↔B5)

Produce a single inter-block relation diagram. Produce the formal OEO bridge module (`weo-bridge-oeo.owl`).

### Phase 3 Deliverables
- [ ] **WEO v0.3.0** — All 8 blocks at bridging depth
- [ ] Inter-block relation diagram
- [ ] `weo-bridge-oeo.owl` alignment module
- [ ] ADRs for: process-attribute vs. quality boundary (B6/B7), ICE scope boundary (B8), energy-as-quality convention (B7)
- [ ] Updated CQ coverage (target: ≥60%)
- [ ] Published on TechnoPortal

---

## Phase 4 — Validation & Use Cases (Jul–Sep 2027 · Meetings 31–36)

### 4.1 Competency Question Testing (Meetings 31–32)

Translate all CQs into SPARQL / DL queries. Run against WEO populated with representative instance data (e.g. IEA 15-MW reference turbine in a reference wind farm). Track pass/fail; gap analysis feeds targeted fixes. Target: ≥80% CQ coverage.

### 4.2 Axiom Enrichment (Meetings 33–34)

- Add existential restrictions where they apply at bridging level: e.g. `WindTurbine ⊑ ∃hasPart.Rotor`, `OffshoreWindFarm ⊑ ∃locatedIn.OffshoreSite`.
- Add disjointness axioms between key sibling classes.
- Review every class definition for quality (singular nouns, genus–differentia form, no unnecessary acronyms).
- Run OOPS! and fix critical pitfalls.

### 4.3 Pilot Use Cases (Meetings 34–36)

Select 2–3 use cases and run concrete pilots:

1. **FAIR metadata annotation:** Annotate a sample dataset on the Open Energy Platform or WeDoWind using WEO terms.
2. **KG construction:** Populate a small knowledge graph of a reference wind farm and run SPARQL queries.
3. **SCADA alarm harmonisation:** Map vendor-specific SCADA alarm codes to WEO's B8 ICE classes, showing how the bridge works.

Involve industry partners where possible. Document each as a technical note.

### 4.4 Expert Review (Meeting 36)

Organise a dedicated review session with external experts (OEO team, IOF community, industry, broader IEA Task 43). Post a public call for feedback via GitHub Discussions, WeDoWind, and LinkedIn. Collect and prioritise feedback for Phase 5.

### Phase 4 Deliverables
- [ ] CQ test report (≥80% coverage)
- [ ] **WEO v0.4.0** — Axiom-enriched, fully annotated
- [ ] 2–3 pilot use case write-ups
- [ ] External review report with prioritised action items

---

## Phase 5 — Release & Outreach (Oct–Dec 2027 · Meetings 37–42)

### 5.1 Release Preparation (Meetings 37–39)

Execute the release checklist:
- [ ] Logical consistency (HermiT + ELK, no unsatisfiable classes)
- [ ] Definition quality check (genus–differentia; no ambiguous acronyms)
- [ ] Backwards-compatibility review (IRI stability)
- [ ] CI + OOPS! passes
- [ ] Versioned IRIs finalised for v1.0.0
- [ ] PURLs registered via w3id.org
- [ ] Human-readable documentation generated (LODE / WIDOCO / ONTOSPY)
- [ ] Content negotiation configured (IRIs resolve to HTML docs or RDF serialisation)
- [ ] License (CC0-1.0 or CC-BY-4.0) + CITATION.cff
- [ ] Version bump + CHANGELOG

### 5.2 Publication & Dissemination (Meetings 39–41)

- Update TechnoPortal entry; submit to IndustryPortal.
- Configure Zenodo archive with DOI.
- Submit paper to a relevant venue (JOWO, ISWC, Wind Energy Science Conference, or *Wind Energy Science* journal).
- Present at IEA Task 43 meeting and OEO developer meeting.
- Host webinar or workshop.
- Announce via Task 43 newsletter, WeDoWind, LinkedIn, mailing lists.

### 5.3 Retrospective & Future Roadmap (Meeting 42)

Structured retrospective. Draft the future roadmap:
- Deeper axiomatisation / SWRL / SHACL as use cases demand higher expressiveness.
- Extending B1 to floating wind and hybrid systems.
- Multilingual labels.
- Formal matching with IOF-Core.
- LLM-assisted ontology population and bottom-up KG construction.
- Long-term governance model.

### Phase 5 Deliverables
- [ ] **WEO v1.0.0** — First official release
- [ ] PURL registered + content negotiation
- [ ] Human-readable docs published
- [ ] Zenodo DOI
- [ ] TechnoPortal / IndustryPortal updated
- [ ] Paper submitted
- [ ] Future roadmap document

---

## Meeting Cadence Overview

| Meeting | Month | Phase | Focus |
|---------|-------|-------|-------|
| 1–2 | Apr 2026 | 1 | Governance, tooling, working group |
| 3–4 | May 2026 | 1 | ORSD, CQs, use cases, scope |
| 5–6 | Jun 2026 | 1 | Alignment survey, BFO backbone |
| 7–8 | Jul 2026 | 2 | **B1** Material entities |
| 9–10 | Aug 2026 | 2 | **B2** Sites & spatial |
| 11–12 | Sep 2026 | 2 | **B3** Roles & agents |
| 13–14 | Oct 2026 | 2 | **B4** Lifecycle stages + WINDPROFILE alignment |
| 15–16 | Nov 2026 | 2 | **B5a** Planned processes |
| 17–18 | Dec 2026 | 2 | **B5b** Natural phenomena + **B5c** Agent activities |
| 19–20 | Jan 2027 | 3 | **B5** refinement in light of B6/B7 |
| 21–22 | Feb 2027 | 3 | **B6** Process attributes |
| 23–25 | Mar–mid Apr 2027 | 3 | **B7** Qualities |
| 26–27 | late Apr–May 2027 | 3 | **B8** ICE — top-level taxonomy |
| 28–29 | May–Jun 2027 | 3 | **B8** ICE — formalisation + extension points |
| 30 | late Jun 2027 | 3 | **B8** ICE review + inter-block relations + OEO bridge |
| 31–32 | Jul 2027 | 4 | CQ testing |
| 33–34 | Aug 2027 | 4 | Axiom enrichment + pilot use cases |
| 35–36 | Sep 2027 | 4 | Pilot use cases + expert review |
| 37–39 | Oct–mid Nov 2027 | 5 | Release prep (PURL, docs, QA) |
| 40–41 | late Nov–Dec 2027 | 5 | Publication, dissemination |
| 42 | late Dec 2027 | 5 | Retrospective + future roadmap |

---

## Risk Register

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| Low participation / contributor churn | Medium | High | ≤90 min meetings; clear block ownership; WeDoWind for async work |
| Scope creep — going too granular | High | High | **Enforce bridging-depth rule**: if a class only matters for one specific application, it belongs downstream, not in WEO. Review scope at every block milestone. |
| BFO classification disagreements (esp. B4 lifecycle, B5 phenomena) | High | Medium | Dedicated design discussions; ADRs; consult *Building Ontologies with BFO*; accept pragmatic decisions |
| B8 (ICE) scope explosion | High | Medium | "Stop rule": genus-level ICE classes + `isAbout` links only. Assign a scope guardian for B8. |
| Misalignment with OEO | Medium | High | Alignment liaison attends OEO dev meetings; update bridge module quarterly |
| Tooling barriers for non-ontologists | Medium | Medium | Spreadsheets + sketch tools for early prototyping; Protégé setup guide; WebProtégé fallback |
| WINDPROFILE integration difficulties | Medium | Medium | Resolve lifecycle-stage BFO question early (B4); keep WEO at genus level so WINDPROFILE can specialise freely |

---

## Appendix: Recommended Practice Cross-Reference

For traceability, the table below maps each RP from the IEA Wind Task 43 Recommended Practice to its primary location(s) in this plan.

| RP | Title (short) | Where addressed |
|----|---------------|-----------------|
| RP1 | Align with existing semantic artefacts | Phase 1 §1.3 |
| RP2 | Balance expressiveness with simplicity | Phase 1 §1.2, Phase 2–3 (per-block) |
| RP3 | Form working groups with domain + KE experts | Phase 1 §1.1 |
| RP4 | Use collaborative platforms | Phase 1 §1.1 |
| RP5 | Define clear application use cases | Phase 1 §1.2, Phase 4 §4.3 |
| RP6 | Follow established development methods | Micro-cycle (throughout) |
| RP7 | Carefully construct terms and definitions | Phase 2–3 (all blocks), Phase 4 §4.2 |
| RP8 | Aim for interoperability with adjacent domains | Phase 2–3 (alignment), cross-cutting |
| RP9 | Use tools appropriate for complexity & stage | Phase 1 §1.1, Phase 2 |
| RP10 | Manage artefacts with version control | Phase 1 §1.1 |
| RP11 | Automate publishing, deployment, validation | Phase 1 §1.1 (CI), Phase 5 §5.1 |
| RP12 | Assign versioned IRIs to stable releases | Phase 5 §5.1 |
| RP13 | Delay PURL registration until stabilisation | Phase 5 §5.1 |
| RP14 | Provide machine- and human-readable docs | Phase 5 §5.1 |
| RP15 | Make IRIs resolve to documentation pages | Phase 5 §5.1 |
| RP16 | Publish on TechnoPortal or IndustryPortal | Continuous + Phase 5 §5.2 |
| RP17 | Implement review & approval before releases | Cross-cutting + Phase 5 §5.1 |
| RP18 | Archive on open data portals (Zenodo/DOI) | Phase 5 §5.2 |
| RP19 | Test early prototypes in real-world applications | Phase 4 §4.3 |
| RP20 | Engage wider community with open calls | Phase 4 §4.4, Phase 5 §5.2, cross-cutting |

---

## References & Resources

- **IEA Wind Task 43 Recommended Practice:** *Ontology creation, publication, and maintenance for wind energy domain experts* (Feb 2026)
- **Knowledge engineering for wind energy:** Marykovskiy et al. (2024), *Wind Energ. Sci.*, 9, 883–917
- **BFO 2020:** ISO/IEC 21838-2:2021 — https://github.com/BFO-ontology/BFO-2020
- **Building Ontologies with BFO:** Arp, Smith & Spear (2015), MIT Press
- **OEO:** https://github.com/OpenEnergyPlatform/ontology — Booshehri et al. (2021), *Energy and AI*, 5, 100074
- **Ontology Development 101:** Noy & McGuinness (2001), Stanford
- **UPON:** De Nicola, Missikoff & Navigli (2005), DEXA
- **Methontology:** Fernández-López et al. (1997), AAAI Spring Symposium
- **NeOn Methodology:** Suárez-Figueroa et al. (2015), *Applied Ontology*, 10(2), 107–145
- **WINDPROFILE:** https://technoportal.hevs.ch/ontologies/WINDPROFILE
- **RDS for WPS:** https://apqp4wind.org/other-projects/tim-wind/workstream-2
- **WindIO (Task 37/55):** https://github.com/IEAWindTask37/windIO
- **IEA Task 43 Glossary:** https://iea-task-43.gitbook.io/iea-task-43-glossary/
- **TechnoPortal (WEO):** https://technoportal.hevs.ch/ontologies/WEO/
- **WeDoWind Community:** https://community.wedowind.ch
- **OOPS!:** https://oops.linkeddata.es/
- **ROBOT:** http://robot.obolibrary.org/
- **LODE:** https://github.com/essepuntato/LODE
- **WIDOCO:** https://github.com/dgarijo/Widoco
- **xls2rdf:** https://github.com/sparna-git/xls2rdf
- **SKOS Play!:** https://skos-play.sparna.fr/

---

*Document prepared: March 2026 · WEO Working Group*
