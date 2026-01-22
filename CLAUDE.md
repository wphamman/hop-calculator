# Hop Value Calculator: Project Brief & Logic Summary

## 1. Project Overview

The Hop Value Calculator is a sales and brewing utility designed to demonstrate the financial and efficiency benefits of "Advanced Hop Products" (e.g., FLEX®, INCOGNITO®, LupoMAX®) compared to standard T90 pellets.

While advanced products often have a higher upfront price per kg, they typically offer higher alpha acid concentration and zero-to-low wort loss (vegetal matter absorption). This calculator quantifies those "hidden" savings—specifically beer yield improvement—to calculate the true Net Benefit per Batch.

## 2. Core Features

- **Dual Scenario Comparison:**
  - Scenario A (Baseline): A standard recipe using only T90 pellets.
  - Scenario B (Advanced): A modified recipe using a mix of advanced products (or T90s) to achieve specific brewing goals (Bittering, Whirlpool, Dry Hop).

- **Multi-Step Recipe Builder:** Allows brewers to input specific addition times (e.g., 60 min, 0 min, Dry Hop) rather than a single global dosage.

- **Scale-Aware Logic:** Automatically detects if the user is a Homebrewer or Commercial Brewer based on batch size and adjusts utilization assumptions accordingly.

- **Financial Analysis:** Breaks down direct hop costs vs. revenue lost to wort absorption.

## 3. Mathematical Logic

### A. IBU Calculation (Metric)

The calculator uses a standard metric estimation for International Bitterness Units, derived from the mass of iso-alpha acids in solution.

```
IBU = (Dosage (kg) × Alpha Acid (%) × Utilization (%) × 100) / Batch Volume (hL)
```

**Derivation:**
- 1 kg = 1,000,000 mg
- Alpha Mass (mg) = kg × 1,000,000 × (Alpha / 100)
- Iso-Alpha Mass (mg) = Alpha Mass × (Utilization / 100)
- IBU = Iso-Alpha Mass (mg) / Volume (Liters)

Note: The formula simplifies to the one above when using hL (1 hL = 100 L).

### B. Financial Calculations

- **Direct Cost:** Dosage (kg) × Price per kg
- **Wort/Beer Loss:** Dosage (kg) × Absorption Rate (L/kg)
  - Default T90 Absorption: 14 L/kg (Hot Side) or 20 L/kg (Dry Hop)
  - Advanced Products: Often 0 L/kg (Extracts) or reduced (Enriched Pellets)
- **Revenue Lost:** Volume Lost (L) × Revenue per L
- **Net Benefit:** (Total Cost A + Revenue Lost A) - (Total Cost B + Revenue Lost B)

## 4. Smart Defaults ("Scale Aware" Logic)

To ensure accuracy for both homebrewers and production facilities, the calculator adjusts the default Utilization Rates based on the entered Batch Size.

**Threshold:** 5 hL (approx. 4.2 BBL)

| Addition Time | Homebrew (< 5 hL) | Commercial (>= 5 hL) | Source / Reasoning |
|---------------|-------------------|----------------------|-------------------|
| 60+ min | 30% | 44% | Based on Ballast Point study (Justus, 2018) on 50 BBL system |
| 30-59 min | 20% | 35% | Interpolated efficiency curve |
| 10-29 min | 15% | 30% | Commercial whirlpools maintain isomerization temps longer |
| 0-9 min / WP | 5% | 30% | Commercial systems see high utilization during long knockout/whirlpool stands |

## 5. Product Database

The calculator includes built-in profiles for common hop products, defining their default specifications and "Best Use" cases.

- **T90 Pellets:** Standard baseline. High wort loss (14-20 L/kg).
- **FLEX®:** Bittering extract. 65% Alpha. 0 L/kg loss. Fixed high utilization.
- **INCOGNITO®:** Whirlpool flowable. 45% Alpha. 0 L/kg loss.
- **LupoMAX®:** Enriched pellet. ~20% Alpha. Reduced loss (7 L/kg). 1.1x Utilization factor.
- **SPECTRUM / Quantum:** Liquid dry hop products. 0 L/kg loss.
- **Redihop®:** Light-stable post-fermentation bittering (Rho-iso-alpha). 0.7x bitterness perception.
- **Aromahop OE:** CO2 extract for aroma.

## 7. TDS-Validated Product Specifications

Product specifications validated against manufacturer Technical Data Sheets (January 2026):

### Bittering Products

| Product | Alpha % | Wort Loss | Utilization | Source |
|---------|---------|-----------|-------------|--------|
| **T90 Pellets** | 4-15% (variety) | 14 L/kg | Tinseth formula | Industry standard |
| **FLEX®** | 65% ±5% | 0 L/kg | 25-35% (calc uses 30%) | BarthHaas TDS |
| **CO₂ Extract** | 35-50% (variety) | 0 L/kg | 32-38% | BarthHaas TDS |

### Whirlpool/Aroma Products

| Product | Alpha % | Wort Loss | Notes | Source |
|---------|---------|-----------|-------|--------|
| **INCOGNITO®** | 35-55% | 0 L/kg | Raw α-acids, whirlpool use | BarthHaas TDS |
| **Cryo Hops®** | 5-30% (variety) | ~3 L/kg* | 50% dose of T90, concentrated lupulin | YCH TDS |
| **LupoMAX®** | ~20% | 7 L/kg | 1.1x utilization factor | BarthHaas |
| **LupoCore™** | Variety dependent | ~10 L/kg* | Refined pellet, less vegetal matter | John I. Haas TDS |

### Rho-Isomerized Products

| Product | Concentration | Wort Loss | Utilization | Bitterness Factor | Source |
|---------|---------------|-----------|-------------|-------------------|--------|
| **Redihop®** | 30% ±0.5% rho-iso-α | 0 L/kg | Kettle: 45%, Post-ferm: 70-75% | 0.7× | BarthHaas TDS |

### Aroma-Only Products (No IBU)

| Product | Alpha % | Wort Loss | Use Case | Source |
|---------|---------|-----------|----------|--------|
| **Spectrum** | 0% (oil-based) | 0 L/kg | Dry hop alternative, 1:5-1:8 vs pellets | BarthHaas TDS |
| **Aromahop® OE** | <0.2% | 0 L/kg | Hop aroma, 1kg = 15kg pellets (oil equiv) | BarthHaas TDS |
| **Quantum** | 0% | 0 L/kg | Brite tank aroma, 0.065-0.5 mL/L (0.3-2 oz/bbl) | Abstrax TDS |
| **Skyfarm** | 0% | 0 L/kg | Fruit flavors (terpene-based) | Abstrax TDS |

*\* Estimated values - not explicitly stated in TDS*

### Validation Notes

- **FLEX®**: Calculator correctly implements as pre-isomerized with 30% utilization (TDS: 25-35%)
- **Redihop®**: All values match TDS exactly (30% conc, 45%/73% util, 0.7× bitterness)
- **Cryo Hops®**: Alpha varies widely by variety; 3 L/kg wort loss is estimated
- **Quantum/Skyfarm**: These are Abstrax products - Quantum for hop aroma, Skyfarm for fruit flavors

## 6. UX Enhancements

- **Dynamic Reset:** Clearing data resets to smart defaults based on the currently active scale.
- **Mixed Additions:** Scenario B allows mixing T90s with Advanced products to model "Hybrid" hopping schedules.
- **AI Integration Stub:** A "✨ Ask AI" feature that generates a prompt for LLMs to analyze the current recipe for cost savings.
