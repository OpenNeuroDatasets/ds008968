# Spell-IT-OUT

EEG recorded while native Italian speakers listened to sentences, repeated
them, and then produced them again syllable by syllable at a fixed pace,
either aloud (`task-overt`) or in their mind without moving (`task-covert`).
For the overt task, the participants' speech was recorded and its word and
syllable timing aligned by hand; the timing is released as events, the voice
recordings are not.

- 25 participants, one session each (about one hour)
- 2 tasks × 3 runs × 20 trials = 150 recordings, 3000 trials
- 20 sentences, 161 distinct words, 192 distinct syllables
- 61 EEG channels + 1 EOG channel at 2048 Hz, BrainVision format

## Participants

25 native Italian speakers (14 male, 11 female), aged 22–33. Three were
left-handed (2 male, 1 female), the others predominantly right-handed. None
reported neurological or psychiatric disorders; all had normal hearing and
normal or corrected-to-normal vision. Some had taken part in motor-imagery
BCI experiments before. Per-participant demographics are not released.

Participants gave written consent to the collection and dissemination of
their data. The study was approved by the Ethics Committee for Psychological
Research, Area 17, University of Padova (ref. 1801-a), in accordance with the
Declaration of Helsinki. Participant IDs were randomized and recording dates
removed.

## Protocol

Participants sat in an electrically shielded, acoustically isolated room,
30–50 cm from a screen. Before the recording they read the sentence list to
get familiar with the words and the phonetic notation used for the syllables.

The overt block (3 runs) always came first, followed by the covert block
(3 runs). Each run lasted about 10 minutes and presented the 20 sentences once,
in random order. A trial (about 35 s) went as follows:

1. **Fixation** (`fixation`, 2 s): a cross at the centre of the screen.
2. **Listening** (`listening_cue`, ~8 s): the sentence is shown and played by a
   text-to-speech synthesizer (Google gTTS); a 3 s pause after playback lets
   participants finish reading.
3. **Repetition** (`repetition_cue`, 5 s): the sentence disappears and
   participants repeat it aloud (overt) or imagine repeating it (covert).
4. **Syllabification**: a three-dot countdown (`pre_syllabification_dot`,
   0.6 s each) sets the pace, then the syllables scroll on screen one every
   0.6 s (`syllabification_cue`), with the current one highlighted.
   Participants pronounce, or imagine pronouncing, each syllable as it appears.
5. **Trial end**: a visual cue, then a 4 s pause.

Covert instructions: *"Imagine that you have something to say, but you cannot
say it out loud or show any facial expression. You should feel your jaw, lips,
tongue, and vocal cords as if they were about to activate, but in reality, you
remain motionless."* Participants were also asked not to grind their teeth or
nod to keep the rhythm. During the overt runs the experimenter noted
production errors and gave feedback after each run.

## Stimuli

20 handcrafted sentences with a similar structure (subject–verb–complement),
10–13 words and 20–26 syllables each, about 4–4.5 s when spoken. Words were
chosen to cover the Italian phonetic inventory with a phoneme distribution
close to PhonItalia, which also provided the syllabification and phonetic
transcription.

`stimuli/wordlist.tsv` has no header; its columns are sentence id, sentence,
and syllabification (syllables separated by `.`, words by spaces).
`stimuli/audio/` holds the synthesized sentences with their TextGrids.

## Acquisition

- ANT Neuro waveguard original cap, 64 Ag/AgCl electrodes, extended 10–20
  layout; ANT Neuro eego sports amplifier.
- 2048 Hz, reference CPz, ground AFz, impedances below 10 kΩ.
- EOG electrode below the left eye.
- Recorded with ROS-Neuro on Linux. EEG was stored as GDF and audio as ROS
  bags; the two streams were aligned through the event markers logged in both.
  The residual synchronization error was not measured.
- Overt speech was recorded with the computer's built-in microphone, then
  resampled to 44112 Hz and saved as mono 16-bit WAV.
- EEG and audio were cropped from 0.5 s before the first event to 0.5 s after
  the last one.

Electrode positions are the standard 10–20 template, not digitized positions.

## Speech annotations

