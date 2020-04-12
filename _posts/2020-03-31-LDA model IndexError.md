---
layout: post
title: "LDA model IndexError: index 4963 is out of bounds for axis 1 with size 4963"
date: 2020-3-31
categories:
  - blog
description:Some problem encounterd while using LDA model.s
image: https://note.youdao.com/yws/api/personal/file/WEB5986828890383f08c9e80ac68f1bb750?method=download&shareKey=74123af89e050b12894d7079c6db5edc
image-sm: https://note.youdao.com/yws/api/personal/file/WEB5986828890383f08c9e80ac68f1bb750?method=download&shareKey=74123af89e050b12894d7079c6db5edc
---

# LDA model IndexError: index 4963 is out of bounds for axis 1 with size 4963

Today I'm running a LDA model in order to find the dominant topic in each sentence.

```
   for i,row in enumerate(lda_model[corpus_in_address]):
        for j,(topic_num,prop_topic) in enumerate(row):
            if j==0:
                wp = lda_model.show_topic(topic_num)
                topic_keywords=",".join([word for word, prop in wp])
                print(topic_num)
                print(prop_topic)
                print(topic_keywords)
                sent_topics_df = sent_topics_df.append(pd.Series([int(topic_num), round(prop_topic, 4), topic_keywords]), ignore_index=True)
            else:
                break
```
However I got this error

`
IndexError: index 4963 is out of bounds for axis 1 with size 4963
`
I'm really confused. and for a long time I can't find out why. Then I read something in [pyLDAvis](https://github.com/bmabey/pyLDAvis/issues/72),[stackoverflow](https://stackoverflow.com/questions/26812617/index-error-when-running-lda-in-gensim). And I realized that might because

```
dictionary = gensim.corpora.Dictionary(processed_docs_in_address)
dictionary.filter_extremes(no_below=15, no_above=0.5, keep_n=100000)
bow_corpus = [dictionary.doc2bow(doc) for doc in processed_docs_in_address]
lda_model = gensim.models.LdaMulticore(bow_corpus, num_topics=34, id2word=dictionary, passes=2, workers=2)

```

I adjusted the dictionary, and I created the corpus afterwords, that means some words in the corpus can't find its index.  
It turns out that really is the reason:)