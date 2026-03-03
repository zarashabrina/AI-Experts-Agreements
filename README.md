# AI vs Human Expert Reasonings: Assessing Agreements in Building Typology Predictions based on Street View Imageries

**Authors**: Zahratu Shabrina, Muhammad Asa, Jin Rui, Lu Yin, Stephen Law
**Date**: 2025

## Overview
This repository supports the research on predicting building typologies (construction materials, current use, and storeys) using Google Street View imageries and Vision-Language models.

## Workflow
### Building image query
0. Download building footprints data from OSM (Notebook 0).
1. Download GSV images using Street View Static API (Notebook 1).

### Image pre-processing
2. Filter out GSV images that does not intersect road networks (using spatial relationship) and images with terrible obstruction (using VLM) (Notebook 2).

### Building typologies prediction
3. Predict building typologies using VLMs (Google's, Anthropics's, and OpenAI's) with 2,000 sampled images to find prompting type which yields highest accuracy (prompt optimization phase) (Notebook 3_1_1, Notebook 3_1_2, Notebook 3_1_3).
4. Analyze reasonings from Step 3 using LDA, assess accuracies, and create confusion matrices (Notebook 3_2, Notebook 3_3).
5. Use the prompting type which yield highest accuracy to predict all images (CoT Merged) (Notebook 3_1_1, Notebook 3_1_2, Notebook 3_1_3).
6. Analyze reasonings from Step 5 using LDA, assess accuracies, find top 10 mistakes, and compare AI vs. human precursors (Notebook 3_2, Notebook 3_4).

## Structure
- `codes/` — Jupyter Notebooks and essential files/directories used for analyses.
- `codes/helper` — Files required to help the analysis (API keys, prompts, etc.).
- `codes/input` — Spatial data to support the analysis (administrative areas, road networks, and neighborhood areas).
- `codes/output` — Directory to store results from Jupyter Notebooks.
- `codes/temp` — Temporary files and logs.
- `0_download_building_data.ipynb` — Download building footprints data from OSM.
- `1_gsv_query.ipynb` — Download GSV images using Street View Static API.
- `2_gsv_filtering.ipynb` — Filter out GSV images that does not intersect road networks (using spatial relationship) and images with terrible obstruction (using VLM).
- `3_1_1_google.ipynb` — Predict building typologies using Google's VLM (Gemini 2.0 Flash).
- `3_1_2_anthropic.ipynb` — Predict building typologies using Anthropics's VLM (Claude 3.5 Sonnet).
- `3_1_3_openai.ipynb` — Predict building typologies using OpenAI's VLM (GPT-4o).
- `3_2_lda.ipynb` — Analyze VLMs' reasonings using LDA.
- `3_3_prompt_opt_accuracy_and_confusion_matrices.ipynb` — Assess accuracies and create confusion matrices from typologies predictions (2,000 images).
- `3_4_final_accuracy_and_precursors.ipynb` — Assess accuracies from the final prediction (~30,000 images)

building-typology-prediction/
├─ README.md
├─ environment.yaml
├─ .gitignore
├─ docs/
│   └─ index.html
├─ codes/
│   ├─ helper/
│   ├─ input/
│   ├─ output/
│   └─ temp/  

## Configure essential services
Before running the codes, make sure to set up some essential APIs and credentials, and save them to their respective path when necessary (unless save path unspecified below):
1. [Google Street View Static API](https://developers.google.com/maps/documentation/streetview/get-api-key#creating-api-keys) —> codes/helper/gsv_token.json
2. [Google Cloud Storage (GCS) Bucket](https://docs.cloud.google.com/storage/docs/creating-buckets)
3. [Google Credentials](https://developers.google.com/workspace/guides/create-credentials#create_credentials_for_a_service_account) —> codes/helper/google_credentials.json
3. [Google Vertex AI API](https://docs.cloud.google.com/vertex-ai/docs/featurestore/setup)
4. [Anthropic API](https://console.anthropic.com/settings/keys) —> codes/helper/anthropic_key.json
5. [OpenAI API](https://platform.openai.com/api-keys) —> codes/helper/openai_key.json

Additionally, install [Ollama's app](https://ollama.com/) and afterwards install a LLaVA model using `ollama run llava:7b-v1.6-mistral-q4_0` in your terminal.

## Usage
git clone https://github.com/YOUR_USERNAME/building-typology-research.git
cd building-typology-prediction
conda env create -f environment.yml
jupyter lab
