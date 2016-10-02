---
layout: post
title: Anonymization, word vectors and O(n) model
---
<center> <img src="http://archive2.cra.org/ccc/files/images/privacy.jpg" alt="alt text" width="800px"> </center>
Automated anonymization of documents is one important research subject, especially in the medical area where [creteria](http://www.hhs.gov/hipaa/for-professionals/privacy/special-topics/de-identification/) are strictly guided. In general, it is a difficult task, and even more so for data outside of medical domain due to the lack of specialized tools. In this post, I explored the possibility of using the [vector embedding](https://papers.nips.cc/paper/5021-distributed-representations-of-words-and-phrases-and-their-compositionality.pdf) [Mikolov 13] approach to anonymize non-medical documents. I arrived at some interesting results with hints from theoretical physics, which I would like share about. 


### The project: anonymize text documents

Recently, I worked on a consultant project as a [Insight Data Science](http://insightdatascience.com/) fellow, partnered with [Bonfire](http://gobonfire.com), a company based in Ontario, whose main product is a website that helps organizations (universities, hospitals, etc) to setup their procurement processes. Public purchasing often involves some very tedious and tiresome procedures, especially the very early step of preparing a bunch of documents (in jargons: [Request For Proposals](https://en.wikipedia.org/wiki/Request_for_proposal), or RFPs). Such a pain can be partially reduced if existing documents on similar purchases can be reused by different users. However, for privacy concern, this requires user identifying information in these documents to be removed. My major goal is to **anonymize the approximately 200,000 documents** produced by a couple hundres of users. 

It is by no means a simple task as search-and-replace. In writing the documents, users tend to refer themselves in various ways that cannot be easily inducted. For example, users may mention their building names, street names or even years they were founded, like in the example below.

<center> <img src="{{ site.baseurl }}/images/utopia.png" alt="alt text" height="120px"> </center>

Name entity recognition seems to be a good start. However, entity name tagging method is not working very well for documents consisting of unnatural flows of sentences, such as the FAQ style of writing that most of my documents is in. Futhermore, private information is mare than just entities. 

It boils down to how we define private inforamtion. This is how "privacy" is [defined on Wikipedia](https://en.wikipedia.org/wiki/Privacy): 

> Privacy is the ability of an individual or group to ***seclude*** themselves, ...

In other words, what we are after is the information that exclusively belongs to an given entity, but not others.

### Word Vectors

The closeness of information to an entity can be naturally quantified through [word vectors](https://papers.nips.cc/paper/5021-distributed-representations-of-words-and-phrases-and-their-compositionality.pdf) in the skip-gram model [Mikolov 13]. The algorithm trains a vector representation of the vocabulary by binding the context (neighboring words) through a negative log-likelyhood (with negative sampling)

$$
l(w) =  \sum_{c} \big[ \log(1+ e^{-\mathbf{v}_w \cdot \mathbf{v}_c}) + \sum_{n\in \mathcal{N}_c}\log(1+e^{ \mathbf{v}_w\cdot \mathbf{v}_n}) \big ]
$$




