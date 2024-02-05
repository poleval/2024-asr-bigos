# Polish Automatic Speech Recognition Challenge

## Introduction

**Automatic speech recognition** (ASR) has made significant progress over the last decade. Improvements in deep learning and increased data availability have resulted in accuracy levels for artificial speech transcription that are on par with human transcription, at least in specific domains, tasks, and speech characteristics. ASR technology has expanded to cover many new languages, use cases, user demographics, and devices. However, achieving robust speech recognition remains a challenge for many low-resource languages, specific speaker groups, application domains, and acoustic conditions.

To gauge the technological advancements in Polish ASR technology, we are introducing the Open Challenge for Polish ASR. This initiative draws inspiration from the Multi-Domain End-to-End Speech Recognition Benchmark for the English language [[1](#references)].

In order to promote multi-domain evaluation across a wide array of speech datasets, a new test dataset named BIGOS was introduced [[2](#references)]. It comprises recordings from 12 open datasets and has been manually curated to ensure dependable evaluation results.

PELCRA benchmark dataset contains selected corpora from PELCRA repository [[3](#references)] (SpokesMix, SpokesBiz and Diabiz sample) in the BIGOS format. The author of curated PELCRA corpora hopes that standardized formatting and distribution via Hugging Face platform will simplify access and use of publicly available ASR speech datasets for Polish. PELCRA corpora significant contributions are spontaneous and conversional speech Combined with BIGOS corpora, it enables the most comprehensive publicly available evaluation of Polish ASR systems in terms of number of speakers, devices and acoustic conditions.

## Task Definition

The goal of this challenge is to benchmark open Polish ASR systems against commercial services on a wide range of datasets.

The participants are provided with training, development, and test sets, from BIGOS and PELCRA corpora. Both datasets are available on Hugging Face [[4, 5](#references)]. While scores for `test-A` will be visible from the beginning, final ranking will be based on systems' performance on `test-B` set and provided after the submissions are closed.

The participants are **allowed** to both create their own system, and fine-tune an existing solution. However, they are obligated to provide a relevant description for the submission.

For each audio recording, the system is supposed to generate a transcription of the utterance. It is **forbidden** for the participants to use any data outside of the provided train and validation sets to develop their systems. It is also **prohibited** to manually transcribe the test examples.

## Dataset

The dataset is divided into four splits and each one of them is stored in corresponding directory. The files for each split have the same structure.

The `in.tsv` file is a tab-separated file with four columns:

1. `dataset` - name corresponding to the dataset available on Hugging Face, i.e. `amu-cai/pl-asr-bigos-v2` or `pelcra/pl-asr-pelcra-for-bigos`,

2. `subset` - name corresponding to the subset of the dataset, as on Hugging Face,

3. `split` - name corresponding to the split of the subset, as on Hugging Face,

4. `audioname` - name corresponding to the file id, as on Hugging Face.

**Example of `in.tsv` file:**

```
amu-cai/pl-asr-bigos-v2	fair-mls-20	train	fair-mls-20-train-0022-00001
amu-cai/pl-asr-bigos-v2	fair-mls-20	train	fair-mls-20-train-0022-00002
```

While text data is provided in the TSV format, the audio files are available on Hugging Face [[4, 5](#references)].

For `train` and `dev-0` splits, also `expected.tsv` files are provided. It is a tab-separated file with one column, where each row is a transcription for the matching audio recording from the `in.tsv` file.

**Example of `expected.tsv` file:**

```
szum mnoży w skałach okolicznych staje się rzeką a w gwałtownym pędzie pieni się huczy i zżyma w bałwany tym sroższy w biegu im dłużej wstrzymany
lecą sandały i trepki i pasy wrzawa powszechna przeraża i głuszy zdrętwiał hyacynt na takie hałasy chciałby uniknąć bitwy z całej duszy a przeklinając nieszczęśliwe czasy resztę kaptura nasadził na uszy
```

### Training Data

The `train` set consists of 311 175 samples.

|             | BIGOS  | PELCRA  | Total   |
| ----------- | ------ | ------- | ------- |
| No. samples | 82 025 | 229 150 | 311 175 |

### Development Data

The `dev-0` set consists of 42 786 samples.

|             | BIGOS  | PELCRA | Total  |
| ----------- | ------ | ------ | ------ |
| No. samples | 14 254 | 28 532 | 42 786 |

The participants are **allowed** to use `dev-0` set to develop their systems.

### Test Data

The `test-A` set consists of 20 284 samples and `test-B` - 20 285 samples.

|          | BIGOS  | PELCRA | Total  |
| -------- | ------ | ------ | ------ |
| `test-A` | 7 386  | 12 898 | 20 284 |
| `test-B` | 7 607  | 12 678 | 20 285 |
| Total    | 14 993 | 25 576 | 40 569 |

It is **forbidden** for the participants to manually transcribe the test examples.

### Downloading Datasets

The text data is provided in the TSV format and the audio files are available on Hugging Face [[4, 5](#references)].

## Evaluation

### Submission Format

The goal of the task is to generates an accurate transcription for each utterance. The submission should consist of a single tab-separated file with one column. Each line in the `out.tsv` file should contain hypothesis for the matching audio recording from the `in.tsv` file.

**Example of `out.tsv` file:**

```
szum mnoży w skałach okolicznych staje się rzeką a w gwałtownym pędzie pieni się huczy i zżyma w bałwany tym sroższy w biegu im dłużej wstrzymany
lecą sandały i trepki i pasy wrzawa powszechna przeraża i głuszy zdrętwiał hyacynt na takie hałasy chciałby uniknąć bitwy z całej duszy a przeklinając nieszczęśliwe czasy resztę kaptura nasadził na uszy
```

### Metrics

For each provided submission two measures of accuracy will be calculated:

- **Word Error Rate** (WER) - number of incorrectly transcribed words divided by the total number of tokens in the reference sentences.

$$ \text{WER} = \frac{\text{number of errors}}{\text{reference text length in words}} $$

- **Character Error Rate** (CER) - number of inccorectly transcribed characters divided by the total number of characters in the reference sentences.

$$ \text{CER} = \frac{\text{number of errors}}{\text{reference text length in characters}} $$

Both metrics range from 0 to 1, where 0 is the best score.

### Text Normalization

As some of the references do not contain punctuation or capitalization, evaluation is performed on normalized text to minimize the probability of false errors. All punctuation marks are removed and case folding is applied.

Since the normalization is performed during evaluation, there is no need for post-processing on the participant's side.

## Baseline

The scores achieved by the baseline systems are available [here](). The scripts used to generate the results are also provided.

## References

1. Sanchit Gandhi, Patrick von Platen, Alexander M. Rush. 2022. *ESB: A Benchmark For Multi-Domain End-to-End Speech Recognition*. [[Paper](https://arxiv.org/abs/2210.13352)] [[Hugging Face](https://huggingface.co/esb)]

2. Michał Junczyk. *BIGOS - Benchmark Intended Grouping of Open Speech Corpora for Polish Automatic Speech Recognition*. [[Paper](https://annals-csis.org/proceedings/2023/drp/1609.html)] [[Hugging Face](https://huggingface.co/datasets/michaljunczyk/pl-asr-bigos-v2)]

3. PELCRA Tools [[Documentation](http://docs.pelcra.pl/doku.php?id=start)]

4. amu-cai/pl-asr-bigos-v2 · Datasets at Hugging Face [[Hugging Face](https://huggingface.co/datasets/amu-cai/pl-asr-bigos-v2)]

5. pelcra/pl-asr-pelcra-for-bigos · Datasets at Hugging Face [[Hugging Face](https://huggingface.co/datasets/pelcra/pl-asr-pelcra-for-bigos)]

## Metadata

Tags: poleval-2023, asr