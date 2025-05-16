# LLM Prompt Evaluation

## Project Structure

```
.
├── data/                              # Processed data files
│   ├── full_results_llm_gpt4o_new_exo.csv
│   └── parsed_results_gpt4o_new_exo.xlsx
├── gpt4_data_new_exo/                 # Raw LLM output files
├── tasks/                             # Exercise definitions
│   └── tasks_for_llm_gpt4_new_exo.xlsx
├── pre_proc_outputs.ipynb             # Pre-processing notebook
├── processing_llm_outputs.ipynb       # Analysis notebook
├── llm_generation.json                # Config file for the streamlit app
└── README.md                          # This file
```

## Workflow

The evaluation process consists of 3 main steps:

### 1. Pre-processing (pre_proc_outputs.ipynb)

This notebook handles the initial data preparation:

- Reads exercise definitions from `tasks/tasks_for_llm_gpt4_new_exo.xlsx`
- Parses raw LLM output files from the `gpt4_data_new_exo` directory
- Extracts user prompts, model responses, and relevant metadata
- Organizes the data into a structured format
- Saves the processed data to `data/parsed_results_gpt4o_new_exo.xlsx`

### 2. Run the LLM analysis with the streamlit app

Run the automactic analysis using the streamlit app to get classification labels

### 3. Analysis (processing_llm_outputs.ipynb)

This notebook performs the evaluation and analysis:

- Loads the processed data from `data/full_results_llm_gpt4o_new_exo.csv`(or the name of the exported file at the end of the LLM analysis)
- Maps prediction labels (0/1/2) to points
- Calculates raw grades for each model, exercise, and prompt type
- Computes "goodised" grades (ensuring higher = better for both prompt types)
- Creates a Balanced-Prompt Grade (BPG) that combines results from both prompt types
- Generates model-level summaries across all exercises
- Ranks exercises by difficulty for each model
- Analyzes response lengths and other metrics

## Usage

1. Place raw LLM output files in the `gpt4_data_new_exo` directory
2. Run the notebook `pre_proc_outputs.ipynb` to process the raw data
3. In the current version of the streamlit app, and for the config file to work, you need a set of human annotations.
For now, this requires a small, not-ideal manual step:
You need to create three annotation columns in the parsed_results_gpt4o_new_exo.xlsx file.
Add the columns "Rater_Oli", "Rater_Chloe", and "Rater_RA".
Fill the first row of each of these columns with a 1.
4. Go to https://flowanalysis.streamlit.app/.
5. Upload the parsed_results_gpt4o_new_exo.xlsx file.
6. Upload the llm_generation.json config file.
7. Nothing needs to be done for steps 2, 3, and 4 of the app.
8. At step 5 in the app, select Azure as the provider and gpt4o as the model.
9. At step 6 in the app, run on All rows (since there is only one annotated entry anyway).
10. Then, go directly to step 8 where you can run on all the data and export the annotated file.
11. Run the notebook `processing_llm_outputs.ipynb` to perform the analysis
