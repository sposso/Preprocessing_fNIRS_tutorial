# fnirs-preprocessing-guide

This repository provides a Python-based preprocessing pipeline for fNIRS signals using the MNE-Python and MNE-NIRS libraries. The goal is to offer a reproducible workflow for preparing fNIRS data for analysis, particularly in the context of speech perception studies.

## Dataset description:  Audio and visual speech 
The  data used in this tutorial is  a publicly available dataset from the [paper](https://www.sciencedirect.com/science/article/pii/S0378595521000903?via%3Dihub). In this study, fNIRS signals were recorded while **eight healthy adults perceived** three stimuli: auditory-only, visual-only, and a silence condition. The auditory-only stimuli comprised audio recordings with an average duration of 12.5 seconds per trial. The visual-only stimuli involved a person articulating a speech segment without sound, requiring subjects to read the lip movements. Auditory-only and visual-only conditions included **18 trials** each. Additionally, **10 trials** of silence were randomly presented throughout the experiment to serve as a baseline control condition. 


The fNIRS equipment utilized **44 channels** at a **sampling rate of 3.9 Hz** that cover the inferior IFG, left auditory, right auditory, and visual brain regions. The left and right auditory areas were separated into Auditory-A and Auditory-B. Auditory-A captures the rostral aspect and auditory-B caudal aspects of the temporal lobe, more specifically,  surrounding the planum temporale, also known as Wernicke's area. The visual region was divided into two subregions: Visual-A and Visual-B. Visual-A covers the cuneus and the superior occipital gyrus, while Visual-B encompasses the middle occipital gyrus. Additionally, 8 short-channel detectors were used in he recordings to be used to remove the influence from local non-cortical changes in blood oxygenation posteriorly. 

### Note
You can download the dataset using the [MNE-NRIS](https://mne.tools/mne-nirs/stable/index.html#) library. 

## Preprocessing pipeline 

The preprocessing pipeline presented here  was implemented in the original [paper]((https://www.sciencedirect.com/science/article/pii/S0378595521000903?via%3Dihub), where the data was released and consists of the following: 

1. Convert raw intensity signals to optical density.
2. **Scalp Coupling Index (SCI)**: The scalp coupling index value is calculated and used to identify optodes that were not well connected to the scalp. The scalp coupling threshold value was used to remove channels with a rejection criterion of < 0.8.
3. **Motion artifact correction**: Motion artifact is then removed using Temporal Derivative Distribution Re-pair (TDDR), which is applied to all channels.
4. **Short-channel regression**: Short-separation channel data are then subtracted from the standard long-separation channel signal to remove the inﬂuence from local non-cortical changes in blood oxygenation.
5. ** Convert to hemoglobin concentrations **:  The signal is then transformed to oxy (HbO) and deoxyhemoglobin (HbR)  using the modiﬁed Beer-Lambert Law.
6. **Band-pass filtering**: Filtering was applied to the signal (0.02-0.4 Hz) to remove slow drifts in the signal and components related to heart rate.
7. **Signal enhancement**: The signal enhancement method introduced [here](https://pubmed.ncbi.nlm.nih.gov/19945536)  is applied.
8. **Epoch extraction**: Epochs corresponding to the onset and 18 s after stimulus onset were extracted.
9. **Epoch rejection**: An epoch rejection criterion is employed to exclude any individual epochs with a peak-to-peak value exceeding 100 μM


