This repository contains code to supplement the paper "Speaker Landscapes: Machine Learning Opens a Window on the Everyday Language of Opinion".

Speaker landscapes are a kind of user embedding, or a spatial representation of speakers by vectors such that the proximity between two speaker vectors is a measure for the similarity of their speech samples from a data corpus. The basic trick of constructing a speaker landscape is to annotate data of speech samples with a token representing the speaker and then training a word embedding.

The main folder shows how to create and analyse speaker landscapes using dummy datasets to highlight the kind of datastructures that are inputs and outputs to the code. The real dataset are available on request, subject to the data owner's permission. 

The two word embeddings studied in the paper can be found in the "case_studies" folder. The speaker tokens are marked by the prefix "agent_" (such as "agent_zuma". 

How to train and use speaker landscapes
---------------------------------------

Environment setup
*****************

Install all packages from the ``requirements.txt`` file into a fresh python environment and activate the environment, so that the notebooks have access to the packages.

1. Providing the data
*********************

Save your raw data as a json lines file (.jl) in the main folder where the notebooks are located. The file has to be named ``raw_data.jl`` and has the following structure:

.. code-block:: python

  {"author": "<name_speaker1>", "text": "<quote1>"}
  {"author": "<name_speaker2>", "text": "<quote2>"}
  ...


Each line represents one text quote by an author. Authors with multiple quotes in the text corpus will appear in multiple lines. 

Each line contains a python dictionary whith (at least) two string keys, ``"author"`` and ``"text"``. The expressions in the brakets <> contain the specific data you are providing.

2. Pre-processing the data
**************************

Open the ``clean_data.ipynb`` notebook and run all cells in consecutive order.

The main folder now contains a new file called ``clean_data.txt``, which is a simple text file of the form

.. code-block:: python

  agent_<name_speaker1> <cleaned_quote1> 
  agent_<name_speaker2> <cleaned_quote2>
  ...

The quote text is cleaned as follows:

* use lower casing,
* remove punctuation except # and @,
* form expressions of up to 4-grams when words are used in the same order more than 70 times,

3. Training the word embedding
******************************

Open the ``train_embedding.ipynb`` notebook and run all cells in consecutive order.

The main folder now contains a new file called ``word_embedding.emb``. This stores the embedding (i.e. a mapping from word to vectors) 
in gensim's `KeyedVector <https://radimrehurek.com/gensim/models/keyedvectors.html>`_ format. 

Note: Since we use multiple workers for training, each training may result in a slightly different embedding, even thought the random seeds in the notebook are fixed. 


4. Extract information for the speaker landscape
************************************************

The speaker landscape consists of all word-vector pairs in the trained embedding that are prepended by ``"agent_"``. These words are the speaker tokens, while the vectors are their coordinates in a 250 dimensional space. To visualise the landscape, one can reduce the 250 to 2 dimensions. 

Open the ``extract_landscape.ipynb`` notebook and run all cells in consecutive order.

This creates a new file ``landscape_info.pkl`` which stores a pandas `DataFrame <https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.html>`_ with the author, their quotes, vector representation and reduced vector representation. 

Another file, ``annotations_info.pkl`` contains the word, vector, and reduced vector representation of desired annotation words, which are selected from the embeddings vocabulary. 

5. Plot and analyse the speaker landscape
*****************************************

The ``analyse_landscape.ipynb`` notebook shows techniques used in the main paper to analyse and visualise the landscape, including:

* Plotting an annotated landscape
* Projecting to list of anger words
* Counting the tweet length
* Counting emojis
