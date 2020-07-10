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

## Ideas

Create models with the features extracted using librosa, and find the best model there. Compare
it to the models built using raw audio

Using neural networks, but also other models (such as SVM), and compare them.

Have a math section in the report?
