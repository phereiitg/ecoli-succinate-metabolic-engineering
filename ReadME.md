# Computational Metabolic Engineering of *E. coli* for Succinic Acid Production

## 1. Project Overview

This project applies genome-scale metabolic modeling to identify genetic engineering targets that enhance succinic acid production in *Escherichia coli*. While wild-type *E. coli* prioritizes biomass formation, this work computationally rewires metabolism to favor industrial chemical secretion by integrating constraint-based modeling with transcriptomic data.

## 2. Scientific Motivation

1. Succinic acid is a high-value platform chemical used in bioplastics, solvents, and pharmaceuticals.
2. Wild-type *E. coli* is evolutionarily optimized for growth, not metabolite secretion.
3. Rational metabolic engineering is required to redirect carbon flux from biomass to product formation.

## 3. Methodology Overview

The workflow is implemented in Python using COBRApy and follows three sequential phases.

### 3.1 Phase A: Model Selection and Baseline Simulation

1. Model selected: **iML1515** (*E. coli* K-12 MG1655; >2,700 reactions)
2. Environmental condition: **Microaerobic** (industry-relevant for succinate production)
3. Optimization objective: **Maximize biomass**
4. Observation: Reduced growth under low oxygen with **zero succinate secretion**, establishing the control case

### 3.2 Phase B: Transcriptomic Data Integration

1. Dataset used: **NCBI GEO GSE189154** (Microaerobic vs. Aerobic)
2. Gene expression fold-changes mapped to metabolic reactions
3. E-Flux–like constraints applied to reactions linked to downregulated genes
4. Outcome: Model reflects realistic physiological limitations under low oxygen

### 3.3 Phase C: In Silico Overexpression Scan

1. Automated overexpression simulated for each metabolic reaction
2. High flux enforced through individual reactions
3. Resulting phenotypes evaluated based on:

   * Viable growth
   * Succinic acid secretion
4. Top-performing reactions shortlisted as engineering targets

## 4. Key Findings and Quantitative Results

### 4.1 Target 1: Phosphoenolpyruvate Carboxylase (PPC)

1. Gene: **ppc**
2. Growth rate: **0.35 1/h**
3. Succinate yield: **0.12 mmol/gDW/h**
4. Interpretation: Growth maintained, but limited product formation

### 4.2 Target 2: Malate Synthase (MALS) — *Primary Recommendation*

1. Genes: **aceB**, **glcB**
2. Growth rate: **0.31 1/h**
3. Succinate yield: **1.10 mmol/gDW/h**
4. Interpretation: Strong product formation with minimal growth penalty

## 5. Mechanistic Interpretation

1. In wild-type cells, carbon flux proceeds through the full TCA cycle
2. The TCA cycle releases carbon as CO₂, limiting product yield
3. Overexpression of Malate Synthase activates the **Glyoxylate Shunt**
4. This bypass conserves carbon and generates excess C4 intermediates
5. Excess intermediates are secreted as succinic acid

## 6. Final Conclusion

1. Overexpression of **aceB / glcB** is identified as the optimal strategy
2. Succinic acid secretion increases by approximately **9-fold**
3. Growth rate remains at ~**88%** of the wild-type strain
4. The glyoxylate shunt is confirmed as a key metabolic lever for succinate production


