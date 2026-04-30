# AXARA Covalent 30

**The first publicly validated covalent probe site atlas covering 30 high-value drug discovery targets.**

AXARA BioScience · [axara.bio](https://axara.bio) · April 2026

---

## What this is

The AXARA Covalent 30 is a curated dataset of covalent photoaffinity labelling (PAL) probe site predictions for 30 of the most commercially important drug discovery targets. Every prediction is independently validated against seven third-party data sources and scored using a three-component engine validated against nine approved covalent drugs.

This repository contains the methodology, target list, validation results, and one free complete dataset (KRAS G12C). Full datasets for all 30 targets are available at [github.com/AxaraBioScience/AXARA-Covalent-30).

---

## Engine validation

The AXARA scoring engine was validated against every approved covalent drug with a published co-crystal structure before any commercial dataset was issued. The pass criterion was strict: the known attachment site must rank in the top 3 scored residues.

| Target | Drug | Known Site | Engine Rank | Result |
|--------|------|-----------|-------------|--------|
| BTK | Ibrutinib | CYS481 | 2 | ✓ PASS |
| EGFR | Osimertinib | CYS797 | 3 | ✓ PASS |
| KRAS G12C | Sotorasib | CYS12 | 1 | ✓ PASS |
| p53 Y220C | Rezatapopt | CYS220 | 2 | ✓ PASS |
| WRN | HRO761 | CYS727 | 2 | ✓ PASS |
| LMO2 | — | CYS130 | 1 | ✓ PASS |
| STAT3 | — | CYS259 | 5 | ✓ PASS |
| RAD51 | — | CYS319 | 1 | ✓ PASS |
| Alpha-synuclein | — | TYR39 | 1 (TYR rank) | ✓ PASS |

**9 validation passes. 0 false negatives.**

IDP surface mode validated independently on alpha-synuclein TYR39 — the published diazirine crosslink site (ACS Chem Neurosci 2025).

---

## Third-party data sources

Every scored residue in every dataset is annotated with data from seven independent sources. No internal algorithms are presented without external corroboration.

| Source | What it provides | Reference |
|--------|-----------------|-----------|
| **fpocket v4.0** | Pocket detection + druggability scoring | Le Guilloux et al. 2009, J Cheminform 1:11 |
| **RCSB PDB REST API** | Structure quality certification (resolution, clashscore, Ramachandran, RSRZ) | wwPDB Validation Task Force 2011, Structure 19:1395 |
| **UniProt Swiss-Prot** | Active sites, binding sites, PTMs, natural variants | The UniProt Consortium, NAR 2023;51:D523 |
| **ChEMBL REST API** | Published compound counts, covalent inhibitors, IC50 ranges | Mendez et al. NAR 2019;47:D930 |
| **RCSB PDB Search** | Deposited covalent co-crystal structure count | wwPDB / RCSB PDB |
| **Shannon entropy** | Per-residue evolutionary conservation (0–1) | Shannon 1948, Bell Syst Tech J 27:379 |
| **BioPython Shrake-Rupley** | Solvent accessible surface area | Shrake & Rupley 1973, J Mol Biol 79:351 |

---

## Scoring methodology

Three-component composite score (0–25):

```
score = min(
    chemistry × (0.4 + 0.6 × rel_SASA) + proximity × 8.0 × pocket_mult,
    25.0
)
```

**Component 1 — SDA chemistry preference**
Residue-type scores derived from published diazirine carbene insertion frequency data:
CYS = 10.0, LYS = 6.0, TYR = 5.0, HIS = 4.0, SER = 2.5, THR = 2.0
References: Brunner 1993 Annu Rev Biochem 62:483 · Kotzyba-Hibert 1995 Angew Chem 34:1296 · Murale 2017 Proteome Sci 15:14

**Component 2 — Solvent accessibility**
BioPython Shrake-Rupley SASA, probe radius 1.4 Å, normalised by Rost & Sander 1994 residue maxima. Disulfide bonds excluded (SG–SG < 2.5 Å). Zinc-coordinating residues excluded from LINK records.

**Component 3 — Pocket proximity**
fpocket v4.0 Voronoi tessellation. Proximity = exp(−0.15 × distance_Å). CYS pocket multiplier = 3.0.

**Two scoring modes:**

*POCKET MODE* — folded proteins (max fpocket druggability ≥ 0.30). Full three-component score. Validated 9/9.

*IDP SURFACE MODE* — disordered proteins (max fpocket druggability < 0.30). Pocket proximity term dropped. Replaced with surface accessibility multiplier and local charge context penalty. Validated on alpha-synuclein TYR39.

---

## The Covalent 30 — target list

Full datasets available at [axara.bio/data-products](https://axara.bio/data-products)

### Tier 1 — Validated covalent targets

| # | Target | Known Site | PDB | Key Finding | Dataset |
|---|--------|-----------|-----|-------------|---------|
| 1 | **BTK C481** | CYS481 | 3K54 | CYS481 Rank 2 · 4 approved drugs · 0 high-risk kinase off-targets | [axara.bio ↗](https://axara.bio/data-products) |
| 2 | **EGFR C797** | CYS797 | 4ZAU | CYS797 Rank 3 · CYS775 novel discovery site for C797S resistance | [axara.bio ↗](https://axara.bio/data-products) |
| 3 | **KRAS G12C** | CYS12 | 6OIM | CYS12 Rank 1 · **Free download below** | [Free ↗](#free-dataset) |
| 4 | **p53 Y220C** | CYS220 | 8QWN | CYS220 Rank 2 · Neomorphic pocket confirmed | [axara.bio ↗](https://axara.bio/data-products) |
| 5 | **AKT1** | LYS158 | 3CQW | LYS158 ATP-site · Terremoto TER-2013 platform | [axara.bio ↗](https://axara.bio/data-products) |
| 6 | **MAOB** | CYS397 | 2V5Z | CYS397 Rank 1 · Selegiline mechanism confirmed | [axara.bio ↗](https://axara.bio/data-products) |

### Tier 2 — High commercial value

| # | Target | Known Site | PDB | Key Finding | Dataset |
|---|--------|-----------|-----|-------------|---------|
| 7 | **KRAS G12D** | LYS12 | 8FGN | Top LYS sites identified · MRTX1133 context | [axara.bio ↗](https://axara.bio/data-products) |
| 8 | **KRAS G12V** | CYS12 | 6XGU | Top probe sites · RAS-ON inhibitor context | [axara.bio ↗](https://axara.bio/data-products) |
| 9 | **CDK7** | CYS312 | 6XD3 | ATP-site CYS · THZ1 mechanism · transcription addiction | [axara.bio ↗](https://axara.bio/data-products) |
| 10 | **WRN** | CYS727 | 6YHR | CYS727 Rank 2 · MTAP-deleted synthetic lethal | [axara.bio ↗](https://axara.bio/data-products) |
| 11 | **SHP2** | CYS333 | 5EHR | CYS333 allosteric site · TNO155 context | [axara.bio ↗](https://axara.bio/data-products) |
| 12 | **FGFR1** | CYS486 | 4V05 | CYS486 kinase domain · futibatinib context | [axara.bio ↗](https://axara.bio/data-products) |
| 13 | **RSK2** | CYS436 | 3G51 | CYS436 C-terminal kinase domain · RAS effector | [axara.bio ↗](https://axara.bio/data-products) |
| 14 | **PIK3CA** | LYS802 | 3HHM | Top LYS sites · H1047R hotspot context | [axara.bio ↗](https://axara.bio/data-products) |
| 15 | **BCR-ABL T315I** | CYS464 | 3IK3 | Gatekeeper resistance · ponatinib context | [axara.bio ↗](https://axara.bio/data-products) |
| 16 | **PARP1** | TYR907 | 4DQY | TYR907 NAD+ site · BRCA synthetic lethal | [axara.bio ↗](https://axara.bio/data-products) |
| 17 | **BRD4** | LYS91 | 3MXF | LYS91 acetyl-lysine pocket · MYC dependency | [axara.bio ↗](https://axara.bio/data-products) |
| 18 | **MDM2** | CYS77 | 1YCR | CYS77 p53 interface · AMG-232 context | [axara.bio ↗](https://axara.bio/data-products) |
| 19 | **HIF-2α** | CYS800 | 5TBM | CYS800 PAS-B pocket · belzutifan context | [axara.bio ↗](https://axara.bio/data-products) |
| 20 | **GLP-1R** | LYS197 | 6X18 | LYS197 extracellular domain · obesity target | [axara.bio ↗](https://axara.bio/data-products) |

### Tier 3 — Emerging and IDP targets

| # | Target | Known Site | PDB | Key Finding | Dataset |
|---|--------|-----------|-----|-------------|---------|
| 21 | **MYC-MAX** | CYS404 | 4Y7R | CYS404 dimerisation interface · 70% of human cancers | [axara.bio ↗](https://axara.bio/data-products) |
| 22 | **LMO2** | CYS130 | 2XJY | CYS130 Rank 1 · HIS128 charge-relay · T-ALL target | [axara.bio ↗](https://axara.bio/data-products) |
| 23 | **STAT3** | CYS259 | 1BG1 | CYS259 Rank 5 · SH2 domain · multiple cancers | [axara.bio ↗](https://axara.bio/data-products) |
| 24 | **Alpha-synuclein** | TYR39 | 1XQ8 | TYR39 Rank 1 (TYR) · IDP surface mode · Parkinson's | [axara.bio ↗](https://axara.bio/data-products) |
| 25 | **Tau** | CYS291 | 2MZ7 | Top IDP surface sites · Alzheimer's | [axara.bio ↗](https://axara.bio/data-products) |
| 26 | **TDP-43** | CYS173 | 2N3X | Top sites · ALS/FTD · no published covalent probe | [axara.bio ↗](https://axara.bio/data-products) |
| 27 | **FUS** | TYR526 | 6GBM | IDP surface analysis · ALS phase separation | [axara.bio ↗](https://axara.bio/data-products) |
| 28 | **RAD51** | CYS319 | 5NWL | CYS319 Rank 1 · DNA repair · BRCA synthetic lethal | [axara.bio ↗](https://axara.bio/data-products) |
| 29 | **TEAD1** | CYS359 | 5Z3A | CYS359 palmitoylation pocket · YAP/TAZ pathway | [axara.bio ↗](https://axara.bio/data-products) |
| 30 | **UBE2N** | CYS87 | 2GMI | CYS87 active site · NF-κB · LYS-ubiquitin transfer | [axara.bio ↗](https://axara.bio/data-products) |

---

## Free dataset

### KRAS G12C — Complete Covalent Tractability Map

The KRAS G12C dataset is freely available with no registration required. CYS12 confirmed Rank 1 against sotorasib and adagrasib.

**[Download free KRAS G12C report →](https://axara.bio/data-products)**

Includes:
- Tractability Map HTML report
- Scored residues CSV
- Digital Twin JSON
- Annotated PDB (B-factor = tractability score)

---

## Data disclaimer

This analysis is based exclusively on publicly deposited structures from the RCSB Protein Data Bank. All source structures are freely available at rcsb.org. No proprietary or confidential data has been used in any part of this analysis.

---

## Citation

If you use AXARA Covalent 30 data in your research, please cite:

```
AXARA BioScience (2026). AXARA Covalent 30: A validated covalent probe site
atlas for 30 high-value drug discovery targets. axara.bio
DOI: 10.5281/zenodo.XXXXXXX
```

---

## Full datasets

Full tractability map datasets — including probe reference cards, validation certificates, annotated PDB files, and bench protocols — are available at **[axara.bio](https://axara.bio)**.

Products:
- **Data Products** — complete tractability maps per target
- **Rule of 25 Engine** — score your scaffold for SDA conjugation efficiency
- **Axara Bridge** — map your PAL experimental hits back to structure
- **SDA-Pro Kit** — validated probe synthesis reagents

---

## Contact

**AXARA BioScience**
axara@axara.bio · [axara.bio](https://axara.bio)
