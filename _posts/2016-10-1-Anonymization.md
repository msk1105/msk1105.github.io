---
layout: post
title: Anonymization, word vectors and O(n) model
---
Automated anonymization of documents one important research subject, especially in medical data where [strict guidance](http://www.hhs.gov/hipaa/for-professionals/privacy/special-topics/de-identification/) is enforced. In general, it is a difficult task, and even so for data outside of medical domain due to the lack of specialized tools. In this post, I explored the possibility of using the vector embedding [Mikolov 13] approach to anonymize non-medical documents. I arrived at some interesting results and some hints from theoretical physics, which I would like share about. 


### The project: anonymize text documents

Recently, I worked on a consultant project, partnered with Bonfire, a company based in Ontario. Their main product is a website that helps organizations (universities, hospitals, etc) to setup their procurement processes. Public purchasing typically involves some very tedious and tiresome procedures, especially the very early step of preparing a bunch of documents (in jargons: Request For Proposals, or RFPs). Such a pain can be partially reduced if existing documents on similar purchases can be reused by different users. However, for privacy concern, this requires user identifying information in these documents to be removed. So this is the main problem I am trying to address: to **anonymize about 200,000 documents** produced by a couple hundres of users. 

### A different approach: word vectors

It is by no means a simple task as search-and-replace. In writing the documents, users tend to refer themselves in various ways that cannot be easily inducted. For example, users may mention their building names, street names or even years they were founded. This information, in the context, can be easily used to identify the users. 

<center> <img src="{{ site.baseurl }}/images/utopia.png" alt="alt text" height="100px"> </center>

Name entity recognition seems to be a good start. However, I found entity name tagging method did not work very well for documents consisting of unnatural flows of sentences, such as the FAQ style that most of the documents I am dealing with is in. Futhermore, private information is mare than just entities. 

It boils down to how we define private inforamtion. This is how "privacy" is [defined on Wikipedia](): 

> Privacy is the ability of an individual or group to ***seclude*** themselves, or information about themselves ...





