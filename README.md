# Dual-signal Transformation LSTM Network

Implementation of the stacked dual-signal transformation LSTM network (DTLN) for real-time noise suppression.


This model was handed in to the deep noise suppression challenge ([DNS-Challenge](https://github.com/microsoft/DNS-Challenge)) of Interspeech 2020. 


This approach combines a short-time Fourier transform (STFT) and a learned analysis and synthesis basis in a stacked-network approach with less than one million parameters. The model was trained on 500h of noisy speech provided by the challenge organizers. The network is capable of real-time processing (one frame in, one frame out) and reaches competitive results.
Combining these two types of signal transformations enables the DTLN to robustly extract information from magnitude spectra and incorporate phase information from the learned feature basis. The method shows state-of-the-art performance and outperforms the DNS-Challenge baseline by 0.24 points absolute in terms of the mean opinion score (MOS).
\
\
Author: Nils L. Westhausen ([Communication Acoustics](https://uol.de/en/kommunikationsakustik) , Carl von Ossietzky University, Oldenburg, Germany)

---
### Contents of the repository:

*  **DTLN_model.py:** \
  This file is containing the model, data generator and the training routine.
*  **run_training.py** \
  Script to run the training. Before you can start the training with `$ python run_training.py`you have to set the paths to you training and validation data inside the script. The training script uses a default setup.
* **run_evaluation.py** \
  Script to process a folder with optional subfolders containing .wav files with a trained DTLN model. With the pretrained model delivered with this repository a folder can be processed as following: \
  `$ python run_evaluation.py -i /path/to/input -o /path/for/processed -m ./pretrained_model/model.h5` \
  The evaluation script will create the new folder with the same structure as the input folder and the files will have the same name as the input files.
*  **pretrained_model/model.h5** \
  The model weights as used in the DNS-Challenge DTLN model.

---
### Python dependencies:

The following packages will be required for this repository:
* tensorflow (2.x)
* librosa
* wavinfo 


All additional packages (numpy, soundfile, etc.) should be installed on the fly when using conda or pip. I recommend using conda environments or [pyenv](https://github.com/pyenv/pyenv) [virtualenv](https://github.com/pyenv/pyenv-virtualenv) for the python environment. For training a GPU with at least 5 GB of memory is required and Cuda 10.1 together with at least the Nvidia driver 418. If you use conda Cuda will be installed on the fly and you just need the driver. For evaluation-only the CPU version of Tensorflow is enough. Everything was tested on Ubuntu 18.04.

---
### Training data preparation:

1. Clone the forked DNS-Challenge [repository](https://github.com/breizhn/DNS-Challenge). Before cloning the repository make sure `git-lfs` is installed. Also make sure your disk has enough space. I recommend downloading the data to a SSD for faster dataset creation.

2. Run `noisyspeech_synthesizer_multiprocessing.py` to create the dataset. `noisyspeech_synthesizer.cfg`was changed according to my training setup used for the DNS-Challenge. 

3. Run `split_dns_corpus.py`to divide the dataset in training and validation data. The classic 80:20 split is applied. This file was added to the forked repository by me.

---
### Run a training of the DTLN model:

1. Make sure all dependencies are installed in your python environment.

2. Change the paths to your training and validation dataset in `run_training.py`.

3. Run `$ python run_training.py`. 

One epoch takes around 21 minutes on a Nvidia RTX 2080 Ti when loading the training data from an SSD. 


  