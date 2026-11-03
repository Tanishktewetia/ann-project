# Mobile Price Classification Lab

A static report website for the project's saved mobile-price model results. It is designed to deploy directly from this repository on Vercel: no database, API server, Python runtime, or repeat training step is required.

## What the website includes

- **Overview** — dataset and evaluation snapshot, best saved holdout score, and a comparison chart.
- **Model bench** — training, holdout, and cross-validation accuracy for the 12 models in the saved summary.
- **Predictions** — filter the precomputed 1,000-row test outputs by model, class, or record ID; preview a stored prediction and download its CSV.
- **Price estimator** — enter the model’s 20 hardware/connectivity features and view an animated, browser-side demonstration across four relative price classes.
- **Model insights** — saved confusion-matrix and SHAP plots.
- **About this study** — what the report includes and deployment notes.

The prediction preview includes a short, clearly described preparation animation. It reveals a row already present in that model CSV; it does **not** run inference or train a model. The Price estimator uses a simple illustrative browser-side heuristic: it does not invoke the trained model, and its demo score is neither a probability nor a calibrated prediction. Test-set rows have no known target in `test.csv`, so the website does not claim test-set accuracy.

## Deploy on Vercel

1. Push or connect this repository to your Vercel account.
2. Create a Vercel project for the repository. Set **Root Directory** to the folder containing this `README.md`, `index.html`, and `vercel.json` (the `ann-project` folder in the provided desktop layout).
3. Choose **Other** as the framework preset. Leave install and build commands empty; the site is plain HTML, CSS, and JavaScript.
4. Deploy the project root as the static output. No environment variables are needed.
5. Verify the Overview, Model bench, Predictions filters/CSV download, and Model insights plots in the deployment.

`vercel.json` enables clean URLs and disables trailing slashes. All pages are sections of one static HTML app, with hash navigation; data and images are served as ordinary files from `ann/results/`.

## Run locally

Serve the repository root with any static file server. For example:

```bash
python -m http.server 8000
```

Then open `http://localhost:8000`. Opening `index.html` directly as a `file://` URL may block browser `fetch()` calls to the saved CSV files.

## Project contents

```text
index.html              Static website entry point
styles.css              Responsive interface styles
app.js                  Page navigation and saved-output rendering
vercel.json             Vercel static-site settings
favicon.svg             Project mark
models_description.md   Model notes
ann/
  dataset/               Existing train/test CSV files
  models/                Saved trained models and preprocessing artifacts
  results/
    model_comparison/    Accuracy report and saved model plots
    predictions/         Precomputed predictions CSV for each model
    advanced_pipeline/   Additional saved comparison and explanation outputs
```

## Data and maintenance notes

- The model bench reads `ann/results/model_comparison/accuracy_summary.txt` at runtime and displays the saved training, testing/holdout, and cross-validation accuracy values.
- Predictions are loaded from `ann/results/predictions/<model>/predictions.csv`. The website parses these files in the browser for filters, counts, previews, and downloads.
- The **testing accuracy** label in the original report refers to its validation/holdout evaluation split. It is not a score on the separate unlabeled `ann/dataset/test.csv` file.
- Model images are the saved PNGs under `ann/results/model_comparison/`. No graph generation occurs in the browser.
- The website does not load or execute the `.pkl` files. It presents existing results only.
- To refresh this report after intentionally changing analysis outputs, keep the summary, prediction CSVs, and plot paths in sync. A static deployment serves the files present in the deployed commit; local, uncommitted changes are not automatically published.

## Original ML workflow

The Python analysis scripts and dependencies remain in `ann/`. They are separate from the static website and are not used by Vercel to serve it. Run the existing pipeline locally only when you deliberately want to regenerate results; it is not required for deploying or viewing this report.
