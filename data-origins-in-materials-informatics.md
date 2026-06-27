# From Evidence to Model: Where Materials-Informatics Data Actually Come From

**A public, source-based review of data origin, lineage, curation, and validation in materials informatics**  
**Last updated: June 2026**

## Scope and central argument

Materials informatics is often described as a sequence that begins with a dataset and ends with a machine-learning model. In practice, the dataset is already the result of many scientific and data-engineering decisions.

A model-ready row may have originated from a physical specimen, a density-functional-theory calculation, a supplier datasheet, a published graph, an instrument export, or a laboratory information system. Between that origin and the final training table, the record may have been parsed, digitized, normalized, merged, deduplicated, filtered, imputed, transformed, and assigned a target value. Each step can change what the record means.

The central argument of this report is therefore:

> The most important question is not simply **“Which database did the data come from?”** It is **“What chain of evidence produced each model input and target, and is that chain appropriate for the decision the model will support?”**

A professional assessment of materials-informatics data should trace the complete **data lineage**:

**physical or computational origin → generating process → acquisition → raw artifact → extraction → normalization → curation → model-ready target → validation**

This report synthesizes public literature, official repository documentation, and publicly described research infrastructures. It does not rely on proprietary industrial datasets.

---

## Executive summary

Materials-informatics data come from several primary scientific origins:

1. **Experiments**: synthesis, processing, characterization, performance testing, high-throughput experimentation, and manufacturing or quality-control measurements.
2. **Computations**: electronic-structure calculations, molecular dynamics, phase-field models, finite-element simulations, and other physics-based workflows.
3. **Published evidence**: text, tables, figures, supporting information, patents, reports, and datasheets that describe experimental or computational results.
4. **Reference collections**: curated crystallographic, chemical, engineering-property, or standards-oriented databases.
5. **Integrated or autonomous workflows**: systems that combine literature, calculations, experiments, robotics, and active learning.

These categories describe the **scientific origin** of the evidence. They should not be confused with the **distribution layer**. The Materials Project, OQMD, NOMAD, Materials Cloud, ICSD, CSD, HTEM, NIST repositories, and benchmark packages do not all represent the same type of source. Some generate standardized calculations, some curate experimental structures, some preserve raw files and provenance, and some repackage existing data into benchmark tasks.

Five conclusions are especially important.

### 1. A repository is not necessarily the original source

A record downloaded from a database may have originated from a journal article, an instrument, an external crystallographic collection, or a calculation performed by another workflow. Database name alone does not establish scientific provenance.

### 2. Row count is not independent evidence

Ten thousand rows may represent ten thousand independent specimens—or repeated measurements of a few batches, multiple properties derived from the same calculation, chemically near-duplicate structures, or records copied across databases. The **effective sample size** can be far smaller than the raw record count.

### 3. Aggregation can reduce quality as well as increase quantity

Combining datasets can help when they measure compatible quantities under compatible conditions. It can hurt when target definitions, methods, instruments, sample histories, or computational fidelities differ. More data are not automatically better data.

### 4. Benchmark accuracy is not the same as discovery or deployment performance

Random train–test splits often place closely related materials, batches, structures, or papers on both sides of the split. This can measure interpolation within a familiar dataset rather than performance on new chemistry, new laboratories, future data, or manufacturing conditions.

### 5. The best source depends on the real decision

For bulk phase stability, standardized computational repositories can be highly useful. For synthesis planning, processing data and failed experiments matter more. For formulations, coatings, batteries, and polymers, process history, composition basis, specimen identity, and test conditions often determine whether a data point is comparable at all.

The field is increasingly data-centric. FAIR principles, shared metadata, provenance systems, curation standards, realistic benchmarks, literature extraction, and closed-loop laboratories are becoming as important as model architecture.[1–5]

---

## 1. What “data origin” should mean

“Data source” is used loosely in materials-informatics papers. It can refer to at least three different things.

### 1.1 Primary scientific origin

This is the process that created the evidence:

- a physical experiment;
- a computational workflow;
- a literature statement or figure;
- an engineering or supplier measurement;
- a manufacturing or field observation.

This level determines what the target physically means.

### 1.2 Acquisition route

This is how the record entered the data system:

- direct instrument integration;
- ELN or LIMS export;
- API query;
- database dump;
- manual transcription;
- optical character recognition;
- natural-language processing;
- large-language-model extraction;
- digitization of a plot;
- parsing of simulation output.

This level determines what extraction errors and omissions are possible.

### 1.3 Distribution or packaging layer

This is where a user obtains the data:

- a public database;
- a repository;
- a benchmark;
- a journal supplement;
- a company data lake;
- a GitHub repository;
- a knowledge graph.

This level determines access, versioning, documentation, and legal reuse—but not necessarily the original scientific meaning.

A rigorous report should state all three. For example:

> “Formation energies were downloaded from Materials Project database version X. The values were derived from standardized DFT workflows, aggregated from task-level calculations, and filtered to a specified thermodynamic compatibility scheme.”

