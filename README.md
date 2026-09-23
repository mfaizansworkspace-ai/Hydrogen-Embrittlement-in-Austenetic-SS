# HE-Austenite

An open dataset of hydrogen embrittlement in austenitic stainless steels tested in **hydrogen gas**, with weld metal recorded separately from base metal.

113 records · 9 sources, 1983–2024 · Data CC BY 4.0 · Code MIT

## Objective

Hydrogen infrastructure is welded. Refuelling stations run at 70 MPa, storage vessels and tube trailers are welded assemblies, and repurposing existing pipe networks depends on how the joints behave rather than the plate.

A weld is a casting sitting inside a wrought product. Its structure is dendritic, its composition differs from the parent metal, and austenitic stainless weld metal deliberately retains a few percent of delta ferrite to avoid solidification cracking. Delta ferrite is body-centred cubic, in which hydrogen moves orders of magnitude faster than in austenite. The weld is therefore the part of a joint least like the material a designer looked up, and the part hydrogen reaches first.

Most published embrittlement data covers base metal, and most recent weld studies charge specimens electrochemically, which cannot be converted to an equivalent gas pressure without assumptions about charging efficiency. This dataset keeps only gaseous exposure and records the weld zone for every row.

## Results

| Zone | n | Median relative reduction of area |
|---|---|---|
| Base metal | 36 | 0.85 |
| Whole welded joint | 24 | 0.83 |
| Weld metal only | 14 | 0.49 |

Within the type 304 family alone: base metal 0.72, welds 0.49.

Relative reduction of area (RRA) is the reduction of area measured in or after hydrogen divided by the value in the reference environment. A value of 1.0 means no measurable loss. Japanese regulatory practice accepts austenitic stainless steels for hydrogen service at RRA ≥ 0.80.

![Relative reduction of area in gaseous hydrogen by alloy family](figures/fig1_family.png)

The alloy family ordering follows austenite stability. Type 310 and the nitrogen-strengthened grades 21-6-9 and 22-13-5 hold their ductility; the type 304 family, whose austenite transforms under strain, does not.

### Delta ferrite

Nakamura et al. (2018) tested one TIG-welded bar three ways at 233 K in 106 MPa hydrogen:

| Condition | RRA |
|---|---|
| 316 hi-Ni base metal | 1.02 |
| 317L weld metal, as welded (delta ferrite present) | 0.55 |
| 317L weld metal, post-weld solution treated | 0.90 |

One material, one gas, one temperature. The ferrite is the variable that changes.

## Repository contents

```
data/     he_austenite_v1.0.csv     the dataset, 113 records
          HE-Austenite_v1.0.xlsx    same rows plus summary, data dictionary, sources, extraction log
          records_raw.json          raw extraction records with full provenance notes
scripts/  schema.json               record schema
          normalise.py              derived values and validation
          build_outputs.py          cleaning and CSV export
          build_workbook.py         Excel workbook
          make_figures.py           figures
figures/  the three figures used in the paper
paper/    data descriptor, Word and PDF
docs/     data dictionary and source list
```

## Scope

A record is included when:

1. the material is an austenitic stainless steel, or a related grade flagged by `material_family` (duplex 2507, A286);
2. hydrogen was introduced **as a gas**, either by testing in high-pressure hydrogen or by thermal precharging in hydrogen gas;
3. a reference measurement exists from the same study and material, in air, helium, nitrogen, argon or the uncharged condition;
4. the value appears in a table rather than only in a figure.

Coverage: 0.1 to 172 MPa hydrogen, 228 to 423 K.

Electrochemically charged studies are excluded. They are listed with the reason in the Extraction log sheet of the workbook.

## Data quality

- Extraction was manual, from source tables into a validated JSON schema.
- 24 of 113 records were independently re-extracted from the source documents and compared; all matched. Those rows are marked `verified = yes`.
- Every derived ratio was recomputed, every record checked against the scope rule, and every percentage range-checked.
- Where a source printed its own ratio, the computed value was compared against it. For Balch et al. (2015) the six computed weld values reproduce the published ones within rounding.

## Known gaps

- Four relevant studies report results only in figures and are not included. Hirata (2015), on delta ferrite against RRA in 45 MPa hydrogen gas, is the most important of them and is the first target for the next release.
- Family medians for types 321/347 (n = 3) and duplex (n = 2) rest on few records and are indicative only.
- Reference environments are mixed: 48 records against air, 28 against high-pressure helium, 32 against the uncharged condition, 5 against nitrogen or argon. A helium reference at test pressure is stricter than an air reference. The field is recorded so users can filter.

## Reproducing

```bash
pip install -r requirements.txt
python scripts/normalise.py data/records_raw.json   # validate and recompute derived values
python scripts/build_outputs.py                     # clean and export the CSV
python scripts/make_figures.py                      # regenerate the figures
```

## Sources

Caskey, DP-1643 (1983) · San Marchi and Somerday, SAND2012-7321 (2012) · Balch et al., PVP2015-45591 · Michler et al., Int. J. Hydrogen Energy (2009) · Nakamura et al., Trans. JSME (2018) · Younes et al., Int. J. Hydrogen Energy (2013) · Iyer, Can. Metall. Q. (1989) · Fukunaga, Eng. Fail. Anal. (2024) · Matsuoka et al., Solid State Phenomena (2017).

Full citations are in `docs/data_dictionary.md`, in the Sources sheet of the workbook, and in the paper.

This repository contains values extracted from published tables. It does not redistribute any copyrighted PDF.

## Corrections and contributions

If a value here disagrees with the source in front of you, open an issue with the `record_id`, the value in the dataset, the value you read, and the table it came from. Additions are welcome under the same scope rule: gaseous hydrogen, a paired reference measurement, and a value that appears in a table.

## Citation

See `CITATION.cff`. Once the release is archived on Zenodo, cite the DOI.

## Licence

Code MIT. Dataset files in `data/` are CC BY 4.0.
