# continuum
Lambda Architecture is not enough! Continuum is more fundamental

What is Lambda Architecture?
Long long ago, creator of [Apache Storm](http://storm.apache.org/index.html "Apache Storm") talked about Lambad Architecture in his article [How to beat the CAP theorem] (http://nathanmarz.com/blog/how-to-beat-the-cap-theorem.html). The idea was to combine a 'batch compute - which is reliable and *more correct*' and a' stream/realtime compute - which provides better consistency'. The resulting system will be *consistent* and *reliable* - the most desirable traits in any data system.
![](http://nathanmarz.com/storage/batch_realtime_example.png?__SQUARESPACE_CACHEVERSION=1318379033834)