That statement is much more informative than “data came from the Materials Project.” Materials Project documentation explicitly notes that a material page combines information from many individual calculation tasks and recommends recording the database version used.[14]

---

## 2. The full lineage of a materials-informatics label

The natural unit of materials science is usually not a CSV row. It is a connected scientific history.

For experimental work, that history may be:

**precursor lot → formulation → process sequence → specimen → measurement event → analysis → reported property**

For computational work, it may be:

**seed structure → input-generation protocol → code and parameters → calculation tasks → parser → correction scheme → derived property**

For literature-derived data, it may be:

**paper → page/table/figure → text or image extraction → entity resolution → unit normalization → structured tuple**

A useful lineage model contains ten stages.

```mermaid
flowchart LR
    A[Scientific object] --> B[Generating process]
    B --> C[Acquisition]
    C --> D[Raw artifact]
    D --> E[Parsing or extraction]
    E --> F[Normalization]
    F --> G[Derived label]
    G --> H[Curation and dependency control]
    H --> I[Model representation]
    I --> J[Deployment-relevant validation]
```

### Stage 1: Scientific object

What is being described?

- material composition;
- molecular or crystal structure;
- formulation;
- specimen;
- film, coating, device, cell, or component;
- calculation;
- paper-level claim.

### Stage 2: Generating process

How was the object made or computed?

- synthesis route;
- mixing order;
- cure schedule;
- deposition history;
- annealing;
- sample conditioning;
- DFT functional and pseudopotential;
- molecular-dynamics force field;
- numerical solver and boundary conditions.

### Stage 3: Acquisition

How was the observation collected?

- instrument;
- test method;
- operator or automated platform;
- simulation workflow;
- literature search;
- database query.

### Stage 4: Raw artifact

What was originally produced?

- detector counts;
- image stack;
- spectrum;
- force–displacement curve;
- diffraction pattern;
- simulation input and output files;
- table;
- paragraph;
- chart image.

### Stage 5: Parsing or extraction

What converted the raw artifact into structured data?

- vendor software;
- scientific parser;
- fitting routine;
- segmentation model;
- manual entry;
- rule-based NLP;
- language model;
- graph digitizer.

### Stage 6: Normalization

How were identities, units, and conditions harmonized?

- chemical-name resolution;
- formula canonicalization;
- unit conversion;
- structure standardization;
- composition-basis conversion;
- test-method mapping;
- condition alignment.

### Stage 7: Derived label

Was the target directly measured, or calculated from another result?

Examples include:

- peak position fitted from a spectrum;
- glass-transition temperature selected from a DSC trace;
- energy above hull computed from a phase diagram;
- “synthesis success” assigned from diffraction analysis;
- modulus calculated from a stress–strain region;
- property value estimated from a plotted curve.

### Stage 8: Curation and dependency control

Which records were:

- filtered;
- excluded;
- merged;
- deduplicated;
- averaged;
- imputed;
- marked as uncertain;
- grouped by shared sample, paper, batch, or workflow?

### Stage 9: Model representation

How was scientific identity translated into features?

- composition statistics;
- molecular descriptors;
- fingerprints;
- crystal graphs;
- spectra or images;
- process variables;
- physics-aware interactions;
- learned embeddings.

### Stage 10: Validation

Does evaluation mimic the intended use?

- new records from the same distribution;
- new chemistry;
- new structure families;
- a later time period;
- a different laboratory;
- a new manufacturing line;
- prospective experiments.

A dataset is trustworthy only when these stages are documented well enough to determine what the model is actually learning.

---

## 3. Primary origins of materials-informatics data

### 3.1 Experimental data

Experimental data begin with physical specimens and measurement events. Important subtypes include:

- manually designed laboratory experiments;
- design-of-experiments campaigns;
- combinatorial and high-throughput experiments;
- autonomous or self-driving-laboratory campaigns;
- routine characterization;
- manufacturing and quality-control data;
- field-performance and degradation data;
- failed or negative experiments.

The critical object is usually **sample lineage**. Two specimens with the same nominal composition may not be equivalent if they differ in precursor lot, mixing history, moisture exposure, substrate, thickness, cure, thermal history, or conditioning.

Experimental databases such as NREL’s HTEM infrastructure and the Materials Provenance Store were designed around this issue. They link samples, processes, measurements, metadata, and analysis instead of treating each result as an isolated row.[18–20]

#### Main strengths

- direct connection to physical behavior;
- relevance to synthesis and performance;
- access to process–structure–property relations;
- possibility of learning failure regions and operating windows.

#### Main risks

- incomplete process metadata;
- instrument drift and calibration differences;
- sample-to-file linkage errors;
- analyst-dependent preprocessing;
- batch and laboratory effects;
- selective reporting of successful experiments;
- missing uncertainty and replicate information.

A model trained on experimental properties without process and measurement context may learn laboratory conventions rather than general materials behavior.

---

### 3.2 Computational data

Computational datasets are generated by physics-based models and standardized workflows. Common sources include:

