# HE-Austenite

An open dataset of hydrogen embrittlement in austenitic stainless steels tested in **hydrogen gas**, with weld data recorded separately from base metal.

Version 0.1 · 113 records · 9 sources, 1983–2024 · Data CC BY 4.0, code MIT

## Why this exists

Hydrogen infrastructure is welded, and the weld is the part of a joint least like the material a designer looks up. Most hydrogen embrittlement data covers base metal, and most recent weld studies charge specimens electrochemically, which cannot be converted to an equivalent gas pressure. This dataset keeps only gas exposure and records the weld zone for every row.

## Headline numbers

| | n | Median relative reduction of area |
|---|---|---|
| Base metal | 36 | 0.85 |
| Whole welded joint | 24 | 0.83 |
| Weld metal only | 14 | 0.49 |

Within the 304 family: base metal 0.72, welds 0.49.

One TIG-welded bar tested three ways at −45 °C in 106 MPa hydrogen (Nakamura 2018): base metal 1.02, weld metal as-welded 0.55, the same weld after a solution treatment that dissolves the delta ferrite 0.90.

![Ductility retained in gaseous hydrogen, by alloy family](figures/fig1_family.png)

## What's here

```
data/     he_austenite_v0.1.csv     the dataset
          HE-Austenite_v0.1.xlsx    same rows plus summary, dictionary, sources, extraction log
          records_raw.json          raw extraction records with provenance notes
scripts/  schema.json               record schema
          normalise.py              derived values and validation
          build_outputs.py          cleaning, CSV export
          build_workbook.py         Excel workbook
          make_figures.py           figures
figures/  three figures used in the paper
paper/    the data descriptor
docs/     data dictionary and scope notes
```

## Scope

A record is included if the material is an austenitic stainless steel (or a related grade, flagged), hydrogen was introduced **as a gas** (in-situ high-pressure testing or thermal precharging in hydrogen gas), a reference measurement exists from the same study, and the value appears in a table rather than only in a figure. Electrochemically charged studies are excluded, and listed with their reason in the workbook's extraction log.

Coverage: 0.1 to 172 MPa hydrogen, 228 to 423 K.

## Reproducing

```bash
pip install -r requirements.txt
python scripts/normalise.py data/records_raw.json   # validate and recompute derived values
python scripts/build_outputs.py                     # clean and export the CSV
python scripts/make_figures.py                      # regenerate the figures
```

## Status and known gaps

- 24 of 113 records have been independently re-extracted and matched; the rest have passed automated checks only.
- Four relevant studies report their results only in figures and are not yet included. Hirata 2015, on delta ferrite against RRA in 45 MPa hydrogen gas, is the most important of them and is the first target for v0.2.
- Family medians for 321/347 (n=3) and duplex (n=2) are indicative only.

## Corrections

If a value disagrees with the source in front of you, open an issue with the `record_id`, the value here, the value you read, and the table it came from. Additions follow the same scope rule.

## Citation

See `CITATION.cff`. Once archived on Zenodo, cite the DOI.

## Sources

Caskey DP-1643 (1983) · San Marchi & Somerday SAND2012-7321 (2012) · Balch et al. PVP2015-45591 · Michler et al. IJHE 2009 · Nakamura et al. Trans. JSME 2018 · Younes et al. IJHE 2013 · Iyer Can. Metall. Q. 1989 · Fukunaga Eng. Fail. Anal. 2024 · Matsuoka et al. Solid State Phenomena 2017. Full citations in the paper and in the workbook's Sources sheet.

This dataset contains values extracted from published tables. It does not redistribute any copyrighted PDF.
