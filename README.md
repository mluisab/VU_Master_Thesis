# Information Sciences Thesis Project
## Knowledge-Driven Synthetic Dataset Generation for Clinical Risk Classification: A Methodology for Low-Data Environments
This project, titled "Knowledge-Driven Synthetic Dataset Generation for Clinical Risk Classification: A Methodology for Low-Data Environments", is part of a thesis project for the Master in Information Sciences at Vrije Universiteit Amsterdam. Working in collaboration with Pal (Amie Technologies B.V) as a project partner, this study proposes a methodology for knowledge-driven synthetic patient data. 

### Objectives
- Establish a process for eliciting and acquiring clinical knowledge relevant for data generation
- Define a process of formalizing elicited knowledge into a machine-readable input, resulting in a synthetic patient dataset for opioid toxicity risk classification
- Evaluate and validate the generated dataset as a proof of concept, from clinical plausibility and functional utility perspective

### Repository Structure
| File | Project phase | Description |
|---|---|---|
| `feature_spec.json` | Phase 2: Knowledge formalization | Data specification of all clinical features elicited or acquired from expert interviews and clinical guidelines. These include symptoms, risk factors and vitals along with their permitted values, conditional associated values, probabilities, full names and SNOMED CT codes. |
| `data_generation.ipynb` | Phase 3: Synthetic Data Generation and Labelling | Data generation script creating patient scenarios, validating them against rules defined by domain experts and constructing the overall synthetic dataset containing 350 patient records.
| `dataset_c1.csv` `dataset_c2.csv` `dataset_c3.csv` |Phase 3: Synthetic Data Generation and Labelling | Datasets labelled independently by clinicians based on a three level, categorical risk classification: Low, Medium and High Risk.|
| `dataset_analysis.ipynb` | Phase 3: Synthetic Data Generation and Labelling | This notebook serves the purpose of exploring and analyzing the synthetic patient scenario dataset after labelling, and prior to validation. The analysis includes Exploratory Data Analysis, as well as inter-rater agreement. | 
| `MV_dataset.csv` |Phase 3: Synthetic Data Generation and Labelling | *Final dataset* with the final risk labels, where disagreements are resolved based on majority-vote|
| `knowledge_graph.ipynb` |Phase 4: Knowledge Representation and Semantic Mapping | Development of a clinical knowledge RDF/Turtle graph created from the synthetic patient records generated, mapping entities and relationships to standard vocabularies such as Schema.org, SNOMED CT, or OBO. |
| `knowledge_graph.ttl` `knowledge_graph.graphml` | Phase 4: Knowledge Representation and Semantic Mapping | Output of Phase 4, containing a serialized RDF/Turtle knowledge graph and graphml file used to create an interactive visualisation, available at [Gephi Lite](https://lite.gephi.org/?file=https://gist.githubusercontent.com/mluisab/a48178b9ed4db858a2bf1c46e03c15c9/raw/870e83dfb02ed8b38000e6057bbf911e424ce729/knowledge_graph.graphml.json). |
| `ML_evaluation.ipynb` | Proof of Concept Evaluation | Evaluates the dataset's functional utility through five models including Logistic Regression, Random Forest, XGBoost, SVC, and TabPFN. The models are first analyzed based on baseline performance, followed by class-weighted, and SMOTE-NC balancing strategies. |
| `survey_analysis.ipynb` | Proof of Concept Evaluation | Evaluates the dataset's clinical plausibility by analyzing the results of the clinician survey. |

### Dataset
All patient records within the dataset are fully synthetic, and no real patient information was used throughout this project. The dataset is structured in a tabular format, with columns representing clinical features and rows representing one patient scenario. The following data is included:
- patient_ID: representing a random ID number to identify patient scenarios
- Symptom, risk factors and vital feature codes, where codes ending in -000 represent the presence of core clinical features except for MED-OPD-001, MED-OPD-002, and MED-OPD-003 which are related to the patient's opioid treatment.
- Patient_Summary: a narrative, natural language text summarizing the patient's health profile
- Outcome: the risk level of having opioid toxicity as labelled by domain experts

Note: the synthetic dataset generated is a proof of concept intended to demonstrate the established methodology, and is not a benchmark resource for clinical decision support systems.

### Tables and Figures
The following notebooks generate figures and tables presented within the report:
- `feature_spec.json`: The output of this file is presented as a listing within the report, specifically Listing 1: Example Symptom Entry JSON Specification and Listing 2: Example Vital Entry JSON Specification
- `dataset_analysis.ipynb`: The output of this analysis includes Figure 3: Count of Clinical Features per Risk Level (rf_count. pdf and symptom_count.pdf); Figure 4: Cramer’s Analysis of Association of Features and Risk Level (cramer_analysis.pdf); Figure 5: Spearman Correlation Analysis for Vital Signs (spearman_analysis.pdf); Figure 8: Vital Measurement Ranges across Risk Level (vital_count.pdf); Figure 9: Cramer’s Association Matrix (cramer_matrix.pdf); Figure 10: Cramer’s Association Matrix – Conditional Features (cramer_matrix_conditional.pdf)
- `ML_evaluation.ipynb`: The output of this analysis includes the following figures and tables within the report Table 4: Baseline Model Performance, Table 5: Model performance with class weighting, Table 6: Model performance with SMOTE-NC, Figure 13: Confusion Matrices of Baseline Models (LG-confusion.pdf, RF-confusion.pdf, SVM-confusion.pdf, TPFN-confusion.pdf, XGB-confusion.pdf)
- `survey_analysis.ipynb`: The output of this analysis is presented as Figure 6: Plausibility Ratings across Scenarios (overall_rating.pdf); Figure 12: Distribution of Plausibility Scores across Scenarios (rating_distribution.pdf)
- `knowledge_graph.ipynb` and `knowledge_graph.graphml`: These files were used to create an interactive graph visualisation as mentioned in the report, Figure 11: Knowledge Graph Visualisation. 

### Reproducing Results

#### Requirements
```
pandas
numpy
pydantic>=2.0
rdflib
networkx
scikit-learn
xgboost
tabpfn-client
imbalanced-learn
statsmodels
seaborn
matplotlib
openpyxl
scipy
python-dotenv
```

#### Step-by-step instructions
In order to reproduce the results of the research, run the notebooks in the following order:

##### Knowledge-driven synthetic data generation:
1. `data_generation.ipynb`: Generates the synthetic patient dataset without risk levels. Output is `dataset.csv`. To note, generation of records may produce different scenarios from the original dataset due to probabilistic and rejection-based sampling. To recreate analysis results, `MV_dataset.csv` should be used as the original dataset with labels.
2. `dataset_analysis.ipynb`: Runs exploratory data analysis. Requires the independent clinician-labelled datasets (`dataset_c1.csv`, `dataset_c2.csv`, `dataset_c3.csv`) and the final dataset with the merged risk labels (`MV_dataset.csv`). Output is EDA figures and inter-rater agreement analysis.
3. `knowledge_graph.ipynb`: Creates the knowledge graph serialised through RDF Turtle. Requires access to the labelled dataset (`MV_dataset.csv`) and the JSON data specification file (`feature_spec.json`).
  
##### Evaluation of proof of concept
1. `survey_analysis.ipynb`: Runs analysis of survey results, requires survey data (`validation_survey.xlsx`).
2. `ML_evaluation.ipynb`: Runs all ML models on the generated dataset. Requires access to the labelled dataset (`MV_dataset.csv`). To note, the TabPFN model requires an API key, which can be received once a free account is created.

## Authors and Acknowledgements
The author of this repository is Maria Luisa Baba (VunetID: pmb772, Student no: 2896045), with the project being developed in collaboration with Pal Helps (Amie Technologies B.V).