- density functional theory;
- molecular dynamics;
- quantum chemistry;
- phase-field simulations;
- finite-element analysis;
- thermodynamic or kinetic models;
- computational screening campaigns.

The natural lineage is:

**input structure and assumptions → workflow → code and version → numerical parameters → raw outputs → parsed results → corrections and derived properties**

Public ecosystems such as the Materials Project, OQMD, AFLOW, JARVIS, NOMAD, and Materials Cloud have made computational materials data unusually accessible. They differ, however, in whether they primarily generate standardized calculations, aggregate community uploads, preserve raw files, or publish complete workflow provenance.[14–17]

#### Main strengths

- large scale;
- consistent automation within a workflow family;
- access to hypothetical materials;
- reproducible inputs and algorithms when versions are preserved;
- labels that may be expensive or impossible to measure directly.

#### Main risks

- mismatch between simulated and real materials;
- mixed functionals, pseudopotentials, force fields, or convergence settings;
- duplicated or closely related structures;
- omission of failed calculations;
- database updates that change derived values;
- confusing computational fidelity with experimental truth;
- modeling idealized equilibrium structures while deployment depends on defects, kinetics, interfaces, or processing.

Computational and experimental values should generally be treated as different **fidelity levels**, not merged as interchangeable labels without an explicit calibration or multi-fidelity strategy.

---

### 3.3 Literature-derived data

Published literature contains information that is absent from formal databases, especially:

- synthesis procedures;
- processing conditions;
- compositions and formulations;
- measured properties;
- failure descriptions;
- morphology and mechanism interpretations;
- application-specific tests.

Literature extraction can be manual or automated through OCR, NLP, table extraction, image analysis, and language models. Polymer-focused resources such as PolyIE and large-corpus property-extraction pipelines demonstrate both the value and the difficulty of this route.[11–13]

A literature-derived number is not automatically ground truth. It inherits multiple uncertainty layers:

1. **original experimental or computational uncertainty**;
2. **reporting uncertainty**, including omitted conditions or rounded values;
3. **document-conversion uncertainty**, such as PDF parsing or OCR errors;
4. **information-extraction uncertainty**, including entity and relation errors;
5. **normalization uncertainty**, such as chemical-name or unit mapping;
6. **selection bias**, because published work is not a representative record of all attempts.

Automated extraction should therefore preserve source evidence:

- DOI or document identifier;
- page, table, figure, paragraph, or caption;
- original text;
- original value and unit;
- normalized value and unit;
- extraction method;
- model or parser version;
- confidence;
- human-review status.

The absence of a material or processing step in a paper must not automatically be interpreted as zero. It may be unreported, implicit, or described elsewhere.

---

### 3.4 Reference databases and engineering datasheets

Reference collections include crystallographic databases, chemical databases, standards-oriented resources, and engineering-property datasheets.

Examples include:

- ICSD and CSD for curated crystal structures;
- PubChem for chemical identities and annotations;
- NIST data resources;
- MatWeb and supplier datasheets for engineering properties.

These sources are useful but highly heterogeneous. A reported property may represent:

- a specific grade;
- a broad material family;
- a single supplier;
- an unspecified test condition;
- a literature compilation;
- an average or typical value rather than a controlled measurement.

Datasheet values are often appropriate for preliminary screening but may be unsuitable as precise ML targets unless grade, conditioning, method, geometry, and temperature are known.

Licensing and redistribution rights must be checked for each resource. Public accessibility does not imply unrestricted reuse.

---

### 3.5 Manufacturing, quality-control, and field data

Industrial materials development produces data that are rarely visible in the public literature:

- batch records;
- incoming-material certificates;
- process-control signals;
- line conditions;
- in-process inspections;
- quality-control tests;
- customer returns;
- reliability and aging data;
- supplier changes;
- scale-up trials.

These records may be the most relevant source for product and manufacturing decisions, but they have characteristic complications:

- process changes over time;
- censored failures;
- non-random operating policies;
- multiple instruments or sites;
- changing specifications;
- confidential formulations;
- missing scientific metadata;
- labels influenced by business decisions.

Because public papers reveal little about proprietary enterprise practice, claims about “how industry does materials informatics” should be presented cautiously. Published vendor descriptions and case studies show possible architectures, not a representative census of industrial workflows.

---

### 3.6 Autonomous laboratories are integrated acquisition systems

An autonomous laboratory is not a separate physical data origin. It is an integrated system that may combine:

- historical literature;
- computational predictions;
- robotic synthesis;
- automated characterization;
- active learning;
- experiment planning;
- human review.

Its value is not merely automation. A mature autonomous system preserves the connection between proposed experiment, executed protocol, raw measurement, analysis, uncertainty, and next decision.

The A-Lab study is an important example of this integrated model.[22] It is also a useful reminder that automation does not eliminate the need for scientific validation. Nature published an author correction to the paper on January 19, 2026.[23] Claims of phase formation, novelty, synthesis success, or autonomous discovery require explicit criteria, independent checks, and replicable evidence.

