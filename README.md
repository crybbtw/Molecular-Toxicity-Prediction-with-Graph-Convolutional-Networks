# Molecular-Toxicity-Prediction-with-Graph-Convolutional-Networks

A computational biology project that applies **Graph Convolutional Networks (GCNs)** to predict molecular toxicity using the **Tox21 dataset**. The project explores how molecular structures can be represented as graphs and processed with graph neural networks for toxicity screening in drug discovery.

## Overview

Early-stage toxicity screening is an important step in drug discovery, as potentially toxic compounds need to be identified before progressing to more expensive experimental studies.

In this project, molecules are represented as **molecular graphs**, where:

* **Nodes** represent atoms and their chemical features
* **Edges** represent chemical bonds
* A **GraphConvModel** learns molecular representations through message passing
* The model predicts toxicity across **12 biological assay targets** from the Tox21 dataset

The project uses **DeepChem** to build and train the Graph Convolutional Network.

## Objectives

* Understand how molecular structures can be represented as graphs
* Implement a Graph Convolutional Network for molecular property prediction
* Predict toxicity across multiple biological pathways
* Evaluate model performance using ROC-AUC
* Investigate model predictions on new molecules
* Analyze limitations such as class imbalance, overfitting, and the lack of 3D structural information
* Compare GraphConv with attention-based molecular representation methods

## Dataset

The project uses the **Tox21 dataset** from DeepChem's MoleculeNet collection.

* **7,831 molecules**
* **12 toxicity-related assay targets**
* Molecular structures represented using **SMILES**
* Missing labels are handled using sample weights
* Dataset split using **scaffold splitting**

Scaffold splitting separates molecules according to their core chemical structures, providing a more challenging evaluation of the model's ability to generalize to structurally different molecules.

## Methodology

### 1. Molecular Graph Representation

SMILES strings are converted into molecular graphs using DeepChem's `GraphConv` featurizer.

For example:

```text
SMILES: CCO

      C ─ C ─ O
      ↓   ↓   ↓
    atom features
```

Each atom is represented using a feature vector containing chemical information such as atom type, valence, charge, aromaticity, and hybridization.

### 2. Graph Convolutional Network

The model architecture consists of:

```text
Molecular Graph
      ↓
GraphConv Layer 1
      ↓
GraphPool Layer
      ↓
GraphConv Layer 2
      ↓
GraphGather
      ↓
Dense Layer (128)
      ↓
Output Layer (12 tasks)
      ↓
Toxicity Predictions
```

The GraphConv layers use **message passing**, allowing each atom to update its representation based on information from neighboring atoms.

### 3. Training

The model is trained as a **multi-task classification model**.

Key configuration:

* Optimizer: Adam
* Learning rate: `0.001`
* Dropout: `0.2`
* Training epochs: `30`
* Tasks: `12`
* Data split: Scaffold split
* Evaluation metric: ROC-AUC

## Evaluation

Model performance is evaluated using **ROC-AUC**, both across the complete dataset and individually for each toxicity assay.

The project achieved approximately:

| Dataset    | ROC-AUC |
| ---------- | ------: |
| Train      |   ~0.90 |
| Validation |   ~0.70 |
| Test       |   ~0.70 |

The results show a noticeable gap between training and test performance, suggesting **overfitting** and limited generalization to unseen molecular scaffolds.

The notebook also generates:

* Class distribution visualization
* ROC-AUC score for each toxicity task
* ROC curves for selected tasks
* Toxicity probability heatmap

## Molecular Prediction

The trained model is also used to generate toxicity predictions for new molecules, including:

* **Aspirin**
* **Caffeine**
* **Pyrene**

The predictions are visualized as a heatmap showing the estimated toxicity probability across the 12 Tox21 tasks.

> These predictions are model outputs and should not be interpreted as clinical or experimental toxicity assessments.

## Key Findings

### Model Performance

The GraphConv model was able to learn useful representations of molecular structures and achieved approximately **0.70 mean ROC-AUC on the test set**.

However, the difference between training and test performance indicates that the model does not generalize perfectly to structurally different molecules.

### Class Imbalance

The Tox21 dataset is highly imbalanced for several toxicity endpoints, with substantially fewer positive toxic samples than non-toxic samples.

This can make it difficult for the model to correctly identify rare toxic compounds.

### Limitations

Several limitations were identified:

1. **No 3D structural information**
   The model operates on 2D molecular topology and does not explicitly capture molecular geometry.

2. **Limited receptive field**
   With two GraphConv layers, information is primarily propagated across relatively local molecular neighborhoods.

3. **Class imbalance**
   Rare toxicity labels can make prediction more difficult.

4. **Overfitting**
   The training ROC-AUC is considerably higher than the test ROC-AUC.

5. **Novel chemical scaffolds**
   Performance may decrease when predicting molecules that are structurally different from those seen during training.

6. **Metabolism is not modeled**
   The model does not account for toxicity caused by metabolites produced after a compound is processed biologically.

## Related Research

The project also reviews:

> Xiong et al. (2020). *Pushing the Boundaries of Molecular Representation for Drug Discovery with the Graph Attention Mechanism*. Journal of Medicinal Chemistry.

The paper introduces **Attentive FP**, an attention-based graph neural network that learns which atoms and molecular components are more informative for molecular property prediction.

This provides a useful comparison to the GraphConv approach implemented in this project.

## Possible Improvements

Future iterations could explore:

* Attention-based architectures such as **Attentive FP**
* 3D molecular conformer features
* Explicit bond/edge features
* Class-weighted loss functions
* Early stopping
* Ensemble models
* Transfer learning from larger chemical datasets
* Graph Transformer architectures

## Technologies

* **Python**
* **DeepChem**
* **PyTorch**
* **DGL**
* **RDKit / molecular graph featurization**
* **NumPy**
* **Pandas**
* **Scikit-learn**
* **Matplotlib**
* **Seaborn**
* **Jupyter Notebook**

## Project Structure

```text
.
├── GCN_drug_discovery_tox21.ipynb
├── class_distribution.png
├── roc_auc_per_task.png
├── roc_curves.png
├── toxicity_heatmap.png
└── README.md
```

## What I Learned

Through this project, I explored how **graph neural networks can be applied to computational drug discovery**, particularly for molecular toxicity prediction.

The project provided hands-on experience with:

* Molecular graph representation
* Graph convolution and message passing
* Multi-task learning
* Scaffold-based dataset splitting
* ROC-AUC evaluation
* Molecular toxicity prediction
* Model interpretation and critical analysis
* Reading and connecting research literature with an implemented model

---

**Course:** Computational Biology
**Project:** Molecular Toxicity Prediction for Drug Discovery
**Model:** Graph Convolutional Network (GraphConv)
**Dataset:** Tox21
