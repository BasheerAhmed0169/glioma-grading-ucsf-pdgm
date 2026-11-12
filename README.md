# Code and results: Multimodal learning for WHO CNS grade classification of gliomas (UCSF-PDGM)

Dataset: Hugging Face `MedOtter/UCSF-PDGM` (495 patients; Calabrese et al., 2022). Notebooks were run on Google Colab.
Each notebook downloads the dataset itself. Clinical inputs: age and sex only. Seeds: 42, 43, 44 (5-fold, patient-level).

## Which notebook gives which part of the paper

| Notebook (code/) | What it runs | Paper part |
|---|---|---|
| 1_classical_5models_complete_training | Frozen ResNet50 features, PCA 32, LR/DT/RF/SVM/HGB; clinical-only, MRI-only, fused; missing-data test, trained on complete data | Tables 1-2, Section 4.1-4.2 |
| 2_classical_5models_with_missing_training | Same, trained with randomly hidden values (p = 0.4) | Table 2, Section 4.3 |
| 3_frozen_cnn_heads_complete_training | Frozen ResNet50 / EfficientNet-B0 + linear head | Tables 1-2 (heads) |
| 4_frozen_cnn_heads_with_missing_training | Linear heads, trained with missing values | Table 2 (heads) - this copy has no saved outputs |
| 5_finetuned_cnn_seed42, 6_finetuned_cnn_seeds43_44 | Fine-tuned MobileNetV3-Small, EfficientNet-B0, ResNet18 (4 sequences, clinical branch, modality dropout) | Table 3, Figures 3-5, Section 4.5 |

Per-fold results of the fine-tuned models: `results/deep_raw_deep_finetune.csv` (720 rows, 15 folds). Summaries and plots are in the other files in `results/`.

## How to run
Open a notebook in Colab, run all cells from top to bottom. Fine-tuned notebooks need a GPU and save checkpoints to Google Drive (paths start with /content/drive).
Library versions are not pinned; see the first code cell of each notebook.