---

## 4. Repository type is not data type

The following table separates representative distribution systems by their main role.

| Resource class | Representative examples | What is primarily distributed | What users must still verify |
|---|---|---|---|
| Standardized computational production systems | Materials Project, OQMD, AFLOW, JARVIS | Calculated structures and properties produced through defined workflows | workflow version, fidelity, corrections, deprecations, duplicates, applicability to real materials |
| Community computational archives | NOMAD | Raw and normalized outputs from many codes and contributors | source workflow consistency, parser coverage, metadata completeness, contributor-specific quality |
| Workflow-provenance publication systems | Materials Cloud/AiiDA | Data, calculations, and provenance graphs | whether the published workflow matches the intended target and fidelity |
| Experimental structure references | ICSD, CSD | Curated experimentally determined structures and bibliographic metadata | disorder, quality filters, identity resolution, licensing, relevance to target conditions |
| Experimental lineage infrastructures | HTEM, Materials Provenance Store, Materials Experiment Knowledge Graph | Samples, processes, measurements, analysis, and provenance | institution- or modality-specific biases, measurement comparability, incomplete fields |
| General research-data repositories | NIST MDR and similar repositories | Heterogeneous deposited datasets with metadata | dataset-specific schema, quality, licensing, and completeness |
| Literature-derived datasets | synthesis corpora, polymer property datasets, knowledge graphs | Structured records extracted from papers | extraction accuracy, source evidence, publication bias, target comparability |
| Benchmark packages | Matbench, Matbench Discovery, leaderboards | Cleaned tasks and predefined evaluation procedures | whether source curation, split, metric, and target match the real deployment problem |

The most common analytical mistake is to compare these resources as though they were interchangeable databases. They are different parts of the scientific information chain.

---

## 5. How representative groups build data systems

### 5.1 Computational materials-informatics groups

Typical workflow:

1. select seed structures or compositions;
2. generate standardized simulation inputs;
3. execute high-throughput calculations;
4. parse and normalize results;
5. apply correction and compatibility schemes;
6. build derived property collections;
7. expose data through an API or repository;
8. construct benchmarks or screening models.

Their major advantage is controlled automation at scale. Their major limitation is that the model may be highly accurate for the computational label while remaining weakly connected to synthesis, defects, processing, or device performance.

### 5.2 Experimental and high-throughput groups

Typical workflow:

1. register precursors, samples, and intended compositions;
2. execute synthesis or processing;
3. acquire raw instrument data;
4. connect files to specimen and process history;
5. perform quality control and analysis;
6. store derived labels with uncertainty;
7. train models or active-learning policies;
8. validate through new experiments.

Their major advantage is physical relevance. Their major limitation is the cost of metadata capture and the difficulty of maintaining consistent sample identity across instruments and operators.

### 5.3 Literature-mining groups

Typical workflow:

1. define a scientific ontology and target schema;
2. retrieve a document corpus;
3. parse text, tables, and figures;
4. identify materials, properties, values, and conditions;
5. resolve relations and coreferences;
6. normalize identities and units;
7. attach source evidence and confidence;
8. validate against expert annotation;
9. create a searchable database or training dataset.

Their major advantage is access to distributed historical knowledge. Their major limitation is that the extracted record may omit exactly the context required for scientific comparability.

### 5.4 Product-development and enterprise groups

A defensible conceptual model is:

1. integrate internal ELN, LIMS, instrument, simulation, supplier, and manufacturing records;
2. map them to shared sample, batch, formulation, and process identities;
3. combine external public data only where definitions are compatible;
4. build decision-specific models;
5. validate under scale-up or production conditions;
6. monitor performance as materials, suppliers, equipment, and specifications change.

This model is logically consistent with product development, but public evidence is incomplete because the most valuable industrial datasets and workflows are proprietary. Public reports should distinguish observed published practice from reasonable inference.

---

## 6. What must happen before machine learning

### 6.1 Identity resolution

A model cannot learn a stable relationship if the same material is represented inconsistently.

Identity resolution may involve:

- canonical chemical names;
- persistent identifiers;
- formula normalization;
- structure matching;
- salt, solvate, and protonation handling;
- polymer repeat-unit and architecture representation;
- supplier and grade mapping;
- specimen, batch, and lot identifiers.

A single “material name” is often insufficient.

### 6.2 Condition and method harmonization

The same property can change with:

- temperature;
- humidity;
- frequency;
- strain rate;
- specimen geometry;
- film thickness;
- substrate;
- aging;
- sample preparation;
- instrument;
- analysis method.

Records should not be merged merely because their property column has the same name.

### 6.3 Unit and basis normalization

Unit conversion is only valid when the quantity basis is known.

For formulations, concentration may be reported as:

- weight percent of total formulation;
- weight percent solids;
- parts per hundred resin;
- volume fraction;
- molar fraction;
- functional-group equivalents;
- dry-film fraction.

A value of “10%” is not interpretable without this basis.

### 6.4 Deduplication and dependency control

