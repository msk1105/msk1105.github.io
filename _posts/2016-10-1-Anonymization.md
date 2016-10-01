---
layout: post
title: Anonymization, word vectors and O(n) model
---
Automated anonymization of documents one important research subjects related to medical data where [strict guidance](http://www.hhs.gov/hipaa/for-professionals/privacy/special-topics/de-identification/) is enforced. In general, it is a difficult task, especially for data outside of the domain of medical records due to the lack of specialized tools. In this post, I explored the possibility of using the vector embedding [4,5] approach for the anonymization of non-medical documents. I found some interesting results and some hints from theoretical physics, which I would like share about. 


### The project: anonymize text documents

Recently, I worked on a consultant project, partnered with Bonfire, a company based in Ontario. Their main product is a website that helps organizations (universities, hospitals, etc) to setup their procurement processes. Public purchasing typically involves some very tedious and tiresome procedures, especially the very early step of preparing a bunch of documents (in jargons: Request For Proposals, or RFPs). Such a pain can be partially avoided if previous documents on similar purchases can be reused by different users. However, this is only feasible if the user identifying information in these documents is removed. This is the main problem I am trying to address: to **anonymize about 200,000 documents** produced by a couple hundres of users. 

### New approach: word vectors

It is by no means as simple as search and replace. In writing the documents, users tend to refer themselves in various ways that cannot be easily inducted. 

<img src="{{ site.baseurl }}/images/utopia.png" alt="alt text" height="40px">

![_config.yml]({{ site.baseurl }}/images/utopia.png)


