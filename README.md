# MGCL-DTI
A Multi-View Graph Contrastive Learning Framework for Drug-Target Interaction Prediction

## Environment

Recommended environment:

- Python 3.9
- PyTorch 1.11.0
- DGL 0.8.0
- NumPy
- SciPy
- scikit-learn

A typical installation is:

```bash
pip install torch==1.11.0 dgl==0.8.0 numpy scipy scikit-learn
```

If GPU training is used, please install the PyTorch and DGL versions matching your CUDA environment.

---

## Dataset

The current implementation uses the **Luo heterogeneous biological network benchmark** for DTI prediction.

### Node types

- **Drugs**: 708
- **Proteins**: 1512
- **Diseases**: 5603
- **Side effects**: 4192

### Required relation files

Place the following files in the data directory:

- `mat_drug_protein.txt`
- `mat_drug_drug.txt`
- `mat_drug_disease.txt`
- `mat_drug_se.txt`
- `mat_protein_protein.txt`
- `mat_protein_disease.txt`
- `Similarity_Matrix_Drugs.txt`
- `Similarity_Matrix_Proteins.txt`

### Required feature files

Place the following feature files in `data/feature/`:

- `drug_feature.txt`
- `protein_feature.txt`
- `disease_feature.txt`
- `sideeffect_feature.txt`

### File description

- `mat_drug_protein.txt`: known drug-protein interaction matrix
- `mat_drug_drug.txt`: drug-drug association matrix
- `mat_drug_disease.txt`: drug-disease association matrix
- `mat_drug_se.txt`: drug-side-effect association matrix
- `mat_protein_protein.txt`: protein-protein association matrix
- `mat_protein_disease.txt`: protein-disease association matrix
- `Similarity_Matrix_Drugs.txt`: drug chemical similarity matrix
- `Similarity_Matrix_Proteins.txt`: protein sequence similarity matrix
- `drug_feature.txt`, `protein_feature.txt`, `disease_feature.txt`, `sideeffect_feature.txt`: node feature matrices used for initialization

> Important  
> The current code automatically searches for benchmark data and feature files under the project directory. Keep file names consistent with the names above.

---

## Implemented Settings

This repository currently supports:

- **Benchmark evaluation** (`--task benchmark`)
- **Standard edge split** (`--cold_start none`)
- **Drug cold-start** (`--cold_start drug`)
- **Protein cold-start** (`--cold_start protein`)
- **Pair cold-start** (`--cold_start pair`)
- **Similarity-filtered negative sampling**
- **Meta-graph structural view on/off ablation**
- **Representation fusion on/off ablation**
- **Three fusion strategies**: `gate`, `sum`, `cat`

The current implementation is centered on the **MG view**. The previous MP-based version is **not used** in this repository.

---

## Quick Start

Run training from `application`:

```bash
cd application
python main.py
```

### Basic settings

- `--device`  
  Device used for training, e.g. `cuda:0` or `cpu`.

- `--task`  
  Current supported task: `benchmark`.

- `--n_splits`  
  Number of folds for cross-validation. Default: `10`.

### Evaluation protocol

- `--cold_start`  
  Evaluation scenario:
  - `none`: standard edge split
  - `drug`: unseen-drug cold start
  - `protein`: unseen-protein cold start
  - `pair`: unseen drug-protein pair cold start