Duplicates include more than exact repeated rows. Materials datasets often contain:

- symmetry-equivalent or nearly identical structures;
- multiple calculations of the same composition;
- repeated measurements from one specimen;
- multiple specimens from one batch;
- several properties from the same paper and formulation;
- train and test entries with shared chemical ancestry.

Studies of redundancy and out-of-distribution prediction show that conventional random splits can substantially overstate model performance.[7–10]

The correct question is not “Are the rows identical?” It is:

> “Could information from a shared scientific ancestor make the test record easier to predict?”

### 6.5 Missingness

Missing data are rarely random in materials research.

A value may be missing because:

- the test was not relevant;
- the material failed before testing;
- the result was unfavorable;
- the instrument was unavailable;
- the paper omitted the value;
- the value was below a detection limit;
- the calculation did not converge.

These mechanisms have different meanings. Converting all missing values to zero or imputing them without flags can create false scientific patterns.

### 6.6 Label-generation transparency

Many targets are outputs of another model or fitting process.

Examples:

- phase fraction from Rietveld refinement;
- modulus from curve fitting;
- particle size from image segmentation;
- glass transition from a selected DSC criterion;
- stability from a convex-hull construction;
- “success” from a classification rule.

The label-generation software, parameters, and decision criteria should be versioned as part of the dataset.

### 6.7 Versioning

Materials databases evolve. Records are added, deprecated, corrected, or rebuilt. Materials Project, for example, publishes database-version changes and advises researchers to record the version used.[14]

A reproducible dataset should capture:

- source version or retrieval date;
- exact query;
- raw response or snapshot;
- parser version;
- curation code version;
- descriptor version;
- final dataset hash.

---

## 7. Effective sample size: why 10,000 rows may not mean 10,000 examples

Machine learning assumes that observations provide enough independent information to learn and evaluate a pattern. Materials data often violate this assumption.

Consider three datasets, each with 1,000 rows:

- **Dataset A:** 1,000 independently prepared specimens.
- **Dataset B:** 100 formulations, each measured ten times.
- **Dataset C:** 20 base chemistries with small compositional variations and multiple properties.

All have the same row count, but not the same scientific diversity or independent evidence.

The term **effective sample size** is useful here. It is not necessarily a single universal number. It expresses the amount of independent information after accounting for:

- repeated measurements;
- shared batches;
- shared papers;
- related crystal prototypes;
- near-identical compositions;
- common parent formulations;
- common calculation workflows;
- temporal and laboratory clusters.

A responsible report should provide both:

- **raw record count**, and
- **grouped counts**, such as unique materials, unique formulations, unique batches, unique papers, unique chemical families, and unique laboratories.

For small formulation datasets, the number of distinct formulation families or experimental campaigns may be more informative than the number of rows.

---

## 8. Aggregation: when more data help and when they hurt

Data aggregation is attractive because materials datasets are often small. However, recent work has shown that combining datasets does not guarantee better downstream performance.[5]

Aggregation is most defensible when:

- target definitions match;
- measurement or computational methods are compatible;
- units and conditions are recoverable;
- material identities are resolvable;
- fidelity differences are modeled explicitly;
- dataset origin is retained as a variable;
- external validation confirms transfer.

Aggregation is risky when:

- one source reports intrinsic properties and another reports device performance;
- temperatures or frequencies differ;
- simulated and experimental targets are mixed without calibration;
- literature values omit process history;
- supplier datasheets use unspecified methods;
- duplicates are present across repositories;
- one large source overwhelms a smaller but more relevant source.

Recommended strategies include:

- source-specific models;
- hierarchical models;
- multi-task learning;
- multi-fidelity learning;
- domain adaptation;
- dataset-origin features;
- uncertainty-weighted training;
- retaining incompatible data as separate evidence layers.

The decision to merge should be treated as a scientific hypothesis, not a default preprocessing step.

---

## 9. Uncertainty must be separated by origin

A single error bar cannot describe all uncertainty in a materials-informatics pipeline.

### 9.1 Aleatoric uncertainty

Irreducible or repeatability-related variation in the observed system:

- specimen heterogeneity;
- measurement noise;
- batch variation;
- stochastic process outcomes.

### 9.2 Epistemic uncertainty

Uncertainty caused by limited knowledge or data:

- sparse chemical regions;
- model extrapolation;
- unknown interactions;
- insufficient training diversity.

### 9.3 Extraction uncertainty

Uncertainty introduced while converting evidence into structured records:

- OCR errors;
- table parsing;
- entity recognition;
- incorrect material–property relations;
- figure digitization;
- unit normalization.

### 9.4 Domain uncertainty

Uncertainty caused by a change in context:

- new laboratory;
- new instrument;
- new supplier;
- new process line;
- new chemical family;
- simulation-to-experiment transfer.

### 9.5 Curation uncertainty

Uncertainty caused by human or algorithmic data decisions:

- duplicate resolution;
- representative-value selection;
- outlier removal;
- disorder treatment;
- missing-data imputation;
- exclusion rules.

