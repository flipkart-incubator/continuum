# continuum
Lambda Architecture is not enough! Continuum is more fundamental

## What is Lambda Architecture?
Long long ago, creator of [Apache Storm](http://storm.apache.org/index.html "Apache Storm"), Nathan Marz, talked about Lambda Architecture in his article [How to beat the CAP theorem] (http://nathanmarz.com/blog/how-to-beat-the-cap-theorem.html). Idea was to combine a 'batch compute - which is reliable and *more correct*' and a 'stream/realtime compute - which provides better consistency'. The resulting system will be, hypothetically, *consistent* and *reliable* - traits desired in any data system.

![Lambda Architecture](http://nathanmarz.com/storage/batch_realtime_example.png?__SQUARESPACE_CACHEVERSION=1318379033834)

## But??
The blog presents Lambda Arch as a solution to balance availability vs consistency trade-offs. Practically, data systems need to make *freshness* vs *correctness* trade-offs. **Freshness** is defined as "how realtime are the results of the computation/processing". **Correctness** is defined as "how accurate and complete the results of a processing are". These tradeoffs arise because of finite storage and compute capacity. The freshness and correctness properties are fundamental properties of a data system whereas 'availability' and 'consistency' are properties of the solution's architecture. 

Now, data can arrive unordered and late. A recent, but a small window of data cannot portray the 'complete' reality. As such, a realtime system's accuracy is limited by the avilability of 'complete data' within its smaller operating window. Using a similar reason, a batch system is more accurate (and less fresh) because a larger window of data.

![Batch vs Stream](https://github.com/flipkart-incubator/continuum/blob/master/docs/images/continuum-stream-vs-batch-lambda.jpg)

Lambda Architecture makes a discreet choices for freshness and correctness; infact it picks up 2 points to balance the freshness and correctness. There are problems with above choice:
* Most often the stream and batch are 2 different stacks and hence 2 definitions of the same processing. This leads to maintenance and operational overheads.
* The 2 pipelines have to be tuned/configured to ensure that each event is processed at-least once *as and when it is received* by the respective pipeline. Example, assume that we need to join 2 streams of events e1 and e2. Suppose e1 stream stops for arbitrary amount of time. In such a case we cannot process events from stream e2 and have to wait for batch systems to pick up the processing. If e1 stream has not recovered by that time, then we would miss processing of events from e1 stream entirely. (This is depicted in above figure by regions D1 and D2; if D1 < D2, then any event that arrives after D1 duration, might not be processed by stream pipeline)
* Re-runs are a big issue as one need to manually figure out which pipeline is required to be re-run based on the duration and the age of the data.
* The entire system needs to be reconfigured if we need to change W1, W2 parameters for stream and batch pipelines respectively. More generically, since the system is designed to be pivoted on 2 distinct points, it cannot tolerate presence of more pipelines working at different parameters (W and S) - which might be the case if we have resource crunch. We can engineer the *Query* system to merge results from more than 2 pipelines; but we still need to manage an entirely new data pipeline as and when they are introduced.

## [WIP] Introducing...
Continuum. Instead of solving the 'freshness' and 'correctness' problem by 2 different systems, we take it as a tradeoff technology needs to make all the time.
Continuum is designed to be a technology solution rather than an architecture reference.


### Problem with Message Queues/Relays
Mostly operational rather than scale. One which affects us most is the failure recovery. When there is a consumer failure, most often than not, messages pile up and it is essential that we recover the newest changes first and either drop the older changes or take a snapshot from the source or process the older changes in background. As such *time* become one of the dimension to track in message queues (along with message offsets).


### Problem with Data Location
In Lambda architecture, data can be present in the storage layer of either or both pieplines, but we cannot be sure. The data storage tech used in the stream and batch pipelines are very different and usually there is no meta data to establish relationship between them. It makes pipeline recovery inefficient (typically near-realtime pipeline) as one need to decide if they really need to process some part of data which could have already been processed by batch pipeline. Another case, where we need to reloadphysically copy data between data stores to re-process in case a bug was discovered and fixed at a later point in time.






