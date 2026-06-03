# Creating complex PDBe API queries

A practical, runnable tutorial on how to **combine several [PDBe](https://www.ebi.ac.uk/pdbe/) API v2 endpoints** into biological workflows.

This notebook accompanies the EMBL-EBI training webinar
[**Creating complex PDBe API queries**](https://www.ebi.ac.uk/training/events/creating-complex-pdbe-api-queries-2026/)
(03 June 2026), part of a six-part series on accessing structural data programmatically with PDBe.

Rather than calling a single endpoint, each case study chains multiple endpoints together to answer a
specific biological question, and composes the results into a tidy `pandas` DataFrame.

All endpoints used in this notebook are part of the **[PDBe API v2](https://www.ebi.ac.uk/pdbe/api/v2)**, with base URL <https://www.ebi.ac.uk/pdbe/api/v2>. The endpoint paths shown in the tables below are relative to it.

## What's inside

`pdbe_complex_api_workflows.ipynb` works through three case studies, each illustrating a different
*pattern* of API composition:

| # | Entry / accession | Pattern | Biological question |
|---|---|---|---|
| 1 | **1HVR** — HIV-1 protease | Residue-level joins (contacts ∩ interface ∩ conservation) | Which dimer interface residues contact the inhibitor, and how conserved are they? |
| 2 | **2DN2** — Hemoglobin (HbA) | Entry → complex aggregation | Starting from one PDB entry, what does the whole PDBe-KB complex look like? |
| 3 | **Q96Y14** — *S. tokodaii* hexokinase | UniProt-anchored aggregation (superposition clusters ∩ bound ligands) | Do PDB structures of one protein partition into conformational states that track ligand binding? |

### The three composition patterns

| Pattern | Where to use it | Endpoints chained | Join key |
|---|---|---|---|
| **Residue-level intersection** (Example 1) | "Which residues do X *and* Y?" | `entry/molecules` → `bound_molecules` → `bound_molecule_interactions` → `pisa/interfaces` → `sequence_conservation` | `(chain_id, author_residue_number)` |
| **Entry → complex aggregation** (Example 2) | Lift one PDB entry into all entries sharing the same biological complex | `complex/details` (by pdb_id) → `complex/details` (by pdb_complex_id) → `complex/bound_molecules_summary` → `complex/interactions` | `pdb_complex_id` |
| **UniProt-anchored aggregation** (Example 3) | From one UniProt accession, group every related PDB chain by structural state | `uniprot/superposition` → `pdb/entry/summary` → `pdb/entry/molecules` → `pdb/bound_molecules` → `pdb/compound/summary` | `(pdb_id, auth_asym_id)` |

## Requirements

- Python 3.11+
- [`pandas`](https://pandas.pydata.org/)
- [`requests`](https://requests.readthedocs.io/)
- [Jupyter](https://jupyter.org/) (to run the notebook)

```bash
pip install pandas requests jupyter
```

## Running it

```bash
jupyter notebook pdbe_complex_api_workflows.ipynb
```

Then run the cells top to bottom. The notebook makes live calls to the PDBe API, so an internet
connection is required; no API key is needed.

## Audience

Students in the life sciences, postdoctoral researchers, bioinformaticians, and structural biologists
interested in exploring structural data programmatically. No prior experience with the PDBe API is assumed.

## Author

Sri Devan Appasamy (EMBL-EBI)