Speech in the overt runs was segmented with Silero-VAD, aligned to the
transcriptions with the Montreal Forced Aligner, and then checked and
corrected by hand in Praat at sentence, word and syllable level. Syllables
that were skipped or produced incorrectly have no annotation (45 in the
syllabification phase). The resulting timings are in the events files
(`repetition_word`, `repetition_syllable`, `syllabification_syllable`). The
voice recordings themselves are not distributed, to protect the participants'
identity.

## Files

```
sub-XX/ses-01/eeg/              raw EEG (BrainVision), events, channels, electrodes
stimuli/                        sentence list, synthesized sentences and TextGrids
derivatives/preprocessing/      cleaned continuous EEG (see its README)
derivatives/syllable-datasets/  syllable epochs in HDF5, ready for ML (see its README)
```

## Events

`*_events.tsv` has one row per event:

| Column | Content |
|---|---|
| `onset`, `duration` | seconds |
| `trial_type` | event name (below) |
| `phase` | trial phase the event belongs to |
| `trial_index`, `token_index` | trial and word/syllable position |
| `token` | word or syllable (phonetic notation) |
| `trigger_code` | original trigger, for trigger-based events |
| `event_source` | `gdf_trigger`, `textgrid_word` or `textgrid_syllable` |
| `stimulus_id`, `stim_file` | sentence id and stimulus audio |

Event names:

- from triggers: `trial_start`, `fixation`, `listening_cue`, `repetition_cue`,
  `pre_syllabification_dot`, `syllabification_cue`, `trial_end`;
- from the stimulus alignment: `listening_word`, `listening_syllable`;
- from the participant's speech (overt only): `repetition_word`,
  `repetition_syllable`, `syllabification_syllable`.

`na` is a real syllable. When reading the TSVs with pandas use
`keep_default_na=False`, otherwise it becomes a missing value. Missing values
are written as `n/a`.

## Things to keep in mind

- The overt block always preceded the covert one, so condition comparisons may
  include practice or fatigue effects.
- Syllables were shown on screen, so cue-aligned EEG also contains visual
  responses, in both tasks.
- Overt recordings contain muscle activity from speaking, also after
  preprocessing, which was deliberately conservative.
- Consecutive syllable epochs overlap. For machine learning, split by trial,
  run or participant rather than by epoch; we suggest training on the first
  two runs and testing on the third.

## Getting started

The `spellitout` Python package loads the dataset through MNE-BIDS:
<https://github.com/neurorobotics-iaslab/spell-it-out>.

```python
from spellitout import SpellItOutDataset

dataset = SpellItOutDataset("path/to/spellitout")
run = dataset.get_run(subject=1, task="overt", run=1)
run.raw, run.events
```

## License and citation

Released under CC0. If you use this dataset, please cite:

Cortecchia, T., Tortora, S., Kiesner, J., Menegatti, E., Tonin, L. *Spell-It-Out, a speech EEG dataset for
overt and covert production of Italian syllables.* TODO: journal and DOI.

Contact: Tommaso Cortecchia (tommaso.cortecchia@polito.it), Luca Tonin
(luca.tonin@unipd.it).

## References

Appelhoff, S., Sanderson, M., Brooks, T., Vliet, M., Quentin, R., Holdgraf, C., Chaumon, M., Mikulan, E., Tavabi, K., Höchenberger, R., Welke, D., Brunner, C., Rockhill, A., Larson, E., Gramfort, A. and Jas, M. (2019). MNE-BIDS: Organizing electrophysiological data into the BIDS format and facilitating their analysis. Journal of Open Source Software 4: (1896). https://doi.org/10.21105/joss.01896

Pernet, C. R., Appelhoff, S., Gorgolewski, K. J., Flandin, G., Phillips, C., Delorme, A., Oostenveld, R. (2019). EEG-BIDS, an extension to the brain imaging data structure for electroencephalography. Scientific Data, 6, 103. https://doi.org/10.1038/s41597-019-0104-8

Goslin, J., Galluzzi, C. & Romani, C. (2014). PhonItalia: a phonological lexicon for Italian. Behavior Research Methods, 46, 872–886.
