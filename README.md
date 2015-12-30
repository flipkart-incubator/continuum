# continuum
Lambda Architecture is not enough! Continuum is more fundamental

## What is Lambda Architecture?
Long long ago, creator of [Apache Storm](http://storm.apache.org/index.html "Apache Storm") talked about Lambad Architecture in his article [How to beat the CAP theorem] (http://nathanmarz.com/blog/how-to-beat-the-cap-theorem.html). The idea was to combine a 'batch compute - which is reliable and *more correct*' and a' stream/realtime compute - which provides better consistency'. The resulting system will be *consistent* and *reliable* - the most desirable traits in any data system.

![Lambda Architecture](http://nathanmarz.com/storage/batch_realtime_example.png?__SQUARESPACE_CACHEVERSION=1318379033834)

## If Lambda Arch is the solution, what is the problem?
The blog explains the need for Lambda Arch through availability and consistency as trade-offs. But practically, the data systems need to make *freshness* and *correctness* trade-offs. The freshness is defined as "how realtime are the results of the computation/processing" and correctness is defined as "how accurate the results are". These tradeoffs arise because of finite storage and compute capacity. 

Data can arrive unordered and late. A recent, but a small window of data cannot portray the 'complete' reality. As such, a realtime system's accuracy is limited by the avilability of 'complete data' within a recent, smaller window. For the same reason, a batch system is more accurate and less fresh because a larger window of data.

Lambda Architecture makes a discreet choices for freshness and correctness; infact it picks up 2 points to balance the freshness and correctness. The problems with this choice are
* most often the stream and batch are 2 different stacks and hence 2 definitions of the same processing. This leads to maintenance and operational overheads. For example, 2 different unit tests, larger upgrade cycles whenever there is a change in data formats, resource wastage?
* The 2 pipelines have to be tuned/configured so that they have don't leave gaps (ie data is lost because of lack of ownership).



