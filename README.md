# PhyloCompoCLR v2

**Phylogeny-aware contrastive learning for multi-disease microbiome classification**

PhyloCompoCLR v2 is a self-supervised representation learning framework that combines phylogenetic ILR (isometric log-ratio) transforms with contrastive learning to produce robust embeddings from gut microbiome compositional data. The learned representations are evaluated on six disease classification tasks: type 2 diabetes (T2D), type 1 diabetes (T1D), inflammatory bowel disease (IBD), Crohn's disease (CD), ulcerative colitis (UC), and colorectal cancer (CRC), at both species- and genus-level taxonomic resolution.

## Key Features

- **Phylogenetic ILR transform**: Leverages the tree structure of microbial phylogeny to create interpretable balance coordinates
- **Aitchison augmentations**: Compositional data augmentations (perturbation, power, mixup) that respect the simplex geometry
- **Multi-view stacking**: Combines predictions from XGBoost, Random Forest, and neural network views via ElasticNet meta-learner
- **Domain adaptation**: DEBIAS-M, CORAL, and BatchNorm correction for cross-study generalization
- **Clinical metrics**: Sensitivity at 90% specificity, Youden's J, PPV/NPV at prevalence-adjusted thresholds

## Repository Structure

```
PhyloCompoCLR-v2/
├── src/                        # Python source code
│   ├── phylocompoclr_v2_train.py      # Pretraining pipeline
│   ├── phylocompoclr_v2_eval.py       # Evaluation pipeline
│   ├── aitchison_augmentations.py     # Compositional augmentations
│   ├── clinical_metrics.py            # Clinical metric computation
│   ├── cross_disease_interpret.py     # Cross-disease balance probing
│   ├── domain_adaptation.py           # DEBIAS-M, CORAL, BatchNorm
│   ├── global_delong_all.py           # Global DeLong across 72 comparisons
│   ├── multiview_stacking.py          # Multi-view stacking with ElasticNet
│   ├── unified_evaluation.py          # Unified evaluation across tasks
│   ├── run_delong_and_clinical.py     # Per-task DeLong + clinical metrics
│   ├── run_ablation*.py               # Ablation experiment runners
│   ├── run_species_eval*.py           # Species-level evaluation runners
│   └── ...                            # Additional run scripts
├── figures/
│   ├── main/                   # Main manuscript figures (png + svg)
│   ├── supplementary/          # Supplementary figures S1-S8 (png + svg)
│   └── other/                  # Additional analysis figures
├── manuscript/
│   ├── manuscript.tex          # LaTeX source
│   └── manuscript.pdf          # Compiled PDF
├── results/                    # Result JSON files (AUC, DeLong, clinical metrics)
└── README.md
```

## Dependencies

- Python 3.9+
- PyTorch
- scikit-learn
- xgboost
- scipy
- numpy
- pandas
- matplotlib / seaborn
- gneiss (for PhILR transform)
- biom-format
- statsmodels

## Usage

### 1. Pretraining

```bash
python src/phylocompoclr_v2_train.py \
    --level genus \
    --epochs 200 \
    --batch-size 128 \
    --lr 1e-3 \
    --augmentations perturbation,power,mixup
```

### 2. Fine-tuning and Baselines

```bash
python src/run_finetuning_and_baselines.py --level genus --task t2d
```

### 3. Unified Evaluation

```bash
python src/unified_evaluation.py --level genus
```

### 4. DeLong Tests and Clinical Metrics

```bash
python src/run_delong_and_clinical.py --level genus --task t2d
python src/global_delong_all.py
```

### 5. Multi-View Stacking

```bash
python src/multiview_stacking.py --level genus --task t2d
```

### 6. Domain Adaptation

```bash
python src/domain_adaptation.py --level genus --task t2d --method debias-m
```

## Results Summary

| Task | Level | Best Method | AUC |
|------|-------|-------------|-----|
| T2D  | genus | Stack_XGB_V2enh_RF | 0.791 |
| T2D  | species | Stack_all5 | 0.802 |
| T1D  | genus | Stack_all5 | 0.858 |
| T1D  | species | Stack_all5 | 0.885 |
| IBD  | genus | Stack_all5 | 0.947 |
| IBD  | species | Stack_all5 | 0.967 |
| CD   | genus | Stack_all5 | 0.948 |
| CD   | species | Stack_all5 | 0.967 |
| UC   | genus | Stack_all5 | 0.938 |
| UC   | species | Stack_all5 | 0.972 |
| CRC  | genus | Stack_XGB_V2enh_RF | 0.814 |
| CRC  | species | Stack_all5 | 0.831 |

## Citation

If you use this code, please cite:

```bibtex
@article{guzzi2025phylocompoclr,
  title={PhyloCompoCLR v2: Phylogeny-Aware Contrastive Learning for Multi-Disease Microbiome Classification},
  author={Guzzi, PietroHiram},
  year={2025}
}
```

## License

This project is licensed under the MIT License - see the LICENSE file for details.
