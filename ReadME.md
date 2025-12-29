# Computational Metabolic Engineering of *E. coli* for Succinic Acid Production

## 1. Project Overview

This project applies genome-scale metabolic modeling to identify genetic engineering targets that enhance succinic acid production in *Escherichia coli*. While wild-type *E. coli* prioritizes biomass formation, this work computationally rewires metabolism to favor industrial chemical secretion by integrating constraint-based modeling with transcriptomic data.

## 2. Scientific Motivation

1. Succinic acid is a high-value platform chemical used in bioplastics, solvents, and pharmaceuticals.
2. Wild-type *E. coli* is evolutionarily optimized for growth, not metabolite secretion.
3. Rational metabolic engineering is required to redirect carbon flux from biomass to product formation.

## 3. Methodology Overview

The workflow is implemented in Python using COBRApy and follows three sequential phases.

### 3.1 Phase A: Baseline (Control) Simulation

| Step | Action                                  |
| ---- | --------------------------------------- |
| 1    | Selected iML1515 genome-scale model     |
| 2    | Applied microaerobic oxygen constraints |
| 3    | Optimized for biomass growth            |
| 4    | Measured growth and succinate secretion |

**Observation:** Growth decreases under low oxygen, but succinate secretion remains **zero**, confirming the need for engineering.

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

## 4. Results Summary

The in silico overexpression scan revealed that only a small subset of reactions can successfully redirect metabolic flux toward succinate production **while maintaining viable growth**. Two key enzymes emerged as effective intervention points.

### 4.1 Identified Engineering Targets

| Target Enzyme | Gene(s)    | Growth Rate (1/h) | Succinate Yield (mmol/gDW/h) | Biological Interpretation                                                                            |
| ------------- | ---------- | ----------------- | ---------------------------- | ---------------------------------------------------------------------------------------------------- |
| PPC           | ppc        | 0.35              | 0.12                         | Increases anaplerotic flux into oxaloacetate, but most carbon is still consumed for growth           |
| **MALS** ⭐    | aceB, glcB | 0.31              | **1.10**                     | Activates the glyoxylate shunt, conserving carbon and forcing excess C4 intermediates to be secreted |

**Key Result:** Malate Synthase overexpression leads to ~9× higher succinate secretion compared to PPC, with only a minor reduction in growth rate.


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