These uncertainties should be recorded separately where possible. Model uncertainty alone does not cover uncertainty in extraction, curation, or scientific target definition.

---

## 10. Validation should follow the intended decision

A model can be statistically impressive and scientifically unhelpful if evaluated on the wrong split or metric.

### 10.1 Random splits

Random splits are useful for estimating interpolation within a well-mixed dataset. They are often too easy for materials discovery because similar records appear in training and test sets.

### 10.2 Grouped splits

Group by the unit that must remain unseen:

- paper;
- formulation family;
- polymer backbone;
- scaffold;
- chemical system;
- crystal prototype;
- sample batch;
- laboratory;
- instrument;
- supplier;
- processing campaign.

### 10.3 Temporal splits

Train on earlier data and test on later data. This better represents prospective use and captures changes in explored chemistry and experimental practice.

### 10.4 Out-of-distribution splits

Hold out sparse or chemically distinct regions. Research on robustness, domain adaptation, and structure-based out-of-distribution benchmarks shows that model performance often degrades sharply outside the training distribution.[7–10]

### 10.5 External validation

Test on a genuinely separate source, laboratory, database release, or institution.

### 10.6 Prospective validation

Use the model to select new experiments or calculations, then evaluate the resulting outcomes. This is the strongest test of decision utility.

### 10.7 Decision-aligned metrics

A low mean absolute error may not translate into good candidate selection. For discovery, useful metrics may include:

- precision among top-ranked candidates;
- recall of viable candidates;
- enrichment over random selection;
- calibration of uncertainty;
- false-positive cost;
- discovery acceleration;
- experimental success rate.

Matbench standardized property-prediction tasks, while Matbench Discovery emphasized that formation-energy regression metrics can be misaligned with stability-screening decisions.[6]

---

## 11. Special case: polymers, formulations, coatings, and soft materials

Polymers and formulations expose the limitations of structure-only materials informatics.

A polymer or coating is not fully defined by a single repeat-unit string. Relevant identity may include:

- monomer composition;
- sequence or composition distribution;
- molecular weight and molecular-weight distribution;
- architecture and branching;
- tacticity;
- end groups;
- residual monomer;
- additives;
- crosslink density;
- phase morphology.

A formulation additionally requires:

- ingredient role;
- exact grade or supplier where relevant;
- amount;
- amount basis;
- total-solids basis;
- mixing order;
- shear and mixing time;
- storage history;
- application method;
- wet and dry thickness;
- cure dose, temperature, humidity, and time;
- substrate and surface treatment;
- conditioning;
- test method and environment.

### A three-layer public data architecture

#### Layer 1: Evidence layer

Preserve what the source actually says:

- document ID and DOI;
- page, paragraph, table, figure, or caption;
- original chemical name;
- original value and unit;
- original text or cropped evidence;
- extraction method;
- extraction confidence;
- reviewer status.

#### Layer 2: Canonical scientific layer

Normalize without discarding source meaning:

- canonical ingredient;
- material role;
- amount and basis;
- formulation total and closure status;
- polymer architecture;
- molecular-weight information;
- process and cure conditions;
- substrate;
- thickness;
- test method;
- environment;
- replicate and uncertainty information;
- links to shared batches or specimens.

#### Layer 3: Model-ready layer

Record every modeling decision:

- descriptor or representation version;
- feature-engineering rules;
- exclusion criteria;
- missing-data strategy;
- target transformation;
- group split ID;
- dataset version;
- provenance links back to Layer 1.

### Rules that prevent common errors

- Do not interpret an unreported ingredient as zero.
- Do not mix wet-basis, solids-basis, phr, and molar quantities without explicit conversion.
- Preserve formulation closure and flag totals that do not reconcile.
- Keep multiple values when sources conflict; do not silently average them.
- Distinguish direct measurements from values digitized from figures.
- Group train/test splits by paper, chemistry family, and experimental campaign.
- Human-review high-impact records and unusual model-driving values.
- Treat process history as part of material identity, not optional metadata.

For polymers and formulations, the strongest model is often not the model with the most molecular descriptors. It is the model built on the most faithful representation of **composition + process + specimen + test context**.

---

## 12. A 20-point data-maturity score

A public report can compare datasets or projects using the following ten dimensions. Score each from 0 to 2.

| Dimension | 0 | 1 | 2 |
|---|---|---|---|
| Provenance | source unclear | source named | record-level lineage to raw evidence |
| Material identity | ambiguous names | partly normalized | persistent, scientifically sufficient identity |
| Process metadata | absent | partial | complete relevant process history |
| Measurement or calculation metadata | absent | method named | method, settings, versions, and conditions preserved |
| Dependency and duplicate control | ignored | exact duplicates handled | scientific ancestry and near-duplicates controlled |
| Uncertainty | absent | single error/confidence value | multiple uncertainty sources represented |
| Missingness and failures | silently imputed or omitted | missing values flagged | mechanisms, failures, and censoring represented |
| Versioning and reproducibility | no snapshot | retrieval date or code available | source, query, code, environment, and dataset hash versioned |
| Evaluation realism | random split only | grouped or OOD test | deployment-matched external or temporal validation |
| Prospective validation | none | retrospective case study | new experiments, calculations, or production outcomes |

