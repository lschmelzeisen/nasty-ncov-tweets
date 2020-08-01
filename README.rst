========================================================================================
NASTY nCoV Tweets Dataset
========================================================================================

This is a dataset of Tweets related to the novel coronavirus (nCoV, SARS-CoV-2,
COVID-19).

Currently, it contains **English** and **German** Tweets from **1st December 2019** up
to **31st May 2020**.

It's main distinction from other coronavirus Tweet datasets is that it was not created
using Twitter's streaming API, but through the use of `NASTY
<https://github.com/lschmelzeisen/nasty>`_, a Tweet retriever using the Twitter Web UI.
The upside compared to using the streaming API is that with methodology guarantees
that no time frame is missed, as past Tweets can be retrieved at will.
Consequently, relevant Tweets that might have been deleted before retrieval are not
included in the dataset.

To confirm with the `Twitter Developer Policy
<https://developer.twitter.com/en/developer-terms/agreement-and-policy#id8>`_, we can
only make public the IDs of all collected Tweets.
The corresponding Tweets for each ID can be looked up using any Tweet hydrator or you
can use NASTY's included hydrator which is compatible with the data format published
here.

Tweet hydration using NASTY
========================================================================================

1. Obtain API keys from Twitter (see `Twitter Developers: Getting Started
   <https://developer.twitter.com/en/docs/basics/getting-started>`_).

2. Download the `configuration file
   <https://raw.githubusercontent.com/lschmelzeisen/nasty/master/config.example.sh>`_,
   enter you API keys, and `source <https://ss64.com/bash/source.html>`_ it into you
   environment.

3. Install NASTY via `pip <https://en.wikipedia.org/wiki/Pip_(package_manager)>`_::

    pip install nasty

4. Hydrate Tweets to folder ``ncov-tweets/``::

    nasty unidify --in-dir ncov-tweets.ids/ --out-dir ncov-tweets/


Data format
========================================================================================

Tweet-ID are available in the ``ncov-tweets.ids/`` folder.
Here, you can find many files with the following naming structure::

    <language>-<day>-<filter>-<heyword>.ids

Each contains a newline-delimited set of Tweet-IDs in plain text that were retrieved
with the search criteria from the file name:

* **Language**: The search language that was used to retrieve Tweets.
  Note that in rare cases, Twitter's automatic language identification may
  miss-categorize a Tweet's language.

  Currently only ``en`` (English) and ``de`` (German) are included.
  If you are interested in Tweets of other languages, contact me.
  Given a list of suitable keywords, I should be able to add any language.

* **Day**: The UTC date of the contained Tweets in ``YYYY-MM-DD`` format.

* **Filter**: Either ``TOP`` or ``LATEST``.
  This refers to a Twitter-internal Tweet-ranking system.
  Tweets in ``TOP`` are likely to be of higher quality, less spammy, etc.
  On the flipside, for ``LATEST`` many more Tweets are available.

* **Keyword**: Keyword that was used to retrieve the Tweets.
  Currently the following keywords used are ``corona``, ``coronavirus``, ``covid19``,
  ``covid``, ``ncov``, and ``sars`` in both English and German.

Note that some overlap of Tweet-IDs in the respective files is to be expected.
For instance, a Tweet could mention multiple keywords.
Additionally, *most* (but not all) ``TOP`` Tweets are contained in the respective
``LATEST`` files.

For each ``*.ids`` file, you will find a corresponding ``*.meta.json`` file.
These are NASTY-internal files and you can safely ignore them.
Notably, they denote at which time each respective set of Tweets was retrieved.

Known limitations
========================================================================================

* Compared with other coronavirus Tweet datasets, relative few keywords are used.
  This was done to strike a sensible balance between `precision and recall
  <https://en.wikipedia.org/wiki/Precision_and_recall#Definition_(information_retrieval_context)>`_,
  i.e., to ensure that most collected Tweets are actually relevant to the coronavirus
  topic.

* No consistent time intervals for retrieval have been used.
  Note that this is a much small limitation than for all other methodologies, since
  NASTY allows to retrieve Tweets from the past.
  This only means, that Tweets that have been deleted before crawling are not included
  in this dataset.

* Because of our methodologies upsides, it can be expected that when controlling for
  time frame and keywords, this dataset should contain atleast as many Tweets as all
  other datasets.
  However, this hypothesis has not been verified yet.
