# Labeled Span Graph Network

### Requirements
* Python 2.7
  * TensorFlow 1.8.0
  * pyhocon (for parsing the configurations)
  * tensorflow_hub (for ELMo)

## Getting Started
### Setting Up:
* Python 2.7
* TensorFlow 1.8.0
* pyhocon (for parsing the configurations)
* tensorflow_hub (for ELMo)

* sudo apt-get install tcsh (Only required for processing CoNLL05 data)
* [Git Large File Storage] (https://git-lfs.github.com/): Required to download the large model files. Alternatively, you could get the models [here](https://drive.google.com/drive/folders/1TPpXx1-0TDL-hcMDa0b6fwmvn2HIp-yk?usp=sharing)
* [GloVe](https://nlp.stanford.edu/projects/glove/) embeddings and the [srlconll](http://www.lsi.upc.edu/~srlconll/soft.html) scripts:  
`./scripts/fetch_required_data.sh`
`./scripts/build_custom_kernels.sh` (Please make adjustments to the script based on your OS/gcc version)


## CoNLL Data
For replicating results on CoNLL-2005 and CoNLL-2012 datasets, please follow the steps below.

### CoNLL-2005
The data is provided by:
[CoNLL-2005 Shared Task](http://www.lsi.upc.edu/~srlconll/soft.html),
but the original words are from the Penn Treebank dataset, which is not publicly available.
If you have the PTB corpus, you can run:  
` ./scripts/fetch_and_make_conll05_data.sh  /path/to/ptb/`  

### CoNLL-2012
You have to follow the instructions below to get CoNLL-2012 data
[CoNLL-2012](http://cemantix.org/data/ontonotes.html), this would result in a directory called `/path/to/conll-formatted-ontonotes-5.0`.
Run:  
`./scripts/make_conll2012_data.sh /path/to/conll-formatted-ontonotes-5.0`

## Training Instructions

* Experiment configurations are found in `experiments.conf`
* Choose an experiment that you would like to run, e.g. `best`
* For a single-machine experiment, run the following two commands:
  * `python singleton.py <experiment>`
  * `python evaluator.py <experiment>`
* Results are stored in the `logs` directory and can be viewed via TensorBoard.
* For final evaluation of the checkpoint with the maximum dev F1:
  * Run `python test_single.py <experiment>` for the single-model evaluation.

## Other Quirks

* It does not use GPUs by default. Instead, it looks for the `GPU` environment variable, which the code treats as shorthand for `CUDA_VISIBLE_DEVICES`.
* The evaluator should not be run on GPUs, since evaluating full documents does not fit within GPU memory constraints.
* The training runs indefinitely and needs to be terminated manually. The model generally converges at about 400k steps and within 48 hours.
