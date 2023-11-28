# Challenge #1 method summary

## Resources Used

PubMed articles were searched and collected using the NCBI EUtilities API. This provided an efficient method to programmatically query and retrieve literature related to BCRP.
Additional data sources like PubChem was used to retrieve chemical identifiers (CIDs) and structures in Canonical SMILES format for the extracted drug names. The PubChem PUG REST API was utilized.

## Document Collection

The search_pmc function was written to leverage the NCBI EUtilities ESearch and EFetch APIs. It searches PubMed for relevant articles containing terms for "BCRP OR ABCG2 OR Breast Cancer Resistance Protein".
Filters were applied to only retrieve free full-text articles in PubMed Central. In total 5000 articles were collected by searching until reaching the maximum number of retrievable results.
The get_abstract function was used with the EFetch API to retrieve JSON or XML abstracts, which were then parsed and processed.

## Data Extraction and Labeling

Abstract texts were processed to remove HTML tags using BeautifulSoup and regular expressions.
Cleaned abstracts were input to a spaCy pipeline with named entity recognition to extract drug name entities. Documents containing exactly one drug name were filtered out.
The filtered abstracts and drug names were fed as input prompts to the GPT-3.5 Turbo model via the OpenAI API, configured with instruction tuning to classify the drugs as either BCRP inhibitors or substrates based on the context.
A robust retry mechanism using Tenacity was implemented to handle potential transient errors from the API.

## Validity Checking

For drugs with conflicting labels across documents, the majority label was taken to resolve conflicts.
Additionally, 10-15 drugs were manually checked by close reading of the abstracts in articles. On spot checking, the automated labeling was found to be highly accurate.


## Adaptability

The NCBI API search could be customized to find articles on any protein by changing the search query terms. The spaCy NER model could be retrained or swapped to recognize drug names and other entities relevant to a new topic. The GPT-3.5 instructions for labeling could be adapted by providing examples of text labeling other entities like cancer outcomes or protein interactions rather than transporter activities. Overall, the workflow could be configured to work for other proteins or biomedical applications.

## Advanced Project Scope

### Leveraging LLMs for Weak Supervision
Manually labeling data for model training can be very costly and time consuming. Recent advances have explored using large language models (LLMs) like GPT-3 to automatically generate labeled data, an approach called weak supervision.

The labels produced by LLMs may not be perfectly accurate, but provide a strong starting point for dataset creation. This data can then be used to fine-tune microsoft/BiomedNLP-BiomedBERT-base-uncased-abstract-fulltext for complex NLP tasks like biomedical text classification.

As these model trains on the LLM-labeled data, it learns to correct errors and become more robust at the classification task. The end model can be evaluated on a small hand-labeled dataset to validate its performance.

This paradigm allows companies to scale up labeled datasets and train accurate models with much lower manual annotation costs. The demonstration of interacting with GPT-3 to label BCRP inhibitor status in this project serves as an example of weak supervision for biomedical literature mining.