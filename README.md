# Music Genre Classification

Final project for ISYE-6740 (Computational Data Analytics at Georgia Institute of Technology). 
The goal is to classify raw music audio into genres.

## Data

We leverage the [Free Music Archive](https://github.com/mdeff/fma) to get data for our project.

@inproceedings{fma_dataset,
  title = {{FMA}: A Dataset for Music Analysis},
  author = {Defferrard, Micha\"el and Benzi, Kirell and Vandergheynst, Pierre and Bresson, Xavier},
  booktitle = {18th International Society for Music Information Retrieval Conference (ISMIR)},
  year = {2017},
  archiveprefix = {arXiv},
  eprint = {1612.01840},
  url = {https://arxiv.org/abs/1612.01840},
} 

TODO: add more data description

## Usage

1. Clone the directory

```bash
git clone https://github.com/celestinhermez/music-genre-classification
cd music-genre-classification
```

2. Create a Python 3.7 environment

```bash
conda create -n music-genre-classification python=3.7
conda activate music-genre-classification
```

3. Install dependencies

```bash
pip install -r requirements.txt
```

4. Create a data directory, and download the data

```bash
mkdir data
cd data
curl -O https://os.unil.cloud.switch.ch/fma/fma_metadata.zip
curl -O https://os.unil.cloud.switch.ch/fma/fma_small.zip
curl -O https://os.unil.cloud.switch.ch/fma/fma_medium.zip
echo "f0df49ffe5f2a6008d7dc83c6915b31835dfe733  fma_metadata.zip" | sha1sum -c -
echo "ade154f733639d52e35e32f5593efe5be76c6d70  fma_small.zip"    | sha1sum -c -
echo "c67b69ea232021025fca9231fc1c7c1a063ab50b  fma_medium.zip"   | sha1sum -c -unzip fma_metadata.zip
unzip fma_small.zip
unzip fma_medium.zip
cd ..
```

On MacOS, the default `unzip` command failed. Using The Unarchiver utility in Finder to unzip
the file did solve the issue. If you run into "folder incomplete" errors, 
you can safely ignore them.

## Modeling
### Neural Networks

For this project, we use TensorFlow to build neural networks. The goal is to see whether we can
learn patterns in raw audio. This has the advantage of not requiring one to be familiar with
feature engineering for audio data, which is a specialized field in itself.

The notebook to build and extract the model is included. A few notes:
* training was done using GPU on Google Colab. This required the use of TFRecord datasets and
Google Cloud Storage (GCS). This part of the project re-used a lot of code from the official 
TensorFlow documentation
* in order to fully leverage TensorFlow, we converted mp3 files to wav files and uploaded 
the latter to GCS. This was done in convert_mp3_wav.ipynb
* the data uploaded to GCS, as well as the Drive mounted to the Colab, are private and
will return an error if you try to run the code as is. The hope is that this example
empowers you to reproduce our analysis and result with your own file structure if desired

Our findings:

## Ideas

Have a math section in the report?

Lingering NN issues
* sometimes there is a string instead of float data for certain songs
* shape issue