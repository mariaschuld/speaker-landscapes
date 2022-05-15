0. Environment setup
--------------------

Install all packages from the "requirements.txt" file into a fresh python environment and activate the environment, so that the notebooks have access to the packages.

1. Providing the data
---------------------

Save your raw data as a json lines file (.jl) in the main folder where the notebooks are located. The file has to be named ``raw_data.jl`` and has the following structure:

```
{"author": "<name_speaker1>", "text": "<quote1>"}
{"author": "<name_speaker2>", "text": "<quote2>"}
...
```

Each line represents one text quote by an author. Authors with multiple quotes in the text corpus will appear in multiple lines. 

Each line contains a python dictionary whith (at least) two string keys, ``"author"`` and ``"text"``. The expressions in the brakets <> contain the specific data you are providing.

2. Pre-processing the data
--------------------------

Open the ``clean_data.ipynb`` notebook and run all cells in consecutive order.

The main folder now has a new file called ``clean_data.txt``, which is a simple text file of the form

```
agent_<name_speaker1> <cleaned_quote1> 
agent_<name_speaker2> <cleaned_quote2>
...
```

The quote text is cleaned as follows:

* use lower casing
* remove punctuation except # and @
* form expressions of up to 4-grams when words are used in the same order more than 70 times

3. Training the word embedding
------------------------------

Open the ``train_embedding.ipynb`` notebook and run all cells in consecutive order.

The main folder now has a new file called ``word_embedding.emb``. This stores the embedding (i.e. a mapping from word to vectors) 
in gensim's KeyedVector format. 

Note: each time the notebook is run, it will produce the same outcome. However, this is a result of fixing random seeds in training and data loader. 
Changing these seeds at the top of the notebook will also change the details of the embedding. If training is robust -- which needs to be analysed for 
each new study -- then changing the seeds should not change the important trends we are interested in.


4. Extract information for the speaker landscape
------------------------------------------------

The speaker landscape consists of all word-vector pairs in the trained embedding that are prepended by ``"agent_"``. These words are the speaker tokens, while the vectors are their coordinates in a 250 dimensional space. To visualise the landscape, one can reduce the 250 to 2 dimensions. 

Open the ``extract_landscape.ipynb`` notebook and run all cells in consecutive order.

This creates a new file ``landscape_info.pkl`` which stores a pandas DataFrame with the author, their quotes, vector representation and reduced vector representation. 

Another file, ``annotations_info.pkl`` contains the word, vector, and reduced vector representation of desired annotation words, which are selected from the embeddings vocabulary. 

5. Plot and analyse the speaker landscape
-----------------------------------------

The ``analyse_landscape.ipynb`` notebook shows techniques used in the main paper to analyse and visualise the landscape, including:

* Plotting an annotated landscape
* Finding tweets posted by speakers located in a certain region
* Projecting to list of anger words
* Counting the tweet length
* Counting emojis