**Maximum score: 20**

The score is not a universal standard. It is a transparent way to prevent a large, clean-looking table from being mistaken for a mature scientific dataset.

---

## 13. Recommended paper-level data-lineage table

A literature review of materials-informatics studies should extract the following fields.

| Field | Question |
|---|---|
| Study and year | What work is being assessed? |
| Scientific objective | What decision or prediction is the model intended to support? |
| Unit of observation | Material, specimen, batch, formulation, structure, calculation, paper, image, spectrum? |
| Primary origin | Experiment, computation, literature, datasheet, manufacturing, mixed? |
| Acquisition route | Instrument integration, API, manual extraction, NLP, OCR, database dump? |
| Data generator | Which laboratory, workflow, code, instrument, or organization created the evidence? |
| Raw record count | How many records are present before grouping? |
| Independent sample indicators | Unique materials, batches, papers, chemical families, laboratories? |
| Target definition | Exactly what quantity or class is predicted? |
| Conditions and method | Under what measurement, processing, or computational conditions? |
| Curation | What was filtered, merged, averaged, or imputed? |
| Missingness | How were unreported, failed, or censored values handled? |
| Representation | How was the material encoded? |
| Split strategy | Random, grouped, temporal, OOD, external, prospective? |
| Uncertainty | Which uncertainty sources were recorded or modeled? |
| Validation | Was performance tested on a deployment-relevant decision? |
| Availability | Are raw, parsed, and model-ready data available? |
| Version and license | Can the exact data state be recovered and legally reused? |

This table reveals differences between groups more clearly than a simple “database used” column.

---

## 14. What is well established, and what remains hard to know

### Well supported by public evidence

- Materials data require domain-specific metadata and provenance.[2–4]
- Computational repositories generally offer mature programmatic access and standardized workflows.[14–17]
- Experimental data are more sensitive to sample lineage and process context.[18–20]
- Dataset redundancy and random splits can overstate generalization.[7–10]
- Literature extraction is valuable but introduces entity, relation, and context errors.[11–13]
- Benchmark metrics should be aligned with the actual scientific decision.[6]
- Databases and derived labels change over time and require versioning.[14]
- Autonomous systems still require strict scientific validation.[22–23]

### Difficult to infer from public evidence

- the prevalence and architecture of industrial materials data platforms;
- how much failed experimental data companies retain;
- the real comparative performance of proprietary materials-informatics systems;
- the degree of human review in private automated-extraction pipelines;
- the economic value of models after scale-up and manufacturing constraints;
- how often published benchmark gains survive prospective deployment.

A public report should not fill these gaps with confident generalizations.

---

## 15. Practical recommendations

### For researchers building a dataset

1. Define the downstream decision before collecting data.
2. Preserve raw, parsed, canonical, and model-ready layers.
3. Assign stable identities to materials, samples, batches, calculations, and sources.
4. Record conditions, methods, and process history.
5. Separate measured, calculated, extracted, and inferred values.
6. Retain source evidence for literature-derived records.
7. Track shared ancestry and effective sample size.
8. Treat aggregation as a hypothesis that requires validation.
9. Use grouped, temporal, OOD, external, or prospective evaluation.
10. Publish the source version, query, curation code, schema, and dataset hash.

### For readers evaluating a paper

Ask:

- What physically generated the target?
- Is the database the origin, or only the distributor?
- How many independent materials or experiments are present?
- Could related records appear in both training and test data?
- Are processing and measurement conditions comparable?
- Is the model interpolating, extrapolating, ranking, or making a discovery claim?
- Does the validation reproduce the intended real-world decision?
- Can every important label be traced back to evidence?

### For organizations choosing infrastructure

Favor systems that support:

- persistent sample and calculation identities;
- raw-file retention;
- machine-readable schemas;
- provenance graphs;
- dataset versioning;
- access control;
- record-level uncertainty;
- human review;
- reproducible exports;
- clear licensing.

A sophisticated model cannot compensate for a missing chain of evidence.

---

## Conclusion

Materials informatics does not begin with machine learning. It begins with the production and interpretation of evidence.

The field’s public data ecosystem now includes highly developed computational databases, experimental provenance systems, research-data repositories, literature-mining pipelines, benchmarks, and autonomous laboratories. Their value is substantial, but they solve different problems. A database of equilibrium DFT calculations, a thin-film high-throughput experiment archive, a polymer literature corpus, and a manufacturing quality database should not be treated as interchangeable sources.

The most useful distinction is not simply experimental versus computational, or public versus private. It is the completeness of the path from the **data-generating process** to a **trustworthy, deployment-relevant label**.

A mature materials-informatics workflow can answer five questions for every important record:

