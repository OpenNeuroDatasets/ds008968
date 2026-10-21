# Spell-IT-OUT syllable datasets

EEG epochs around single syllables, ready for machine learning. Each dataset
has its own folder with one HDF5 file per subject, so `<dataset>/*.h5` is the
whole dataset. These files are derived from the BIDS data but are not a BIDS
datatype.

| Dataset | Task | Aligned to | Extra |
|---|---|---|---|
| `covert_cue` | covert | `syllabification_cue` | |
| `overt_cue` | overt | `syllabification_cue` | |
| `overt_production` | overt | `syllabification_syllable` | |

## How they were built

- EEG source: raw recordings
- Window: -0.3 to 0.6 s around the event
- Processing on the continuous signal: resampled to 512 Hz; band-pass 2–70 Hz; notch 50 Hz (causal IIR filters)
- Windows overlapping a reviewed `BAD_*` span are left out.

The full configuration is in `build_config.json`; its SHA-256 is stored in
every file.

## File layout

```
eeg/data                       (n_samples, n_channels, n_times), float32, V
eeg/times, eeg/channel_names
labels/phonetic_categories     (n_samples, n_categories), 0/1
labels/phonetic_category_names
samples/syllable, samples/id, samples/run, samples/trial_index, ...
```

```python
import h5py

with h5py.File("overt_cue/sub-01_ses-01_task-overt_desc-overtcue_dataset.h5") as f:
    X = f["eeg/data"][:]
    syllables = f["samples/syllable"].asstr()[:]
```

`manifest.tsv` lists every sample with its file and index; `exclusions.tsv`
lists the candidate events that were left out and why.
