# Benchtop ¹H NMR dataset of edible oils for benchmarking conversational LLMs

Raw and processed low-field ¹H NMR data of 66 vegetable oil samples (extra virgin olive oil, high-oleic sunflower oil and sunflower oil), together with the binned data matrix used in the article:

> Belmonte-Sánchez, J. R.; Romero-González, R.; Garrido Frenich, A. *LLMs in the laboratory: Benchmarking conversational AI for multivariate analysis of NMR spectroscopic data.* Analytical Chemistry, 2026 (manuscript ac-2026-02701j).

The samples and their NMR classification were originally reported in:

> Belmonte-Sánchez, J. R.; Romero-González, R.; Tello-Jiménez, J. A.; Garrido Frenich, A. Expanding Horizons: Toward the Classification of Extra Virgin Olive Oil Using ¹H NMR and Machine Learning. *Food Control* **2026**, *179*, 111610. https://doi.org/10.1016/j.foodcont.2025.111610

## Contents

| Path | Content |
|---|---|
| `dataset/LLMsOilDatasetNMR.xlsx` | The exact workbook supplied to the tested systems: 66 spectra × 1,395 binned intensities, with the sample identifiers and the class labels of the 57 labeled samples. Unmodified. |
| `dataset/LLMsOilDatasetNMR.csv` | The same table exported to CSV (UTF-8, comma-separated). |
| `dataset/sample_list.csv` | One row per sample: file identifier, role (labeled or blind unknown), oil type, workbook row identifier, identifier stored inside the JCAMP-DX files, acquisition date and time, number of scans, receiver gain and magnet temperature. |
| `raw_fid_jcamp/` | 66 free induction decays (FID) in JCAMP-DX 5.01 (`NMR FID`, NTUPLES), as exported by the spectrometer software. These are the raw data. |
| `processed_spectra_jcamp/` | 66 frequency-domain spectra in JCAMP-DX 5.01 (`NMR SPECTRUM`, XYDATA), as processed and exported by the spectrometer software. |
| `processed_spectra_csv/` | The same processed spectra as CSV text (`Frequency(ppm), Real, Imaginary`, 131,072 points) with the processing parameters in a `#` header. |
| `LICENSE` | Creative Commons Attribution 4.0 International (CC BY 4.0). |
| `CITATION.cff` | Citation metadata. |

Total size is about 620 MB.

## Samples

66 oils of three types: 22 extra virgin olive oils (EVOO), 22 high-oleic sunflower oils (HOSO) and 22 sunflower oils (SO). In the benchmark, 57 samples were labeled (19 per class) and 9 were supplied as blind unknowns, "Unknown 1" to "Unknown 9". For reference, Unknowns 1–3 are EVOO, 4–6 are HOSO and 7–9 are SO, as stated in the article.

**File naming.** Labeled samples keep the identifier used in the workbook, `<n>_ControlNMR-<TYPE>` (for example `296_ControlNMR-EVOO`), where `<n>` is the sample number of the measurement campaign. The nine blind samples are named `Unknown1` to `Unknown9`. Two details to be aware of:

- In the workbook the blind samples are spelled `Unknonw1` to `Unknonw9` (sic). The workbook is provided unmodified because it is the exact input the systems received; `dataset/sample_list.csv` maps every workbook row to its files.
- The JCAMP-DX and CSV files are unmodified exports, so their `##TITLE=` field (JCAMP-DX) still holds the acquisition identifier, which for the blind samples includes the oil type. That identifier is listed in the `jcamp_title` column of `dataset/sample_list.csv`.

## Acquisition

All spectra were acquired on a Nanalysis NMReady 100 benchtop spectrometer (permanent magnet, ¹H at 102.4856 MHz, NMReady software v3.0.8.3) between 16 and 20 December 2024, with the oils dissolved in CDCl₃ (chloroform-d). Acquisition parameters, identical for all samples and recorded in every JCAMP-DX header:

| Parameter | Value |
|---|---|
| Scans | 8 |
| 90° pulse | 16.05 µs |
| Acquisition time | 11.42 s |
| Time-domain points | 16,384 (complex) |
| Spectral width | 14.0 ppm (about 1,435 Hz), centred at 5.0 ppm |
| Inter-scan delay | 1.0 s |
| Receiver gain | fixed per sample (19, 20 or 21) |
| Magnet temperature | about 33.2 °C |
| Total time per sample | about 106.5 s |

## Processing of the exported spectra

The `.jdx` and `.csv` spectra are the spectrometer's own processing of each FID: zero filling to 131,072 points, exponential apodization (0.1 Hz), Fourier transform and automatic phase correction (the phase values are recorded in each header), without baseline correction. The chemical shift axis runs from −2.0 to 12.0 ppm.

## Binned dataset

`dataset/LLMsOilDatasetNMR.xlsx` (sheet `NMR Oils`) has one row per sample and the columns `ClassID` (EVOO, HOSO or SO; empty for the blind samples), `Primary ID` (sample identifier) and 1,395 numeric columns whose headers are the centres of 0.01 ppm bins from −1.94 to 12.00 ppm. The spectra were processed and binned at 0.01 ppm as described in the article, and each binned spectrum is scaled so that its maximum intensity equals 100. The 57 labeled rows form the training matrix of the article (57 × 1,395); the complete workbook is 66 × 1,395.

## Reading the files

The JCAMP-DX files open in common NMR software (for example MestReNova or TopSpin) and with open-source JCAMP-DX readers such as the Python packages `nmrglue` and `jcamp`. The CSV spectra can be read with any spreadsheet program or, in Python, with `pandas.read_csv(path, comment="#", names=["ppm", "real", "imaginary"])`.

## License and citation

The data are released under the Creative Commons Attribution 4.0 International license (CC BY 4.0). If you use them, please cite the article above and this dataset (see `CITATION.cff`).

## Contact

José Raúl Belmonte-Sánchez (jbs324@ual.es), Department of Chemistry and Physics, Analytical Chemistry Area, Research Centre for Agricultural Food Biotechnology (CIAIMBITAL), University of Almería, Spain.