1. What scientific object does this record represent?
2. How was the evidence generated and acquired?
3. What transformations created the model input and target?
4. Which records share scientific ancestry?
5. Does validation match the decision the model will actually support?

When those questions are answered, machine learning becomes a scientific tool. When they are not, even a high-performing model may be learning the structure of the dataset rather than the behavior of materials.

---

## Selected references and official resources

1. Himanen, L. et al. “Data-Driven Materials Science: Status, Challenges, and Perspectives.” *Advanced Science* 6, 1900808 (2019). https://doi.org/10.1002/advs.201900808  
2. Scheffler, M. et al. “FAIR data enabling new horizons for materials research.” *Nature* 604, 635–642 (2022). https://doi.org/10.1038/s41586-022-04501-x  
3. Ghiringhelli, L. M. et al. “Shared metadata for data-centric materials science.” *Scientific Data* 10, 626 (2023). https://doi.org/10.1038/s41597-023-02501-8  
4. Hart, M. et al. “Trust Not Verify? The Critical Need for Data Curation Standards in Materials Informatics.” *Chemistry of Materials* 36, 9046–9055 (2024). https://doi.org/10.1021/acs.chemmater.4c00981  
5. Ottomano, F. et al. “Not as simple as we thought: a rigorous examination of data aggregation in materials informatics.” *Digital Discovery* 3 (2024). https://doi.org/10.1039/D3DD00207A  
6. Dunn, A. et al. “Benchmarking materials property prediction methods: the Matbench test set and Automatminer reference algorithm.” *npj Computational Materials* 6, 138 (2020). https://doi.org/10.1038/s41524-020-00406-3; Riebesell, J. et al. “Matbench Discovery.” https://arxiv.org/abs/2308.14920  
7. Li, K. et al. “A critical examination of robustness and generalizability of machine learning prediction of materials properties.” *npj Computational Materials* 9, 55 (2023). https://doi.org/10.1038/s41524-023-01012-9  
8. Xiong, Z. et al. “Realistic material property prediction using domain adaptation based machine learning.” *Digital Discovery* 3, 300–312 (2024). https://doi.org/10.1039/D3DD00162H  
9. Xiong, Z. et al. “Structure-based out-of-distribution materials property prediction: a benchmark study.” *npj Computational Materials* (2024). https://doi.org/10.1038/s41524-024-01316-4  
10. Li, Q. et al. “MD-HIT: Machine learning for material property prediction with dataset redundancy control.” *npj Computational Materials* 10, 245 (2024). https://doi.org/10.1038/s41524-024-01426-z  
11. Cheung, J. J. et al. “PolyIE: A Dataset of Information Extraction from Polymer Material Scientific Literature.” (2023). https://arxiv.org/abs/2311.07715  
12. Shetty, P. et al. “A general-purpose material property data extraction pipeline from large polymer corpora using natural language processing.” *npj Computational Materials* 9, 52 (2023). https://doi.org/10.1038/s41524-023-01003-w  
13. Foppiano, L. et al. “Mining experimental data from Materials Science literature with Large Language Models: an evaluation study.” (2024). https://arxiv.org/abs/2401.11052  
14. Materials Project documentation: database versions, API, and data access. https://docs.materialsproject.org/changes/database-versions ; https://docs.materialsproject.org/downloading-data/using-the-api ; Horton, M. K. et al. “Accelerated data-driven materials science with the Materials Project.” *Nature Materials* 24, 1522–1532 (2025). https://doi.org/10.1038/s41563-025-02272-0  
15. NOMAD documentation: parsers, archives, and data processing. https://nomad-lab.eu/docs ; https://nomad-lab.eu/prod/v1/docs/explanation/basics.html  
16. Talirz, L. et al. “Materials Cloud, a platform for open computational science.” *Scientific Data* 7, 299 (2020). https://doi.org/10.1038/s41597-020-00637-5  
17. OQMD documentation and API. https://oqmd.org/documentation/overview ; https://oqmd.org/api/  
18. Talley, K. R. et al. “Research data infrastructure for high-throughput experimental materials science.” *Patterns* 2, 100373 (2021). https://doi.org/10.1016/j.patter.2021.100373  
19. Statt, M. J. et al. “The Materials Provenance Store.” *Scientific Data* 10, 184 (2023). https://doi.org/10.1038/s41597-023-02107-0  
20. Statt, M. J. et al. “The materials experiment knowledge graph.” *Digital Discovery* 2, 909–914 (2023). https://doi.org/10.1039/D3DD00067B  
21. Ward, L. et al. “Principles of the Battery Data Genome.” (2021). https://arxiv.org/abs/2109.07278  
22. Szymanski, N. J. et al. “An autonomous laboratory for the accelerated synthesis of inorganic materials.” *Nature* 624, 86–91 (2023). https://doi.org/10.1038/s41586-023-06734-w  
23. Szymanski, N. J. et al. “Author Correction: An autonomous laboratory for the accelerated synthesis of inorganic materials.” *Nature* 650, E1 (2026). https://doi.org/10.1038/s41586-025-09992-y  

