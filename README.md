# AXARA BioScience — Data Products

**Computational probe design intelligence for photoaffinity labelling in drug discovery.**

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.19919321.svg)](https://doi.org/10.5281/zenodo.19919321)

AXARA BioScience produces structural dataset products that tell medicinal chemists and chemical biologists exactly where to attach SDA photoaffinity probes on therapeutically relevant proteins — and what science those probes will reveal. Our datasets are built on published structural biology methods, validated against approved drug co-crystal structures, and delivered as analysis-ready reports with bench-ready experimental guidance.

---

## GLP-1 GPCR Covalent Tractability Atlas v1.0

### What This Product Is

The GLP-1 GPCR Covalent Tractability Atlas is a probe attachment site recommendation dataset for four metabolic disease GPCRs:

| Receptor | Disease Area | Key Drug | Structure Used |
|----------|-------------|----------|----------------|
| GLP-1R | Type 2 Diabetes / Obesity | Semaglutide, Liraglutide, Dulaglutide | PDB 6X1A — cryo-EM, ligand-bound |
| GIPR | Type 2 Diabetes / Obesity | Tirzepatide (dual GLP-1R/GIPR) | PDB 7DTY — cryo-EM, ligand-bound |
| GCGR | Type 2 Diabetes / NASH | Cotadutide (dual GLP-1R/GCGR) | PDB 8JIT — cryo-EM, cotadutide-bound |
| GLP-2R | Short Bowel Syndrome | Teduglutide | OpenFold3 prediction (Apache 2.0) |

For each receptor, the atlas identifies the highest-tractability residues for sulfo-SDA NHS ester and aryl-SDA probe attachment, filtered for transmembrane topology, disulfide bonds, and solvent accessibility. Every recommendation includes the scientific hypothesis the probe will test, the expected mass spectrometry result, and step-by-step experimental implementation guidance.

---

### The Science

#### Three-Component Scoring

Each residue is scored on three independent components:

**1. Binding Pocket Proximity**
fpocket Voronoi tessellation (Schmidtke & Barril, *J Med Chem* 2010) identifies druggable surface cavities. Proximity score is weighted by normalised druggability score per pocket.

**2. Solvent Accessibility**
Shrake-Rupley SASA (Shrake & Rupley, *J Mol Biol* 1973) calculated on the receptor chain of each mmCIF structure. Normalised to 150 Å².

**3. SDA Chemistry Preference**
Residue-type scores derived from published diazirine carbene insertion frequency data:

| Residue | Score | Basis |
|---------|-------|-------|
| CYS | 10.0 | S-H highest carbene reactivity (Brunner & Semenza 1983; Klinman 2017) |
| LYS | 6.0 | Primary NHS ester target (Qin et al. 2019) |
| TYR | 5.0 | O-H insertion >16% observed (Klinman et al. 2017) |
| HIS | 4.0 | N-H moderate, pH 6.5–7.4 optimal (Hacker et al. 2021) |
| SER | 2.5 | O-H NHS ester reactive (Qin et al. 2019) |
| THR | 2.0 | O-H lowest insertion 0.1% (Klinman et al. 2017) |

**References:** Klinman et al. *J Am Soc Mass Spectrom* 2017;28(8):1550; Qin et al. *Anal Chem* 2019;91(14):8909; Hacker et al. *JACS* 2021;143(47):19498.

#### Filters Applied

Four sequential filters are applied before any residue is scored:

1. **Transmembrane topology filter** — UniProt TM helix annotations exclude TM1-7 residues embedded in the lipid bilayer
2. **Disulfide filter** — CYS pairs with SG-SG distance < 2.5 Å excluded; structurally locked, thiol-inaccessible
3. **SASA filter** — residues with SASA < 1.0 Å² excluded as truly buried
4. **Pocket-proximal CYS bypass** — CYS with SASA < 5.0 Å² but fpocket proximity > 0 treated as partially accessible (acc = 0.15); corrects for apparent SASA reduction by co-crystallised ligands in holo structures

#### Scoring Parameter Justification

The scoring formula contains four parameters empirically calibrated against the 9/9 validation set: base weight (0.4), accessibility weight (0.6), proximity distance scale (8.0 Å), and CYS pocket proximity multiplier (3.0). Sensitivity analysis across ±20% parameter variation (81 combinations) confirms that top-ranked site identity is stable across the parameter space. Primary sites for GLP-1R (LYS202) and GIPR (TYR200) achieve 100% rank stability across all combinations.

---

### Engine Validation — 9/9 PASS

The AXARA Dataset Engine v4.0 was validated against 9 approved and clinical-stage covalent drug co-crystal structures spanning 5 protein families.

**Pass criterion:** Known covalent attachment site ranks in top 3 scored residues.

| Target | Family | Drug | Known Site | Engine Rank | Result |
|--------|--------|------|-----------|-------------|--------|
| KRAS G12C | GTPase | Sotorasib | CYS12 | 1 | ✓ PASS |
| BTK | Kinase | Ibrutinib | CYS481 | 1 | ✓ PASS |
| EGFR | Kinase | Osimertinib | CYS797 | 2 | ✓ PASS |
| MAOB | Oxidoreductase | Selegiline | CYS397 | 3 | ✓ PASS |
| EGFR | Kinase | Afatinib | CYS797 | 1 | ✓ PASS |
| BTK | Kinase | Zanubrutinib | CYS481 | 1 | ✓ PASS |
| EGFR T790M | Kinase | Neratinib | CYS797 | 2 | ✓ PASS |
| 3CLpro | Protease | Bofutrelvir | CYS145 | 3 | ✓ PASS |
| HER2 | RTK | Covalent inhibitor | CYS805 | 1 | ✓ PASS |

**Validation certificate** is included with every purchase.

#### Extrapolation Scope

The validation set consists of small-molecule covalent drugs (MW < 800 Da) targeting kinases, a GTPase, a protease, and an oxidoreductase. This atlas applies the engine to class B GPCRs binding large peptide ligands (MW 3–4 kDa). The structural logic transfers across protein families; quantitative score precision should not be assumed to transfer directly. All recommendations require experimental validation before synthesis commitment.

---

### Key Findings

**GLP-1R (PDB 6X1A)**
- Primary site: **LYS202** — Score 12.9 | SASA 105.2 | Pocket proximity 1.0 | Rank stability 100%
- Secondary site: **TYR220** — Score 11.5 | SASA 75.0 | Pocket proximity 1.0
- LYS202 sits at the orthosteric binding site entrance — directly relevant to semaglutide and liraglutide binding mechanism

**GIPR (PDB 7DTY)**
- Primary site: **TYR200** — Score 13.0 | SASA 173.0 | Pocket proximity 1.0 | Rank stability 100%
- Secondary site: **TYR36** — Score 10.8 | SASA 39.0 | Pocket proximity 1.0
- TYR200 is homologous in position to GLP-1R TYR220 — enables direct tirzepatide dual agonism structural comparison

**GCGR (PDB 8JIT — cotadutide-bound)**
- Primary site: **LYS381** — Score 11.0 | SASA 23.9 | Pocket proximity 1.0
- Secondary site: **CYS287** — Score 9.4 | SASA 69.2 | Free cysteine confirmed
- Scored from the cotadutide-bound activated structure — directly relevant to cotadutide mechanism of action

**GLP-2R (OpenFold3, Apache 2.0)**
- No experimental structure available; recommendations based on OpenFold3 prediction
- High-confidence ECD residues (pLDDT ≥ 70) scored; low-confidence regions (pLDDT < 50) excluded

---

### What the Atlas Delivers

Each atlas purchase includes five deliverables:

| Deliverable | Format | Purpose |
|-------------|--------|---------|
| Tractability Atlas | PDF | Primary report — probe recommendations, methodology, experimental protocol |
| Probe Reference Cards | PDF | One-page per receptor bench reference — print and use |
| Digital Twin | JSON | Machine-readable dataset — ingest into LIMS or computational pipelines |
| Scored PDB Files | PDB (×3) | B-factor replaced with tractability score — visualise in PyMOL or ChimeraX |
| Validation Certificate | PDF | 9/9 engine validation — for scientific review and procurement |

---

### Pricing

| Option | Price | Includes |
|--------|-------|---------|
| Full Atlas — all four receptors | **$12,000** | GLP-1R, GIPR, GCGR, GLP-2R + all five deliverables |
| Single receptor | **$3,500** | One receptor of choice + all five deliverables |

Licensed for use within a single organisation. Multi-site and CRO licensing available on request.

---

### Experimental Implementation

The atlas includes a full experimental protocol appendix covering:

- **Probe selection decision tree** — five steps from surface control to binding site probe
- **Recommended concentrations** — sulfo-SDA-NHS ester 10–100 µM; aryl-SDA 1–50 µM; maleimide-SDA 1–20 µM
- **UV crosslinking parameters** — 365 nm, 10 min on ice, 10 cm from source
- **Essential controls** — no-UV negative, competitor displacement (≥50% signal reduction at 100× competitor), surface probe positive, receptor-null background
- **Common failure modes** — diagnostics and remediation for each failure type

---

### Structures and Data Sources

All structures are sourced from publicly available repositories. No proprietary structural data is used.

- PDB 6X1A — Zhang et al. *Nature* 2020
- PDB 7DTY — Zhao et al. *Nat Commun* 2022
- PDB 8JIT — Li et al. *PNAS* 2023
- GLP-2R — OpenFold3 (OpenFold Consortium, Apache 2.0); UniProt O95838

---

### Contact and Licensing

**AXARA BioScience**
axara@axara.bio
axara.bio

To purchase, request a sample report, or discuss custom receptor panels, contact us at axara@axara.bio.

---

*AXARA BioScience Dataset Engine v4.0 | April 2026 | Confidential — Licensed Use Only*
*All recommendations require experimental validation before synthesis commitment.*
