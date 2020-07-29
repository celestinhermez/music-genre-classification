# Music Genre Classification

Final project for ISYE-6740 (Computational Data Analytics at Georgia Institute of Technology). 
The goal is to classify raw music audio into genres.

## Data

We leverage the [Free Music Archive](https://github.com/mdeff/fma) to get data for our project.

This is a wide repository of audio segments with relevant metadata, 
which was originally collected for International Society for Music Information Retrieval Conference (ISMIR) in 2017.
We focus on the small subset of the data. It contains 8,000 audio segments, each 30s long. There are 8 distinct genres:  
* Hip-Hop
* Pop
* Folk
* Experimental
* Rock
* International
* Electronic
* Instrumental
\end{itemize}
Each genre comes with 1,000 representative audio segments. 
With the sample rate of 44,100, this means there are more than 1 million data points for each example, 
creating more than 10<sup>9</sup> data points total.

## Usage
### Data Download

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
echo "f0df49ffe5f2a6008d7dc83c6915b31835dfe733  fma_metadata.zip" | sha1sum -c -
echo "ade154f733639d52e35e32f5593efe5be76c6d70  fma_small.zip"    | sha1sum -c -
unzip fma_metadata.zip
unzip fma_small.zip
cd ..
```

In case of issues with unzipping the files:
* Windows: use 7-zip
* Mac:

```
brew install p7zip
cd data
7z x fma_small.zip
```

### Feature Extraction

Run the notebook which can be found at `data_preprocessing/create_audio_features.ipynb`

### Exploratory Data Analysis

Some graphs and general exploratory data analysis can be found at `model_development/exploratory_data_analysis.ipynb`

### Modeling

We consider two broad approches: standard machine learning, and deep learning. In addition,
we work both with raw audio and with features informed from music theory research.

#### Standard Machine Learning

We can compare various standard machine learning methods and see how they perform on this 
classification task. In order to do so, we work with extracted features, either chosen by us
or from the provided metadata.

#### Deep Learning

For this project, we use TensorFlow to build neural networks. The goal is to see whether we can
learn patterns in raw audio. This has the advantage of not requiring one to be familiar with
feature engineering for audio data, which is a specialized field in itself. We also see whether
models built from extracted features perform better

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
* the models built from raw audio suffer from overfitting
* treating the Mel spectrogram as an image, and applying transfer learning on it delivers
the best testing accuracy (~40%)

## Areas for Future Work

The identification of overfitting as a major issue opens several areas of future work:
* reduce the dimensionality of the data: 
techniques such as PCA could be used to combine extracted features together 
and limit the size of the feature vectors for each example
* increase the size of the training data: the data source makes available
 larger subsets of the data. We limited our exploration to 
 less than 10% of the entire dataset. Should more computational resources be available, 
 or dimensionality of the data be successfully reduced, 
 we could consider using the full dataset. 
 This would most likely enable our methods to isolate more patterns 
 and greatly improve performance
*formalize the search for optimal architectures: 
using tensorboard or other cross-validation methods 
could help further tune the architectures of the neural networks created.
 This is a problem which is known to be hard, and we believe there is still 
 room for improvement there

