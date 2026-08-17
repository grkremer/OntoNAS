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
| **Performance Estimation Strategy** | Object SubKind | An *Algorithm* that estimates the quality of candidate architectures and monitors the progress of the search, enabling the evaluation of candidates without necessarily fully training them. |
| **Data** | Collection Kind | An information consumable by computational procedures during a search run. |
| **Dataset** | Object SubKind | A *Data* specialization comprising a curated collection of samples, it participates in the *NAS Experiment* as informational resource and bears the *Structured* quality. |
| **Structured** | Quality Kind | An intrinsic quality inhering in a *Dataset* that characterizes the organization of its samples, whose possible values (structured, semi-structured, unstructured) are qualia in its quality structure. |
| **Hardware Device** | Object Kind | A physical device that provides the material substrate supporting execution, it participates in the *NAS Experiment*, has *Core* and *Memory* components, and bears its operational qualities. |
| **CPU** | Object SubKind | A *Hardware Device* specialization denoting general-purpose processing units employed as the substrate of an experiment. |
| **GPU** | Object SubKind | A *Hardware Device* specialization denoting massively parallel processing units used to accelerate training and evaluation. |
| **Core** | Object Kind | A processing unit that is a component of a *Hardware Device*. |
| **Memory** | Object Kind | A storage component of a *Hardware Device* that bears the *Memory Capacity* quality. |
| **Operating Frequency** | Quality Kind | An intrinsic quality inhering in a *Hardware Device* that measures the clock rate at which it operates. |
| **Memory Capacity** | Quality Kind | An intrinsic quality inhering in a *Memory* component that quantifies its storage capacity. |
| **Neural Network** | Object SubKind | An artificial neural architecture composed of *Layer* components that bears the *Neural Network Hyperparameters* quality, it is produced in the course of a *NAS Experiment*. |
| **Layer** | Object Kind | A structural unit that is a mereological component of a *Neural Network* and bears its own configurable hyperparameters (*Layer Hyperparameters*). |
| **Hyperparameters** | Quality Category | A configuration entity representing the configurable, non-learned attributes of a model, it specializes into *Neural Network* and *Layer* hyperparameters. |
| **Neural Network Hyperparameters** | Quality Kind | A *Hyperparameters* specialization inhering in a *Neural Network*, representing its configurable, non-learned attributes. |
| **Layer Hyperparameters** | Quality Kind | A specialization of *Hyperparameters* that inheres in a *Layer* and specifies one or more configurable architectural characteristics of that *layer* (e.g., number of units, kernel size, stride, activation function). Its magnitude or assigned value is externalized through the *has_value* relation. |
| **Hyperparameters Value** | QualityValue AbstractIndividualType | A concrete value in the quality structure of a *Layer Hyperparameters*, the *quale* that the hyperparameter actually assumes (e.g., a specific learning rate or kernel size). It is externalized from its *Layer Hyperparameters* via `has_value` and figures as a `member_of` the *Hyperparameters Structure*, which aggregates the admissible values defining the search space. |
| **Hyperparameters Structure** | AbstractIndividualType | An organized space of admissible hyperparameter configurations. |
| **Search Space** | Object Kind | A role played by a *Hyperparameters Structure* when it delimits the admissible configurations to be explored, it participates in the *NAS Experiment*. |
| **Candidate Architecture** | Object Role | A role played by a *Neural Network* when it is proposed by the *Search Strategy* and evaluated by the *Performance Estimation Strategy* within the *NAS Experiment*, it bears the *Neural Network Metrics* quality. |
| **Final Architecture** | Object Role | A role played by a *Neural Network* when it is selected by the *Performance Estimation Strategy* as the outcome of the *NAS Experiment*. |
| **Neural Network Metrics** | Quality Kind | A quality inhering in a *Candidate Architecture* that measures its observed performance once trained and evaluated. |
| **NAS Experiment** | Event EventType | A perdurant representing the temporally bounded execution of a NAS run, triggered by the need for a trained network and resulting in a finished-experiment situation, it aggregates the participations of its substantials and in its course *Candidate* and *Final Architectures* come to exist. |

## Competency questions and DL queries

The competency questions were elaborated as functional requirements in natural language and then
translated into Description Logic queries in **Manchester syntax**. They can be executed in
Protégé's **DL Query** tab after starting the reasoner.

| # | Competency Question | DL Query (Manchester Syntax) |
| --- | --- | --- |
| **1** | What was the specific search strategy algorithm that participated in the experiment? | `search_strategy and participatedIn some nas_experiment` |
| **2** | What performance estimation strategy was used to evaluate the candidate architectures? | `performance_estimation_strategy and participatedIn some nas_experiment and evaluates some candidate_architecture` |
| **3** | What exact dataset participated in the experiment? | `dataset and participatedIn some nas_experiment` |
| **4** | Which neural network metrics were produced during the experiment? | `neural_network_metrics and inheresIn some (candidate_architecture and wasCreatedIn some nas_experiment)` |
| **5** | Which hardware device (CPU or GPU) served as the execution environment? | `hardware_device and participatedIn some nas_experiment` |
| **6a** | How many cores did the hardware have? | `core and isComponentOf some (hardware_device and participatedIn some nas_experiment)` |
| **6b** | What operating frequency did the hardware have? | `operating_frequency and inheresIn some (hardware_device and participatedIn some nas_experiment)` |
| **7** | What was the memory capacity of the hardware's memory component? | `memory_capacity and inheresIn some (memory and isComponentOf some (hardware_device and participatedIn some nas_experiment))` |
| **8** | Which candidate architecture was evaluated by which performance estimation strategy? | `candidate_architecture and wasCreatedIn some nas_experiment and inverse evaluates some performance_estimation_strategy` |
| **9** | What final architecture was created as a result of the experiment's completion? | `final_architecture and wasCreatedIn some (nas_experiment and broughtAbout some the_experiment_finished)` |
| **10** | What system necessity situation triggered the execution of the experiment? | `the_necessity_of_a_system_for_a_trained_neural_network and contributedToTrigger some nas_experiment` |

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
