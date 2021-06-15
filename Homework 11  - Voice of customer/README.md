# Homework 11 : Voice of Customer

In this session, I analyze the voice of customer by use Topic Modeling. It's an interesting way to manage customer feedback in the present to find out what customers are saying about your brand on social media. However, there is a clear difficulty in stop words in Thai due to the variety of spelling errors that make a low efficiency model.   
 
1. I use Google Colab to create the voice of customer from 'CustomerReview.csv'.
2. Procedure
  - Sepearte data of each store to 3 groups by brand (1.Mo-Mo-Paradise, 2.Shabushi, 3.ข้าน้อยขอชาบู) for clustering analysis to find some insight.
  - Remove abnormal words and add new groups for similar words.
  - You can follow step-by-step from link : https://colab.research.google.com/drive/1kPVVkkLS4Tc7yVpZ2GFq1E4D2Q0FmA5-?usp=sharing
  - In the link has steps as below
    - Load libraries and data
    ```
    !pip install pythainlp
    !pip install pyLDAvis
    ```
    - Import libraries and data
    ```
    import pandas as pd
    import pythainlp
    import gensim
    import pyLDAvis
    import pyLDAvis.gensim_models
    pyLDAvis.enable_notebook()
    import warnings
    warnings.filterwarnings("ignore", category=DeprecationWarning) 
    
    df = pd.read_csv('CustomerReviews.csv')
       
    ```
    - For Mo-Mo- Paradise
    ```
    df1 = df[df.Restaurant == 'Mo-Mo-Paradise (โม โม พาราไดซ์) เดอะมอลล์ บางกะปิ']
    df1
    ```
    - Tokenize Words
    ```
    stopwords = list(pythainlp.corpus.thai_stopwords())
    removed_words = [' ', '  ', '\n', 'ร้าน', '(', ')','😆','🤣','-',':','/','เลือก','นะคะ','\u200b','++','กก','2','–','!!!!','+','xx','โมโม่','โม']
    screening_words = stopwords + removed_words

    def tokenize_with_space(sentence):
      merged = ''
      words = pythainlp.word_tokenize(str(sentence), engine='newmm')
      for word in words:
        if word not in screening_words:
           merged = merged + ',' + word
      return merged[1:]
    ```
     ```
    df1['Review_tokenized'] = df1['Review'].apply(lambda x: tokenize_with_space(x))
    ```
    - Create Dictionary
    ```
    documents = df1['Review_tokenized'].to_list()
    texts = [[text for text in doc.split(',')] for doc in documents]
    dictionary = gensim.corpora.Dictionary(texts)
    
    print(dictionary.token2id.keys())
    ```
    ```
    gensim_corpus = [dictionary.doc2bow(text, allow_update=True) for text in texts]
    word_frequencies = [[(dictionary[id], frequence) for id, frequence in couple] for couple in gensim_corpus]
    ```
 
    - Topic Modelling
    ```
    num_topics = 10
    chunksize = 4000 # size of the doc looked at every pass
    passes = 20 # number of passes through documents
    iterations = 50
    eval_every = 1  # Don't evaluate model perplexity, takes too much time.

    # Make a index to word dictionary.
    temp = dictionary[0]  # This is only to "load" the dictionary.
    id2word = dictionary.id2token

    %time model = gensim.models.LdaModel(corpus=gensim_corpus, id2word=id2word, chunksize=chunksize, \
                       alpha='auto', eta='auto', \
                       iterations=iterations, num_topics=num_topics, \
                       passes=passes, eval_every=eval_every)
    ```
    ```
    pyLDAvis.gensim_models.prepare(model, gensim_corpus, dictionary)
    ```
     - Predict Topics
    ```
    model.show_topic(1)
    ```
    ```
    df1['topics'] = df1['Review_tokenized'].apply(lambda x: model.get_document_topics(dictionary.doc2bow(x.split(',')))[0][0])
    df1['score'] = df1['Review_tokenized'].apply(lambda x: model.get_document_topics(dictionary.doc2bow(x.split(',')))[0][1])
    
    df1.tail()
    ```
    
    - After you follow the steps as above, you will get the scores of the prediction topic by use 'topic modelling'
    
 3. Conclusion
  - In this project , the NLP model has low efficiency because it can't seperate the words clearly due to each store has small data.
