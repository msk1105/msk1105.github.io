---
layout: post
title: "Anonymization, word vectors and O(n) model"
---
<center> <img src="http://archive2.cra.org/ccc/files/images/privacy.jpg" alt="alt text" width="800px"> </center>
Automated anonymization of documents is one important research subject, especially in the medical area where [creteria](http://www.hhs.gov/hipaa/for-professionals/privacy/special-topics/de-identification/) are strictly guided. In general, it is a difficult task, and even more so for data outside of medical domain due to the lack of specialized tools. In this post, I explored the possibility of using the [vector embedding](https://papers.nips.cc/paper/5021-distributed-representations-of-words-and-phrases-and-their-compositionality.pdf) [Mikolov 13] approach to anonymize non-medical documents. I arrived at some interesting results with hints from theoretical physics, which I would like share about. 


### The project: anonymize text documents

Recently, I worked on a consultant project as a [Insight Data Science](http://insightdatascience.com/) fellow, partnered with [Bonfire](http://gobonfire.com), a company based in Ontario, whose main product is a website that helps organizations (universities, hospitals, etc) to setup their procurement processes. Public purchasing often involves some very tedious and tiresome procedures, especially the very early step of preparing a bunch of documents (in jargons: [Request For Proposals](https://en.wikipedia.org/wiki/Request_for_proposal), or RFPs). Such a pain can be partially reduced if existing documents on similar purchases can be reused by different users as templates. However, for privacy concern, this requires user identifying information in these documents to be removed. My major goal is to **anonymize the approximately 200,000 documents** produced by a couple hundres of users. 

It is by no means a simple task as search-and-replace. In writing the documents, users tend to refer themselves in various ways that cannot be easily inducted. For example, users may mention their building names, street names or even years they were founded, like in the example below.

<center> <img src="{{ site.baseurl }}/images/utopia.png" alt="alt text" height="120px"> </center>

Name entity recognition seems to be a good start. However, entity name tagging method is not working very well for documents consisting of unnatural flows of sentences, such as the FAQ style of writing that most of my documents is in. Futhermore, private information is more than just entities. 

It boils down to how we define private inforamtion. Here is how "privacy" is [defined on Wikipedia](https://en.wikipedia.org/wiki/Privacy): 

> Privacy is the ability of an individual or group to ***seclude*** themselves, ...

In my special case, what I am after is the information that exclusively belongs to a particular user, but not others. I need to find a way to segementate the information within a document according the closeness to the user. 

### Word Vectors

The closeness of information to a given word can be naturally quantified through [word vectors](https://papers.nips.cc/paper/5021-distributed-representations-of-words-and-phrases-and-their-compositionality.pdf) in the skip-gram model [Mikolov 13]. The algorithm trains a vector representation of the vocabulary by binding the context (neighboring words). For any word $w$ and its context $c$, this is can be achieved by minizing the negative log-likelyhood

$$
\begin{equation} \label
l(w) =  \sum_{c} \big[ \log(1+ e^{-\boldsymbol{v}_w \cdot \boldsymbol{v}_c}) + \sum_{n\in \mathcal{N}_c}\log(1+e^{ \boldsymbol{v}_w\cdot \boldsymbol{v}_n}) \big ]
\end{equation} 
$$

where $\boldsymbol{v}$ is a mapping (that we are trying to learn) from the vocabulary to a vector space. The second term in the squared brackets is the negative sampling where $\mathcal{N}_c$ are the set of random negative samples that are not $c$. It mostly serves like a regularization term. 

The context binding here provides a way to separate the private words from common words. This can be visualized in the cartoon below. Intuitively, when a common word shows up in the multiple contexts, the binding will pull it away from from any of the contexts, as a compromise to minimize the negative log-likelyhood. As a result, only words that are unique will be binded closer in the vector space. 

<center> <img src="{{ site.baseurl }}/images/wordvec.gif" alt="alt text" width="800px"> </center>

We can also look at an alternative form of Eq. (1). If we ingore the negative sampling term for a moment, restrict $c$ to be strictly the next word of $w$ and restrict the embedding $\boldsymbol{v}$ is only a unit vector,  Then 
 
$$
\begin{align}
&& \arg \min \sum_w \big[ \log(1+ e^{-\boldsymbol{v}_i \cdot \boldsymbol{v}_{i+1}}) \big]   \\
& = & \arg \min \sum_i \big [ \log( e^{-\boldsymbol{v}_i \cdot \boldsymbol{v}_{i+1}}) \big ] \\
& = & \arg \min (-\sum_i \boldsymbol{v}_i \cdot \boldsymbol{v}_{i+1} ) 
\end{align}
$$ 

This is nothing but the one-dimensional [O(n) model](https://en.wikipedia.org/wiki/N-vector_model) in statistical physics! The O(n) model or n-vector model is a simplified physics model that explains how macro magnect effect is formed as an effect of grids of tiny magnects (or spins) lining up together. Strong magnect field is formed if all the underlining individual tiny magnets are orientated in the same direction and otherwise if they are randomly orientated. Every tiny magnet is affected by and, at the same time, affecting its neighbors. The physics system will settle around the configuration where the total energy (defined by Eq. (2)) is minimum. Our problem at hand is very similar: we are trying to find a configuration of all the word vectors such that the total "energy" is minimum. 

<center> <img src="{{ site.baseurl }}/images/nvectors.png" alt="alt text" height="80px"> </center>


### The implementation using FastText

[FastText](https://github.com/facebookresearch/fastText) is a open source tool developed by [Facebook AI](https://research.facebook.com/ai/) which is based on the skip-gram model we just discussed, but adding an important feature that is highly relevant to our interest here: the subword information, based on [an algorithm](https://arxiv.org/pdf/1607.04606v1.pdf) they recently published. Its implication here is two-folded. One, it largely reduces the occassions of out-of-vocabulary. Second, it connects morphologically similar words or even typos. It in part achieves the functionality of "regular expressions", which is crucial for our anonymization task. 

I simply concated the entire set of documents into one hot word vector, then feeded it to fastText to learn the vector representation. 


### An improvement: PolarizedText



