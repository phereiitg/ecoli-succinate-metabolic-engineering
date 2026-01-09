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

### 3.3 Phase C: Combinatorial Optimization (Greedy Forward Selection)

Instead of a simple single-reaction scan, we implemented a **Greedy Forward Selection Algorithm** to identify synergistic gene combinations.

1.  **Preparation:** Standard fermentation byproducts (Lactate, Acetate, Ethanol) were computationally "knocked out" to prevent carbon waste.
2.  **Round 1:** The algorithm scanned all metabolic reactions to find the single best overexpression target. The winner was permanently locked "ON".
3.  **Round 2:** The algorithm scanned all remaining reactions to find a *second* target that works synergistically with the first.
4.  **Capping:** The search was capped at **2 genes** to ensure experimental feasibility.

## 4. Results Summary

The greedy selection algorithm revealed that a **multi-gene strategy** significantly outperforms single-gene targets.

### 4.1 Stepwise Engineering Progression

| Step | Strategy Description | Key Target(s) | Succinate Yield (mmol/gDW/h) | Improvement |
| :--- | :--- | :--- | :--- | :--- |
| **0** | **Waste Knockouts** | $\Delta$ LDH, $\Delta$ PTA, $\Delta$ ACK, $\Delta$ ADHE | ~0.0 - 0.1 | Baseline (Eliminates byproducts) |
| **1** | **Round 1 Winner** | **+ MALS** (Malate Synthase) | **1.10** | **10x** vs Baseline |
| **2** | **Round 2 Winner** | **+ MALS + PPC** (PEP Carboxylase) | **3.22** | **~3x** vs MALS alone |

### 4.2 Biological Interpretation of the Winners

* **Knockouts:** Block the "waste" exits (Lactate/Acetate), forcing carbon to stay in the central metabolism.
* **MALS (`aceB`):** Activates the **Glyoxylate Shunt**, creating a shortcut in the TCA cycle that prevents carbon from being lost as CO₂.
* **PPC (`ppc`):** Increases **Anaplerosis**, acting as a "pump" that feeds fresh carbon (Oxaloacetate) into the cycle to sustain the high flux demanded by MALS.

**Key Result:** The synergistic combination of **MALS + PPC** triples the production yield compared to MALS alone, reaching a theoretical yield of **3.22 mmol/gDW/h**.

## 5. Mechanistic Interpretation

1.  **The Bottleneck:** In standard conditions, carbon entering the TCA cycle is burned to CO₂ for energy.
2.  **The Solution (MALS):** Overexpressing Malate Synthase bypasses the CO₂-generating steps, conserving carbon atoms.
3.  **The Fuel (PPC):** MALS drains the cycle of intermediates. Overexpressing PPC replenishes the cycle by converting PEP directly to Oxaloacetate.
4.  **Synergy:** PPC shovels carbon *in*, and MALS efficiently routes it *out* as succinate.

## 6. Final Conclusion

1.  Single-target engineering is insufficient for maximal yields.
2.  The optimal 2-gene strategy is the simultaneous overexpression of **aceB (MALS)** and **ppc (PPC)**.
3.  This combinatorial approach achieves a **~290% increase** in succinate production over the single-gene strategy.
4.  The strain maintains a healthy growth rate (~0.26 $h^{-1}$), making it a viable candidate for industrial fermentation.