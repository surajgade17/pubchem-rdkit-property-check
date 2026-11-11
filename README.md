# PubChem vs RDKit Property Check

Compares molecular properties of 10 compounds, fetched from the PubChem REST API and recalculated with RDKit. The set includes four EGFR inhibitors (erlotinib, gefitinib, lapatinib, osimertinib) and six reference compounds.

## What it does
1. Fetches CID, MW, XLogP3, TPSA, H-bond donors/acceptors and SMILES from PubChem.
2. Recomputes MW, Crippen logP, TPSA and H-bond counts from the SMILES with RDKit.
3. Calculates the differences and a simple Lipinski rule-of-five violation count.
4. Plots PubChem XLogP3 against RDKit Crippen logP.

## Compounds
erlotinib, gefitinib, lapatinib, osimertinib, aspirin, paracetamol, ibuprofen, caffeine, chalcone, resorcinol

## Results
- **Molecular weight:** all 10 compounds agree within 0.04.
- **TPSA:** 8 of 10 agree within 0.05. Lapatinib differs by -8.65 (PubChem 115.0, RDKit 106.35) and caffeine by +3.42 (58.4 vs 61.82). The lapatinib gap is probably due to its sulfone group, since RDKit's default TPSA leaves out sulfur and phosphorus (not yet tested with `includeSandP=True`).
- **logP:** PubChem XLogP3 and RDKit Crippen logP correlate well (r about 0.96) but are different methods. RDKit is higher for 8 of 10 compounds and lower for ibuprofen and caffeine. Largest gap: lapatinib (5.1 vs 6.14).
- **H-bond donors:** identical for all 10.
- **H-bond acceptors:** RDKit counts one fewer than PubChem for gefitinib, lapatinib, aspirin and ibuprofen; the other 6 match.
- **Lipinski:** 9 of 10 have 0 violations. Lapatinib has 2 (MW > 500 and logP > 5).

## Takeaway
Values from different tools are not interchangeable. MW is identical in practice, TPSA and H-bond counts depend on the atom-typing rules, and logP depends on the method. Always say which tool produced a property when reporting it.

## Files
- `pubchem_rdkit_property_check.ipynb` : the Colab notebook
- `pubchem_vs_rdkit.csv` : full comparison table
- `logp_comparison.png` : logP scatter plot

## How to run
Open the notebook in Google Colab and run the cells in order. It installs RDKit and calls the public PubChem REST API (no key needed, with a short pause between requests).

## Limitations
- Only 10 compounds, so the correlation is descriptive, not a benchmark.
- Lipinski counts here use RDKit values, which differ slightly from PubChem for H-bond acceptors.
- Property values depend on PubChem's current data.

## Tools
Python, pandas, matplotlib, RDKit, requests, Google Colab

## Author
Suraj Gade (github.com/surajgade17)
