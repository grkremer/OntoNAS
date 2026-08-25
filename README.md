# OntoNAS

**A UFO-Based Ontology for Neural Architecture Search**

This repository is the supplementary artifact for the paper *OntoNAS: A UFO-Based Ontology for
Neural Architecture Search*, accepted at the **19th Seminar on Ontology Research in Brazil
(ONTOBRAS 2026)**, Vitória (ES), Brazil, September 23, 2026.

## About

OntoNAS is a domain ontology for Neural Architecture Search (NAS), grounded in the **Unified
Foundational Ontology (UFO)** and developed following the **NeOn Methodology**. The conceptual
model was produced in **OntoUML** and implemented in **OWL 2** using
[gUFO](http://purl.org/nemo/doc/gufo), a lightweight implementation of UFO suited to OWL-based
reasoning.

The ontology is in [OntoNAS.ttl](OntoNAS.ttl) and can be opened in Protégé; the OWL 2
implementation was checked with the HermiT reasoner.

## Entity catalogue

The entities of the NAS domain modeled in this work, classified according to UFO categories.
Textual definitions follow the **genus–differentia** pattern: each term is anchored in a genus and
distinguished by its own characteristics.

| Entity | UFO Category | Definition |
| --- | --- | --- |
| **Algorithm** | Object Kind | A finite, well-defined specification of operations executable on a computational substrate. |
| **Search Strategy** | Object SubKind | An *Algorithm* that governs the exploration of a search space, proposing and traversing candidate configurations. |
| **Random Search** | Object SubKind | A *Search Strategy* that samples architectures uniformly at random from the search space. |
| **Reinforcement Learning** | Object SubKind | A *Search Strategy* that frames architectural decisions as sequential agent actions optimizing a reward signal derived from candidate performance. |
| **Evolutionary Algorithms** | Object SubKind | A *Search Strategy* that explores the search space through population-based selection-and-mutation mechanisms. |
| **Bayesian Optimization** | Object SubKind | A *Search Strategy* that selects configurations by means of a probabilistic surrogate balancing exploration and exploitation. |
| **Gradient Based** | Object SubKind | A *Search Strategy* that relaxes the discrete search space into a differentiable one explored via gradient-driven optimization. |
| **Performance Estimation Strategy** | Object SubKind | An *Algorithm* that estimates the quality of candidate architectures, monitors search progress, and *produces* the *Neural Network Metrics*. |
| **Data** | Collection Kind | An informational resource consumable by computational procedures during a search run. |
| **Dataset** | Object Kind | An informational resource that figures as a member of *Data* (`is_collection_member_of`), participates in the *NAS Experiment*, and bears the *Structured* quality. |
| **Structured** | Quality Kind | An intrinsic quality inhering in a *Dataset* that characterizes the organization of its samples, whose possible values (structured, semi-structured, unstructured) are qualia in its quality structure. |
| **Hardware Device** | Object Kind | A physical device that provides the material substrate supporting execution, participates in the *NAS Experiment*, has *Core* and *Memory* components, and bears its operational qualities. |
| **CPU** | Object SubKind | A *Hardware Device* specialization denoting general-purpose processing units employed as the substrate of an experiment. |
| **GPU** | Object SubKind | A *Hardware Device* specialization denoting massively parallel processing units used to accelerate training and evaluation. |
| **Core** | Object Kind | A processing unit that is a component of a *Hardware Device* (`is_component_of`). |
| **Memory** | Object Kind | A storage component of a *Hardware Device* (`is_component_of`) that bears the *Memory Capacity* quality. |
| **Operating Frequency** | Quality Kind | An intrinsic quality inhering in a *Hardware Device* that measures the clock rate at which it operates. |
| **Memory Capacity** | Quality Kind | An intrinsic quality inhering in a *Memory* component that quantifies its storage capacity. |
| **Neural Network** | Object SubKind | An artificial neural architecture composed of *Layer* components that bears the *Neural Network Hyperparameters* quality, produced in the course of a *NAS Experiment*. |
| **Layer** | Object Kind | A structural unit that is a mereological component of a *Neural Network* (`is_component_of`) and bears its own configurable hyperparameters (*Layer Hyperparameters*). |
| **Hyperparameters** | Quality Category | A configuration category representing the configurable, non-learned attributes of a model, specializing into *Neural Network Hyperparameters* and *Layer Hyperparameters*. |
| **Neural Network Hyperparameters** | Quality SubKind | A *Hyperparameters* specialization inhering in a *Neural Network*, representing its configurable, non-learned attributes. |
| **Layer Hyperparameters** | Quality SubKind | A *Hyperparameters* specialization inhering in a *Layer* that specifies configurable architectural characteristics of that layer (e.g., number of units, kernel size, stride, activation function), whose assigned value is externalized through the `has_reified_quality_value` relation. |
| **Hyperparameters Value** | QualityValue AbstractIndividualType | A concrete value (*quale*) in the quality structure of *Layer Hyperparameters*, associated via the `has_reified_quality_value` relation. |
| **Hyperparameters Structure** | AbstractIndividualType | An organized space of admissible hyperparameter configurations that defines the structure of *Hyperparameters* (`defines_structure_of`) and is materialized by the *Search Space*. |
| **Search Space** | Object Kind | A substantial entity that materializes the *Hyperparameters Structure* (`materializes`) to delimit the admissible configurations to be explored, participating in the *NAS Experiment*. |
| **Candidate Architecture** | Object Role | A role played by a *Neural Network* when it is created within the *NAS Experiment*, evaluated by the *Performance Estimation Strategy*, and characterized by the *Neural Network Metrics* quality. |
| **Final Architecture** | Object Role | A role played by a *Neural Network* when it is created and selected as the outcome of the *NAS Experiment*. |
| **Neural Network Metrics** | Quality Kind | An intrinsic quality inhering in a *Candidate Architecture* that measures its observed performance, produced by the *Performance Estimation Strategy* (`produces`). |
| **NAS Experiment** | Event EventType | A perdurant representing the temporally bounded execution of a NAS run, triggered by an initial situation (*Initial System Configuration*) and resulting in a finished-experiment situation (*The experiment has finished*), aggregating the participations of its substantials. |
| **Initial System Configuration** | Situation SituationType | An initial situation capturing the pre-state necessity of a system for a trained network, which contributes to trigger (`contributed_to_triggers`) the *NAS Experiment*. |
| **The experiment has finished** | Situation SituationType | A post-state situation brought about (`brought_about`) by the completion of the *NAS Experiment*. |

## Competency questions and DL queries

The competency questions were elaborated as functional requirements in natural language and then
translated into Description Logic queries in **Manchester syntax**. They can be executed in
Protégé's **DL Query** tab after starting the reasoner.

| # | Competency Question | DL Query (Manchester Syntax) |
| --- | --- | --- |
| **1** | What was the specific search strategy algorithm that participated in the experiment? | `Search_Strategy and participatedIn value nas_experiment_1` |
| **2** | What was the performance estimation strategy used to evaluate the candidate architectures in the experiment? | `Performance_Estimation_Strategy and participatedIn value nas_experiment_1 and evaluates min 1 Candidate_Architecture` |
| **3** | What exact dataset participated in the search experiment? | `Dataset and participatedIn value nas_experiment_1` |
| **4** | Which neural network metrics were produced by the performance estimation strategy during the experiment? | `Neural_Network_Metrics and inverse produces some (Performance_Estimation_Strategy and participatedIn value nas_experiment_1)` |
| **5** | Which hardware device, including its specific subclass, served as the environment for the execution of the experiment? | `Hardware_Device and participatedIn value nas_experiment_1` |
| **6a** | How many cores did the hardware used in the experiment have? | `Core and isComponentOf exactly 1 (Hardware_Device and participatedIn value nas_experiment_1)` |
| **6b** | What operating frequency did the hardware used in the experiment have? | `Operating_Frequency and inheresIn exactly 1 (Hardware_Device and participatedIn value nas_experiment_1)` |
| **7a** | What was the memory component of the hardware where the experiment was executed? | `Memory and isComponentOf exactly 1 (Hardware_Device and participatedIn value nas_experiment_1)` |
| **7b** | What was the capacity of that memory component? | `Memory_Capacity and inheresIn exactly 1 (Memory and isComponentOf exactly 1 (Hardware_Device and participatedIn value nas_experiment_1))` |
| **8a** | Which hyperparameter values structured the search space participating in the experiment? | `Hyperparameters_Value and inverse hasReifiedQualityValue some (Layer_Hyperparameters and inverse definesStructureOf exactly 1 (Hyperparameters_Structure and inverse materializes some (Search_Space and participatedIn value nas_experiment_1)))` |
| **8b** | Which layer hyperparameters structured the search space participating in the experiment? | `Layer_Hyperparameters and hasReifiedQualityValue exactly 1 Hyperparameters_Value and inverse definesStructureOf exactly 1 (Hyperparameters_Structure and inverse materializes some (Search_Space and participatedIn value nas_experiment_1))` |
| **9** | Which candidate architecture was evaluated by which performance estimation strategy? | `Candidate_Architecture and wasCreatedIn value nas_experiment_1 and inverse evaluates exactly 1 (Performance_Estimation_Strategy and participatedIn value nas_experiment_1)` |
| **10** | What was the final architecture created as a result of the completion of the experiment? | `Final_Architecture and wasCreatedIn some ({nas_experiment_1} and broughtAbout exactly 1 The_Experiment_Finished)` |
| **11a** | Which layers compose the network architecture generated in the experiment? | `Layer and isComponentOf exactly 1 ((Candidate_Architecture or Final_Architecture) and wasCreatedIn value nas_experiment_1)` |
| **11b** | What are the specific hyperparameters of those layers? | `Layer_Hyperparameters and inheresIn exactly 1 (Layer and isComponentOf exactly 1 ((Candidate_Architecture or Final_Architecture) and wasCreatedIn value nas_experiment_1))` |
| **12** | What initial system configuration or necessity situation triggered this experiment? | `Initial_System_Configuration and contributedToTrigger value nas_experiment_1` |

## Citation

If you use OntoNAS, please cite the paper:

```bibtex
@inproceedings{kremer2026ontonas,
  title     = {{OntoNAS}: A {UFO}-Based Ontology for Neural Architecture Search},
  author    = {Kremer, Gustavo Ribeiro and
               Moura, Karina Vargas de and
               Teixeira, Gustavo Santos and
               Carbonera, Joel Lu{\'i}s and
               Prestes e Silva Junior, Edson and
               Antunes, Cau{\^e} Roca},
  booktitle = {Proceedings of the 19th Seminar on Ontology Research in Brazil (ONTOBRAS 2026)
               and 10th Doctoral and Masters Consortium on Ontologies (WTDO 2026)},
  series    = {CEUR Workshop Proceedings},
  address   = {Vit{\'o}ria, ES, Brazil},
  year      = {2026},
  publisher = {CEUR-WS.org}
}
```

## License

Released under
[Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/),
the same license under which the paper is published.
