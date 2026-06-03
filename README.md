# Creating complex PDBe API queries

A reproducible tutorial demonstrating how to combine multiple [PDBe](https://www.ebi.ac.uk/pdbe/) API v2 endpoints to address structural biology questions.

This notebook accompanies the EMBL-EBI training webinar
[**Creating complex PDBe API queries**](https://www.ebi.ac.uk/training/events/creating-complex-pdbe-api-queries-2026/)
(03 June 2026), part of a six-part series on programmatic access to structural data through PDBe.

Each case study chains several endpoints to answer a defined biological question and assembles the
results into a structured `pandas` DataFrame, illustrating analysis patterns that extend beyond
single-endpoint queries.

All endpoints used here belong to the **[PDBe API v2](https://www.ebi.ac.uk/pdbe/api/v2)**
(base URL <https://www.ebi.ac.uk/pdbe/api/v2>). The endpoint paths listed in the tables below are
relative to this base URL.

## Overview

`pdbe_complex_api_workflows.ipynb` presents three case studies, each demonstrating a distinct
pattern of endpoint composition:

| # | Entry / accession | Pattern | Biological question |
|---|---|---|---|
| 1 | **1HVR** — HIV-1 protease | Residue-level joins (contacts ∩ interface ∩ conservation) | Which interface residues contact the inhibitor? |
| 2 | **2DN2** — Hemoglobin (HbA) | Entry-to-complex aggregation | What is known about this complex across the PDB? |
| 3 | **Q96Y14** — *S. tokodaii* hexokinase | UniProt-anchored aggregation (superposition clusters ∩ bound ligands) | Do ligand-binding states explain conformational clusters? |

### Composition patterns

| Pattern | Application | Endpoints chained | Join key |
|---|---|---|---|
| **Residue-level intersection** (Example 1) | Identifying residues that satisfy multiple structural criteria simultaneously | `entry/molecules` → `bound_molecules` → `bound_molecule_interactions` → `pisa/interfaces` → `sequence_conservation` | `(chain_id, author_residue_number)` |
| **Entry-to-complex aggregation** (Example 2) | Expanding a single PDB entry to all entries sharing the same biological complex | `complex/details` (by pdb_id) → `complex/details` (by pdb_complex_id) → `complex/bound_molecules_summary` → `complex/interactions` | `pdb_complex_id` |
| **UniProt-anchored aggregation** (Example 3) | Grouping every PDB chain associated with a UniProt accession by structural state | `uniprot/superposition` → `pdb/entry/summary` → `pdb/entry/molecules` → `pdb/bound_molecules` → `pdb/compound/summary` | `(pdb_id, auth_asym_id)` |

## Requirements

- Python 3.11 or later
- [`pandas`](https://pandas.pydata.org/)
- [`requests`](https://requests.readthedocs.io/)
- [Jupyter](https://jupyter.org/) (to run the notebook)

```bash
pip install pandas requests jupyter
```

## Usage

```bash
jupyter notebook pdbe_complex_api_workflows.ipynb
```

Execute the cells sequentially. The notebook issues live requests to the PDBe API, so an active
internet connection is required; no API key or authentication is needed.

## Intended audience

Life science students, postdoctoral researchers, bioinformaticians, and structural biologists
interested in querying structural data programmatically. No prior experience with the PDBe API is assumed.

## Author

Sri Devan Appasamy (EMBL-EBI)
