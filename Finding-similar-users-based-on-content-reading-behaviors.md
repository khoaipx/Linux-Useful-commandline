In our Recommender System, we have an interesting problem. User A has similar reading behavior with many users, such as user B. In other words, A tends to choose some in documents which B read. Our task is finding out B and recommending documents in B’s reading list to A. Our new idea to solve this problem was came from [our old post](http://blog.richanchor.com/2016/01/22/context-based-similar-documents/). We have tried it and received very positive results. Furthermore, we found that we could also apply it in many practical applications.
## Old approach for a new problem
In the [previous post](http://blog.richanchor.com/2016/01/22/context-based-similar-documents/), we proposed a solution to find contextually similar documents for an input document using topic modeling techniques. In order to do that, We built a [Latent Dirichlet Allocation (LDA)](https://en.wikipedia.org/wiki/Latent_Dirichlet_allocation) model with **D** topics for all documents in database, then vectorized these documents using the generated model. As mentioned, in LDA model, we assume each document is a set of words that are derived from a mixture of topics. Therefore, each document has a probability generated by each topic. So applying LDA model on a document, we receive a **D**-dimensional vector (This is the probabilistic distribution of **D** topics that generate the selected document). In other words, we attempt to transform a document into a vector. The result we received is **N** vectors of **D** dimensions or **N** points in a **D**-dimensional space (**N** is the number of documents in database). Assume that 2 close points represent 2 similar documents, finding similar documents problem becomes a nearest neighbors searching problem. 
In practical, it is very hard to find the extract nearest neighbors because the number of topics for news is often large (**D** >= 50). One solution is using the approximate nearest neighbor searching techniques. Local Sensitive Hashing (LSH) is one of them. After vectoring process, we run LSH to index documents in database. For each query input document, we vectorize this document with LDA to transform it into a point in space, then run LSH to find **k** nearest neighbors. Finally, we remap received points to get similar documents of the input one.
This pretty much sums up what we had done in order to find the similar documents of an input text. Note that each document in the training dataset of LDA is an item in our database. Imagine that each input document is a reading pattern of a user, we could find the most similar users based on their reading context. 
We achieved this by making a small modification in our system. Instead of using single document as the training input, we combine the reading context of a user and then use it as the training input for LDA. Of course, we must assume that similar users tend to have similar reading contents.
![](https://blogdotrichanchordotcom.files.wordpress.com/2016/03/finding-similar-users-based-on-their-reading-behavior-figure-2.jpg?w=656)
This figure shows the procedure to find similar users in our system. First of all, we need to train the LDA model using the user context as the input. As the result, we receive the user vector data (the vectorized user context). Indexing the user vectors using LSH, we could collect the indexed user vector.
For each new “user context” query, we run through the similar process to find the vector of that user. Finally, we attempt to find k similar users using the retrieved indexed vectors from previous steps and LSH
Application and More
The first application is an user-based recommendation. If we have two similar users: A and B, we could recommend books (articles) in A's reading list for B and vice versa.
Other application is recommending a new document to user when its content is similar with what  he had read. Basically, we can gather all of documents that he had read into a bigger document. Now, our problem becomes finding the most similar “documents” for a new document as the previous post. This method could also be applied to measure (heuristically) the number of users that have interest in a new article.
Moreover, when we know “the distance” between users and the nearest neighbors of a certain user, we can build a connective graph for a set of users or a similarity matrix. They are the inputs for several graph clustering algorithms. We will talk more about clustering problem for users or documents in next posts.
Experiment
As usual, we will show the application of our method in a real world data. We collected random samples on website eva.vn. We created 20,000 users with unique id, and read some news in 5 categories of [eva.vn](http://eva.vn/) website: [Eva tám](http://eva.vn/eva-tam-c66.html), [Làm đẹp](http://eva.vn/lam-dep-c58.html), [Bà bầu](http://eva.vn/ba-bau-c85.html), [Làm mẹ](http://eva.vn/lam-me-c10.html), [Tình yêu giới tính](http://eva.vn/tinh-yeu-gioi-tinh-c3.html)

You can see the result here: http://ra2v.richanchor.com/usersim/?f=text&uid=-7799014149319453873

You can see result of other users when changing uid query param, all of the available uids are stored here:
* [list users](https://drive.google.com/file/d/0B0tlVqWS0XzXMFROVGQ2dWVOekU/view?usp=sharing)

Besides, with second application, when we input an url's content or just content, we can predict which users will be interested in.(We have mentioned about consumer prediction before, that version is built based on categorical data only, now we can build a new model based on the content. Hopefully this could help content providers to measure the popularity of a document before publishing it)

We can input an url (now we support only for eva.vn's url) http://ra2v.richanchor.com/usersim/?f=text&url=http://eva.vn/tu-van-tinh-yeu/dung-nghe-nhung-gi-phu-nu-noi-c273a218267.html

Or we can input text: http://ra2v.richanchor.com/usersim/?f=text&input=c%C3%A1c%20m%C3%B3n%20%C4%83n%20ng%C3%A0y%20t%E1%BA%BFt%20%E1%BB%9F%20vi%E1%BB%87t%20nam%20r%E1%BA%A5t%20ngon

Similarly, you can change URL variable in the query param into a different Eva’s URL or give an input with input query param type and find the result yourself.
Note: the context that we used are crawled from 5 categories on eva.vn and we only have the content parser for Eva.vn. Don’t try links from different websites, our service will not work properly. And please don't spam, the service is just for demo test, we'll close immediately if it hurts our server's performance.

In summary, methodologically, it is not new, but the received results are very useful for our Recommender System.
P/S: I wrote a small post on my personal blog about [implementing Context-based similar for documents with Python](https://dashdotdash.wordpress.com/2016/03/04/implementing-context-based-similar-documents-with-python/). If you consider coding aspect, you can read it and try with your data.