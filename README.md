# Spectra ML v2.0

**Spectra ML** is a computational tool for estimating molecular UV–Visible absorption maxima (λ max (maximum absorption wavelength)) and generating modelled spectra under selected solvent and experimental-condition inputs. The implementation combines cheminformatics, machine-learning predictors, analogue retrieval, empirical spectral rules, optional quantum-chemical calculations, condition-dependent corrections, uncertainty-related scoring, and visualization in a Google Colab notebook.

- **Version:** `v2.0`
- **Interface:** Google Colab / Jupyter Notebook
- **Primary contact:** [dkbhatt1307@gmail.com](mailto:dkbhatt1307@gmail.com)
- **Secondary contact:** [uv.vis.spectra.ml@gmail.com](mailto:uv.vis.spectra.ml@gmail.com)
- **Notebook:** [Spectra ML v2 google colab](https://drive.google.com/file/d/1M3YMe4WKXlD8cszug6jGbUvHJmmOPIIY/view?usp=drive_link)
- **Repository:** [Spectra ML v2](https://github.com/bhattdha713/Spectra-ML-v2)

## Overview

Spectra ML accepts molecular structures in SDF format and user-selected conditions (solvent, pH, temperature, and concentration). It estimates a primary lambda_max (maximum absorption wavelength), generates a simulated spectrum, and can display confidence-related and applicability-domain information. The notebook also supports solute–solvent interaction reporting, reaction-product candidate analysis, reference-feedback collection, model retraining, locked-test evaluation, subgroup analysis, and ablation studies.

The v2 workflow extends the integrated prediction approach with an adaptive reaction engine, a reaction-feasibility learner, an updated meta-combination of base predictors, and additional interaction and explainability outputs. Optional quantum-chemical calculations and iterative refinement can be selected during execution.

The software is intended for computational investigation, method development, and hypothesis generation in molecular spectroscopy. Quantitative performance should be reported with the exact software/data version, evaluation protocol, sample count, and reference-data provenance. Predictions and modelled spectra should be interpreted alongside experimental evidence where scientific conclusions depend on specific compounds or conditions.

## Key capabilities

- **Structure input:** reads molecules from SDF using RDKit; up to 50 molecules are processed in one notebook run.
- **Molecular representation:** combines molecular descriptors and DeepChem-related features in a configured feature vector padded to 1,024 dimensions.
- **Chromophore analysis:** uses a SMARTS-based chromophore dictionary, conjugation analysis, structural heuristics, and chromophore-family classification.
- **Hybrid prediction:** combines neural-network, ensemble, deep-residual-CNN, equivariant graph-neural-network, and NGBoost predictions through a meta-combiner and residual correction.
- **Analogue retrieval:** retrieves structurally similar reference molecules and uses analogue information as an external chemical-neighbour prior.
- **Empirical spectral rules:** includes Woodward–Fieser, Fieser–Kuhn, and Scott-type rules for selected conjugated and aromatic systems.
- **Solvent and condition handling:** models solvent-dependent effects and uses solvent descriptors, Kamlet–Taft-related modelling, pH, and temperature inputs.
- **Solute–solvent interaction report:** analyses conditional hydrogen bonding and reports modelled, approximated, and omitted interaction contributions.
- **Adaptive reaction engine:** evaluates candidate transformations using SMARTS reaction templates, thermodynamic information, reaction-feasibility modelling, and an adaptive learner.
- **Optional quantum chemistry:** can attempt TD-DFT calculations through PySCF and can extract quantum descriptors when enabled.
- **Conformational treatment:** generates or retrieves conformers and applies conformational correction, including Boltzmann-related handling where configured.
- **Spectrum generation:** constructs wavelength-dependent modelled spectra and applies concentration/path-length scaling and instrument-effect handling.
- **Prediction support indicators:** reports confidence-related scores, nearest analogues, chromophore-family information, and applicability-domain/outlier information.
- **Explainability:** can display SHAP feature contributions when the predictor is trained and the explanation path is available.
- **Feedback workflow:** records reference lambda_max (maximum absorption wavelength) values, tracks errors, retrains selected components, and updates learned artifacts.
- **Evaluation and ablation:** supports locked-test metrics, breakdowns by solvent/wavelength region/chemical class, and selected component-ablation comparisons.
- **Visualization:** plots individual and combined simulated spectra, peak annotations, and feedback-error history.

## Screenshots and example outputs

### Example 1 — Nifuroxazide in ethanol

<img width="1072" height="525" alt="Nifuroxazide in ethanol" src="https://github.com/user-attachments/assets/4339eda7-5217-4b76-b500-e874019033d0" />

The supplied run used ethanol, pH 7.0, temperature 300 K, concentration 0.01 M, TD-DFT disabled, and two refinement iterations. The log reports a final predicted λ max (maximum absorption wavelength) of approximately **328.0 nm**, with confidence `0.19`. The Reference λmax is 367 nm. [British Pharmacopoeia (2025), Nifuroxazide](https://nhathuocngocanh.com/bp/nifuroxazide/).

This is an example of one run, not a general performance estimate. The low reported confidence should be retained when interpreting the output.

## Example 2 - Sudan Brown RR in ethanol 

<img width="1072" height="525" alt="Sudan Brown RR in DMF" src="https://github.com/user-attachments/assets/611bd84b-755c-4de1-9d22-ca4c8f3d694c" />

The supplied run used ethanol, pH 7.0, temperature 300 K, concentration 0.01 M, TD-DFT disabled, and two refinement iterations. The log reports a final predicted λ max (maximum absorption wavelength) of approximately **478.5 nm**, with confidence `0.02`. The Reference λmax is 452 nm. [Santa Cruz Biotechnology COA](https://www.scbio.cn/p/fat-brown-rr-6416-57-5).

This is an example of one run, not a general performance estimate. The low reported confidence should be retained when interpreting the output.

## How to run the notebook

1. Open the [Spectra ML v2 notebook](https://drive.google.com/file/d/1M3YMe4WKXlD8cszug6jGbUvHJmmOPIIY/view?usp=drive_link) in GitHub and open it in Google Colab (or use a compatible Jupyter environment).
2. Run the dependency-installation and import cell, followed by the notebook cells in order.
3. At the model-source prompt, choose one of the following:
   - `1` — attempt to load the shared model from its configured central Google Drive location.
   - `2` — attempt to load saved model/data from your personal Google Drive.
   - `3` — start without loading a saved pre-trained model.
4. Select the solvent by entering its displayed ID code.
5. Enter pH, temperature in kelvin (K), and concentration in molarity (M). The notebook defaults are pH `7.0`, temperature `298.15 K`, and concentration `0.01 M`.
6. Upload an SDF file containing the molecule(s) to process. The notebook processes up to 50 valid molecules from the uploaded SDF file.
7. Answer `y` or `n` to the TD-DFT prompt. Enabling TD-DFT can substantially increase computation time and may fail for some structures or environments.
8. Enter the number of iterative-refinement iterations. Enter `0` to skip refinement; the default is `1`.
9. Review the predicted lambda_max (maximum absorption wavelength), simulated spectra, peak table, confidence-related output, applicability-domain status, analogue information, and any interaction report or warnings.
10. Choose whether to collect reference feedback. When entering feedback, select the appropriate data role (`train`, `validation`, or `test`) and enter the measured/reference lambda_max when the prediction is incorrect.
11. Review the locked-test benchmark and, when sufficient test records are available, optionally run detailed evaluation by solvent, wavelength region, and chemical class, followed by an ablation study.
12. Choose whether to save the updated learning data and model artifacts to Google Drive.

A successful run depends on runtime resources, installed package versions, Google Drive authorization, and access to compatible saved model/data files. Starting fresh does not imply that a model has been trained on an adequate dataset.

## Model-source options

| Choice | Behaviour |
|---|---|
| `1` — Shared model | Attempts to load the shared model and related artifacts from the configured central Drive location, if accessible. |
| `2` — Personal Drive | Attempts to load model/data artifacts from the user's configured Google Drive location, if available. |
| `3` — Start fresh | Starts without loading the saved pre-trained model from those locations; subsequent behaviour depends on the available code path and feedback data. |

The repository also contains serialized model and supporting artifacts (for example, model weights, scaler, ensemble, meta-combiner, residual corrector, similarity index, applicability-domain model, and reaction-feasibility learner). The notebook's manifest/version checks determine whether learned artifacts are compatible and trusted for loading. The existence of an artifact file does not, by itself, establish that it is loaded or used in every execution path.

## Computational workflow

At a high level, the notebook follows this workflow:

```text
Molecular structure (SDF) + solvent/pH/temperature/concentration
                              |
                              v
                  RDKit parsing and validation
                              |
                              v
       Molecular features + chromophore/family analysis
                              |
                              v
       Reaction candidate analysis (reaction path enabled)
                              |
                              v
       Base prediction models + analogue retrieval
                              |
                              v
          Meta-combination and residual correction
                              |
                              v
      Solvent / interaction / conformational corrections
                              |
                              v
          Optional TD-DFT and quantum correction
                              |
                              v
              Refinement and spectrum construction
                              |
                              v
       Predicted lambda_max + spectrum + support indicators
                              |
                              v
        Optional reference feedback and selected retraining
                              |
                              v
       Locked-test evaluation / subgroup metrics / ablation
```

This diagram describes the high-level architecture. The active path can vary according to loaded artifacts, installed dependencies, user selections, configuration flags, and whether optional calculations succeed. Reaction candidate analysis is not equivalent to experimental confirmation that a reaction occurs under the selected conditions.

### Main code components

The notebook is organized into eight main sections:

1. **Dependency installation and imports** — installs/imports numerical, cheminformatics, machine-learning, plotting, explainability, and quantum-chemistry libraries.
2. **Configuration** — defines wavelength limits and step size, feature dimensions, neural-network settings, defaults, training/evaluation settings, and reaction-engine settings.
3. **Chromophore detector and solvent database** — contains SMARTS patterns, chromophore logic, and solvent descriptors/parameters.
4. **Thermodynamic reaction engine and adaptive learner** — implements reaction templates, thermodynamic/feasibility handling, and feedback-based learning for reaction feasibility.
5. **Core UV–Vis prediction classes** — includes molecular feature extraction, predictor models, analogue retrieval, meta-combination, solvent and conformational corrections, spectrum generation, confidence scoring, applicability-domain handling, interaction reporting, and visualization.
6. **Empirical rule corrector** — implements selected classical UV–Visible rules, including Woodward–Fieser, Fieser–Kuhn, and Scott-type rules.
7. **Helper functions** — includes random-seed setup, scaffold-aware splitting, artifact loading/saving, and provenance/manifest checks.
8. **Main execution** — handles interactive inputs, prediction, plotting, feedback, optional evaluation, ablation, and saving.

## Methods and equations

### Wavelength grid

The configuration defines a wavelength range from 200 to 800 nm with a 0.5 nm step:

```text
WL_MIN = 200 nm
WL_MAX = 800 nm
WL_STEP = 0.5 nm
```

### Gaussian band construction

The spectrum generator constructs a simulated spectrum by combining Gaussian-shaped bands:

```text
A(lambda) = sum_i [ I_i * exp( -((lambda - lambda_i)^2) / (2 * sigma_i^2) ) ]
```

Here, `lambda_i` is the centre of band `i`, `I_i` is its intensity scale, and `sigma_i` controls band width. This is a spectrum-construction model; the equation alone does not establish that the resulting curve is experimentally calibrated.

### Concentration and path-length scaling

The code applies a Beer–Lambert-style scaling operation:

```text
A_scaled(lambda) = A_model(lambda) * concentration * path_length
```

The configured path length is `1.0` in the notebook. Since the modelled band intensity is not necessarily a calibrated molar absorption coefficient, the scaled values should be interpreted as model outputs unless an experimental calibration establishes their physical scale.

### Empirical spectral rules

The empirical corrector implements selected rules for structural classes, including:

- **Woodward–Fieser rules** for conjugated dienes and selected α,β-unsaturated carbonyl systems.
- **Fieser–Kuhn rule** for suitable extended conjugated polyenes.
- **Scott-type rule** for selected aromatic carbonyl systems.

These rules provide structure-dependent estimates under their respective assumptions. Their applicability depends on the molecule matching the rule's chemical scope. The notebook uses empirical estimates as part of a hybrid correction path; they should not be treated as universally valid for all chromophores.

### Machine learning and hybrid components

The source code includes or configures multiple complementary components. The principal base-model predictions entering the meta-combination are:

- A primary neural-network predictor (`UVVisNN`).
- A random-forest / XGBoost ensemble predictor.
- A deep residual CNN (`DR-CNN`).
- An equivariant graph-neural component (`e3nn`).
- An NGBoost predictor.

The meta-combiner is trained using out-of-fold (OOF) predictions from these five base-model sources. The residual corrector is applied after the meta-combiner to estimate residual error. Analogue retrieval is intentionally kept outside the learned meta-feature stack and is used as a separate chemical-neighbour prior when similarity is meaningful.

Additional components in the notebook include:

- Hückel-based calculation/prediction.
- Chromophore and chromophore-family analysis.
- Chemistry Memory correction.
- Solvent learning and Kamlet–Taft-related modelling.
- Conformational correction.
- Applicability-domain and outlier/anomaly detection.
- Confidence scoring and optional SHAP explainability.

Not every component necessarily contributes to every run. The final estimate depends on loaded artifacts, available dependencies, reference data, configuration settings, and the active execution path. The notebook should therefore be described as a **hybrid prediction workflow**, not as a single universally active model.

### Reaction engine and adaptive reaction learner

The reaction module uses SMARTS-based reaction templates to identify and generate candidate transformations. The source includes reaction classes relevant to solvolysis and other transformations, together with thermodynamic information such as bond-dissociation-energy data and a reaction-feasibility machine-learning component.

The adaptive learner can record feedback and retrain its feasibility model after its configured real-feedback threshold is reached. Reaction predictions are candidate-generation/model outputs; they do not demonstrate that a transformation will occur experimentally. The reaction pathway is separately configured as opt-in, with candidate reporting enabled.

### Solute–solvent interaction analysis

The interaction-report component evaluates selected interaction contributions, including:

- Conditional hydrogen-bond donor/acceptor capability.
- Electronic solvation effects.
- Residual solvent effects.
- Conformational sampling.
- Electrolyte-related contributions.
- Dipole and dispersion contributions.
- Reaction-pathway information.

The report distinguishes contributions marked as modelled, approximated, or omitted, and includes a reliability indicator. This distinction is important: a reported interaction category should not be interpreted as a fully quantum-mechanical or experimentally verified interaction energy unless the relevant calculation and validation support that interpretation.

### Feature representation and reproducibility

The configuration specifies a padded feature dimension of `1024`, neural-network hidden layers `[512, 256, 128, 64]`, dropout `0.3`, learning rate `0.001`, and random seed `42`. The source uses RDKit and DeepChem-related feature extraction. These are implementation settings, not evidence on their own of predictive performance.

The scaffold-splitting helper attempts to group molecules using Bemis–Murcko scaffolds, with a random-split fallback when there are too few scaffold groups. The configuration sets nominal test and validation proportions of 15% each. The effective split should be verified from the saved manifest and actual run log.

## Feedback, training, and evaluation

After predictions, users may provide reference lambda_max (maximum absorption wavelength) values. The feedback workflow records the molecule, prediction, reference value, error, conditions, and data role (train, validation, or test), then retrains selected components and updates related learned artifacts.

**Use measured/reference values for feedback whenever possible.** Do not treat the model's own predictions as independent experimental ground truth. Preserve test-set separation: feedback from test molecules must not be used to train a model that is then evaluated on those same molecules as if the test set were still untouched. Changing a molecule's data role can alter the manifest and evaluation separation.

The notebook can report:

| Metric | Meaning |
|---|---|
| MAE (nm) | Mean absolute difference between predicted and reference lambda_max (maximum absorption wavelength). |
| RMSE (nm) | Root mean squared error; more sensitive to large errors. |
| R-squared | Coefficient of determination; can be negative on evaluation data. |
| SEP | Standard error of prediction as implemented by the notebook. |
| RPD | Ratio of performance to deviation. |
| CCC | Concordance correlation coefficient. |
| ME | Mean error/bias; interpret using the notebook's sign convention. |
| Normalized MAE score | Additional normalized score reported by the locked-test evaluation path. |

Optional subgroup analyses report metrics by solvent, wavelength region, and chemical class. The ablation workflow compares selected component configurations on the selected evaluation records to examine their effect.

### Current repository artifact snapshot

The repository's `metadata.json` and `artifact_manifest.json` record the following saved-data snapshot (metadata last saved on **29 October 2026**):

| Item | Recorded value |
|---|---:|
| Feedback records | 778 |
| Training records in manifest | 608 |
| Validation records in manifest | 81 |
| Test records in manifest | 89 |
| Configured feature dimension | 1,024 |
| Model version in artifacts | 13.7.3 |
| Feature version in artifacts | 1.0.0 |
| Conformational-corrector training samples in metadata | 643 |

These are artifact/manifest counts, not independently verified performance results. They do not substitute for reporting locked-test MAE, RMSE, R², sample inclusion/exclusion, reference-data provenance, and the exact model artifact used for a benchmark.

## Interpretation of confidence and applicability

The notebook produces confidence-related scores and applicability-domain information using signals such as similarity, outlier/applicability checks, ensemble variation, chromophore-family information, and optional quantum-related information. These values are implementation-specific indicators of prediction support. Unless calibration has been independently demonstrated, they should not be interpreted as probabilities that a prediction is correct or as formal prediction intervals.

A low applicability status, unfamiliar structure, weak analogue match, large disagreement between prediction components, missing optional calculation, or interaction report with low reliability should prompt closer review of the result.

## Current scope and limitations

- Prediction quality depends on the amount, quality, diversity, and provenance of the reference data.
- Spectral behaviour can depend on solvent, pH, temperature, concentration, ionization, tautomerism, aggregation, and experimental protocol. These effects may not be fully represented for every compound.
- Structural heuristics and empirical rules have defined applicability ranges and can fail on unusual or complex molecules.
- The generated spectrum is modelled; its intensity scale should not be assumed to be calibrated absorbance or molar absorptivity without supporting calibration.
- TD-DFT is optional, method-dependent, computationally demanding, and may fail for some structures or runtime environments.
- Reaction-engine outputs are candidate transformations and feasibility estimates, not proof of reaction occurrence under experimental conditions.
- Interaction-report categories can include approximated or omitted contributions; consult the report labels and reliability indicator.
- Applicability and confidence outputs should be interpreted according to their implementation and validation.
- Results can depend on saved model artifacts, package versions, Drive permissions, and runtime resources.
- The notebook processes up to 50 valid molecules per uploaded SDF file in one run.
- The repository's saved artifacts and data snapshot can change as feedback is collected and models are retrained. Record the exact artifact/manifest version used for any reported analysis.

These considerations are part of documenting the scope of the software and supporting reproducible use. Performance claims should be based on a documented evaluation protocol and reference measurements appropriate to the intended application.

## Generative-AI disclosure

The core project idea and its inspiration are original to the project author. ChatGPT and DeepSeek were used as assistance tools for debugging, improving understanding of selected code, and reviewing the README wording. They were not the origin of the project's central idea. The author is responsible for the software, scientific claims, verification, and final published content.

## Citation and release

- **Software:** Spectra ML
- **Version:** v2.0
- **GitHub:** [Spectra ML v2](https://github.com/bhattdha713/Spectra-ML-v2)
- **Notebook:** [Spectra ML v2 Jupyter Notebook](https://drive.google.com/file/d/1M3YMe4WKXlD8cszug6jGbUvHJmmOPIIY/view?usp=drive_link)
- **Contributors:** Dhairya Bhatt, Avani Doshi, Mishti Tekwani
