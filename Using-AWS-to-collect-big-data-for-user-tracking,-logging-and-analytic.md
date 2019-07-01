Have you ever wondered how Google Analytics can build a dashboard which is so awesome. It can report you everything from page views, daily users, real-time active users, time on-site, … 

That’s also our question when trying to build an analytics tracking/logging system as a component of our recommendation system.

In this post, we will show you how we build it and what we have learnt from Google Analytics.

## Problems

Now, let’s take a look on our problem. By requirement, we had to build:

- An analytics tracking/logging system
- A simple ETL (Extract, Transform, Load) to DB

Both of them required stable, high available and keep in mind that the highest priority is the system must be as cheap as possible.

Until here, you may want to ask, analytics??? What we analyze for?

The answer is user behavior, like what Google Analytics do, a dashboard for user behavior on site. The destination site is our partner, where we put our script on, it’s eva.vn.

We built a cross recommendation system for eva.vn and alezaa book/magazine (see [here](http://blog.richanchor.com/2016/01/04/cross-domain-recommendation-system-in-rich-anchor)). An analyzed data dashboard on user behaviour will show how good it is. That's why a dashboard is so important.

## Looking for a solution

As mentioned above, our problem is very similar to Google Analytics which is perfectly built by Google. Therefore, the idea is to follow Google’s footsteps.

To setup Google Analytics, Google requires you to add analytics.js to Your Site. Such page also explains more detail about what the script does. However we could move forward quickly by turning on developer mode on browser and open http://vnexpress.net/

![](https://blogdotrichanchordotcom.files.wordpress.com/2016/02/screen-shot-2016-02-26-at-1-21-35-am.png)

As you can see, there is a GET request that was sent to https://www.google-analytics.com/r/collect
Let’s see what is it

```
$ curl -s -I https://www.google-analytics.com/r/collect | grep &amp;amp;quot;^Content&amp;amp;quot;
Content-Type: image/gif
Content-Length: 35
```

So, it’s a GIF image file which have size is 35 bytes.

In fact, it’s a 1 x 1 pixel GIF

![](https://blogdotrichanchordotcom.files.wordpress.com/2016/02/screen-shot-2016-02-26-at-1-32-32-am.png)

There is a GET request with several parameters, that means in server side, Google web server will collect a log line for such request and then post-process it later. Data from logs will be extracted, transformed then loaded to a DB which GA Dashboard query to get data.

![](https://blogdotrichanchordotcom.files.wordpress.com/2016/02/gatcprocessing1.png)

That’s a brief of what GA does to collect data. Here is what we have learnt from GA:

 - (1) Add a script to partner site
 - (2) Send a GET request to an 1 pixel image/gif URL. This will  bring all tracking data in parameters
 - (3) Extract web server’s log, transform, then load to DB

For those who are familiar with Amazon web service, we always know that Amazon CDN service: CloudFront is awesome because its price and quality. CloudFront also required S3 as a storage for static data (image, html, …), and fortunately, it’s is extremely cheap too.

## Implementation

So, step by step:

- Get google GIF, rename as you want, put the gif file in your S3

- Create a new Distribution from CloudFront:
  + point it to the above S3 bucket/prefix
  + enable logging for this distribution
  + take another S3 bucket for log

That’s it, our tracking system is ready to use.
From the script on eva, we can send GET request to CloudFront, get log from S3. Next step is build a small ETL (extract, transform, load) for access logs and put them to DB.

As you may know, eva is a big site which got a huge number of requests daily. At the moment,  we are trying to store all the logs from each page view. It’s an enormous number for a DB, so we should consider about the price for the server and DB . Fortunately, after days of searching, we decided to use SimpleDB because it can solve our problem perfectly since performance is not a must in this problem. Another reason is that SimpleDB is super cheap, much cheaper than other DB services.

We don’t go deep to explain how we build ETL solution here, it’s not difficult when we have all above things. Amazon AWS is great in API for all services in some popular programming languages.

Now, the next task is just some easy coding… So let’s go!

References:

https://aws.amazon.com/cloudfront/pricing/

https://aws.amazon.com/s3/pricing/

https://aws.amazon.com/simpledb/pricing/