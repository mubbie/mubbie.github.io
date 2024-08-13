# Broadcasting in Spark 📣

<p align="center">
  <img src="https://wizardzines.com/images/uploads/scenes-from-distributed-systems.jpg" alt="Distributed Systems">
</p>

<p align="center">
  <em> Image Source: <a href="https://wizardzines.com/">wizard zines</a> </em>
</p>

## Introduction
You might have started reading this blog post with some curiosity. For one, the title might have left more questions than answers unless you're well-versed in big data analytics or distributed systems. Let's begin by introducing the concept of distributed systems in computing. From there, we'll explore `broadcasting` in these systems and finally dive into the specifics of `broadcasting` in Spark. I hope you enjoy the journey.

### Distributed Computing Systems

Imagine you love to cook, and your friends always rave about your dishes. Encouraged by their compliments, you decide to open your restaurant. Initially, you're the only chef handling everything yourself. But as word spreads, business booms and you quickly realize you can't manage it alone. You hire more chefs and expand your kitchen to keep up with demand.

Now, on a busy evening, your kitchen is abuzz with activity. Each chef works at a different station—one handling appetizers, another on main courses, and so on. Together, they ensure that every dish is prepared perfectly and served on time. This setup is much like a distributed system in computing:

- **Chefs as `Nodes`**: Each chef in your kitchen is like a `node` in a distributed system, working independently yet contributing to the same larger goal.
- **Division of Labor/Stations as Subsystems**: Different dishes are prepared at different stations, just as `tasks` in a distributed system are divided among various `nodes.`
- **Coordination**: As the head chef, you ensure that all stations work in sync, much like how a distributed system `coordinates` the work of its `nodes` to produce the final `output.`
- **`Parallel` Processing**: Multiple dishes are prepared simultaneously at different stations, just as tasks are processed in `parallel` across multiple `nodes` in a distributed system, improving speed and efficiency.
- **Fault Tolerance**: If a chef burns a dish, the kitchen adapts without disrupting the entire operation. Similarly, a distributed system continues to function smoothly even if one or more `nodes` fail.

Just as your restaurant thrives by distributing the workload across multiple chefs, distributed computing systems efficiently handle large tasks by dividing them across multiple computers. The benefits of distributed computing are tremendous, many of which you can read about in this [AWS Article](https://aws.amazon.com/what-is/distributed-computing), this [IBM Article](https://www.ibm.com/docs/en/txseries/8.2?topic=overview-what-is-distributed-computing), or this [Geeks for Geeks Post](https://www.geeksforgeeks.org/what-is-distributed-computing/). Now, onto `broadcasting.` 

### Broadcasting (As it applies to Distributed Computing)

After several months of running your restaurant smoothly with a trusted team of chefs, you take a well-deserved break. Your travels take you to Europe, where one of your stops is the historic Bletchley Park in Milton Keynes, renowned for its pivotal role in World War II. Bletchley Park was where brilliant minds, including Sir Alan Turing, Gordon Welchman, Joan Clarke, and many others, worked tirelessly to crack the German Enigma code. The complexity of these codes meant that a single person, or even a small team, couldn’t break them quickly enough to be effective. So, the [Government Code & Cypher School (GC&CS)](https://www.gchq.gov.uk/section/history/our-origins-and-wwi) assembled a large team of experts, creating what was effectively a distributed system.

At Bletchley Park, each codebreaker worked simultaneously on different parts of the problem, much like `nodes` in a distributed computing network. But before they could begin their work, they needed to be equipped with critical information—cipher keys, known plaintexts, and the latest intelligence. This process of equipping every codebreaker with the necessary data is akin to `broadcasting` in distributed computing. Once they had this information, they could work independently, much like how `nodes` process tasks in `parallel.` The distributed system at Bletchley Park, powered by the efficient `broadcasting` of information, was crucial in breaking the Enigma code and contributing significantly to the Allied victory.

In distributed computing, `broadcasting` refers to the process of sending a piece of data to all `nodes` in a `cluster,` allowing them to use the data locally without requiring repeated network communication. This approach minimizes communication overhead and enables more efficient computation, much like how the codebreakers at Bletchley Park worked more effectively with the information they were given.

## Broadcasting in Spark

[Apache Spark](https://spark.apache.org/) is a powerful open-source distributed computing system designed for fast data processing on large-scale datasets. It is widely used in big data analytics because it performs complex computations quickly and efficiently across a `cluster` of computers. Spark builds upon the [Hadoop](https://hadoop.apache.org/) ecosystem and can work seamlessly with the [Hadoop Distributed File System (HDFS)](https://hadoop.apache.org/docs/r1.2.1/hdfs_design.html). HDFS is a distributed storage system that allows Spark to store and access large datasets across multiple `nodes` in a `cluster.` This distributed storage capability is crucial for handling massive volumes of data, enabling Spark to process them in `parallel` across many machines.

However, when working with large datasets and complex computations, efficient data sharing across the `nodes` becomes essential. This is where the concept of `broadcasting` comes into play. In Spark, `broadcasting` is a mechanism that allows a small dataset or a piece of data to be efficiently shared with all the `nodes` in the `cluster.` Instead of sending the same data multiple times to different `nodes,` Spark `broadcasts` the data once to all `nodes.` Each `node` then stores a local copy of the `broadcasted` data, allowing it to access the data quickly without repeatedly fetching it from the driver or another central location. Let's consider `join` operations in Spark as an example of how this works.

#### Shuffling: The Default Approach for Joins

In distributed computing, particularly in Spark, one of the critical operations is `joining` datasets. Spark uses a technique called `shuffling` when performing certain operations on datasets, such as `joins,` grouping, and aggregations. Shuffling redistributes the data across different `nodes` in a Spark `cluster,` grouping data elements that need to be processed together on the same `node.` For instance, in a `join` operation, shuffling ensures that matching keys from different datasets are brought together in the same partition, allowing the `join` to be executed. 

While shuffling is effective in ensuring that data is correctly aligned for operations like `joins,` it comes with significant downsides, mainly when dealing with large datasets:
- **Increased Network Traffic**: Shuffling involves redistributing data across the network, which can substantially increase network traffic. This can be particularly problematic for large datasets, where the volume of data being transferred can become a bottleneck.
- **High I/O**: As data is shuffled, it often needs to be written to and read from the disk, increasing disk I/O operations. This further slows the computation, especially if the disks are not fast enough to handle the load.
- **Resource Intensity**: Shuffling is resource-intensive, requiring significant computational power and memory. This can lead to longer processing times and increased costs, especially in cloud environments where resources are billed based on usage.

#### Broadcasting: A More Efficient Alternative

To mitigate the downsides of shuffling, especially for `join` operations involving a large dataset and a smaller one, Spark employs the concept of `broadcasting.` `Broadcasting` is particularly useful when one of the datasets in a `join` operation is small enough to fit into the memory of each `node.` Instead of shuffling both datasets to bring matching keys together, Spark `broadcasts` the smaller dataset to all `nodes` in the `cluster.` Here’s how it works:

Imagine Spark needs to `join` a large dataset with a much smaller one. The large dataset is already distributed across the `cluster's` `nodes.` If Spark used shuffling, it would have to redistribute the data in large and small datasets, grouping matching entries together. This would involve significant network transfer and could be highly inefficient.

Instead, with `broadcasting,` Spark sends a single copy of the smaller dataset to each `node.` Each `node` then performs the `join` locally, using its portion of the large dataset and the `broadcasted` small dataset. This approach yields advantages in reduced network traffic (large amounts of data are not shuffled across the network; only the small dataset is `broadcasted,` resulting in a reduction in the amount of data that needs to be transferred), faster computation (as each `node` can perform the `join` operation locally without needing to wait for data from other `nodes` and without the need for extension disk I/O, as the data is already in memory), efficient resource utilization (as the need for shuffling is eliminated, `broadcasting` reduces the strain on computational and memory resources; reducing the overall cost of running Spark jobs).

### More on Broadcasting in Spark

It is vital to appropriately leverage `broadcasting` to truly make Spark computations more efficient. When performing operations such as `joins` or aggregations, where a small dataset needs to be applied to a large one, using Spark’s `Broadcast` variables can significantly improve performance. Spark automatically distributes these `broadcast` variables to all the `nodes,` which remain cached in memory on each `node,` allowing for fast access. Developers can create `broadcast` variables using the [`SparkContext.broadcast()`](https://spark.apache.org/docs/3.1.3/api/python/reference/api/pyspark.SparkContext.broadcast.html) method, ensuring the data is distributed efficiently across the `cluster.` You can learn more about using `broadcasting` in spark operations with these articles: [Broadcast variables in spark work like they sound](https://medium.com/@ARishi/broadcast-variables-in-spark-work-like-they-sound-c08097b80ac4), [Understanding `Broadcast` Variables In Apache Spark](https://medium.com/@Prashank.jauhari/understanding-broadcast-variables-in-apache-spark-8233d35726fc). 

### Automatic Broadcasts and the Spark Broadcast Threshold in Spark

Spark further simplifies the use of `broadcasting` by automatically determining when to `broadcast` small datasets during certain operations like `joins.` By default, Spark automatically `broadcasts` any dataset that is smaller than a predefined threshold, known as the `broadcast` threshold. This threshold is controlled by the configuration parameter `spark.sql.autoBroadcastJoinThreshold`, which is set to 10 MB by default (and maxes out at 8GB). When a dataset falls below this size, Spark will automatically `broadcast` it to all `nodes` in the `cluster` without requiring explicit instructions from the developer. This automatic `broadcasting` is particularly useful because it optimizes performance without the need for manual intervention, allowing Spark to handle `joins` more efficiently by avoiding unnecessary shuffling.

Despite the performance improvements from `broadcasting,` it's essential to be mindful of the `broadcast` threshold and how `broadcasting` is used in general. While `broadcasting` small datasets can significantly improve performance, attempting to `broadcast` datasets that are too large can lead to memory issues, as each `node` must store the entire `broadcasted` dataset in memory. If a dataset exceeds the `broadcast` threshold and is still small enough to benefit from `broadcasting,` the threshold can be adjusted by setting the `spark.sql.autoBroadcastJoinThreshold` parameter to a higher value. Conversely, if `broadcasting` is not desirable for certain operations, this threshold can be lowered to prevent automatic `broadcasts.` Understanding and tuning this threshold allows developers to strike the right balance between leveraging Spark's automatic optimizations and effectively managing the resources available in their `cluster.`

### Conclusion

`Broadcasting` in Spark is a powerful tool for optimizing distributed computations, mainly when dealing with large datasets and complex operations like `joins.` By minimizing network traffic and reducing the need for shuffling, `broadcasting` can significantly improve the efficiency and speed of your Spark applications. Understanding when and how to use `broadcasting` and tuning the `broadcast` threshold is crucial for maximizing the performance of your Spark jobs. With these insights, you can make more informed decisions that enhance the effectiveness of your distributed data processing.

### References

And resources to learn more from: 
- [The Women of Bletchley Park](https://artsandculture.google.com/story/the-women-of-bletchley-park-bletchley-park/qgVxQIAxvdB-JA)
- [`Class Broadcast<T>`](https://spark.apache.org/docs/1.5.1/api/java/org/apache/spark/broadcast/Broadcast.html)
- [`Broadcast` Variables](https://books.japila.pl/apache-spark-internals/broadcast-variables/)
- [Video on Spark `Broadcast` Variables](https://youtu.be/irn2Ow4QhWM?si=9q0tg2AnXsH-h82q)
- [Coding Example of `Broadcasting` in PySpark](https://www.youtube.com/watch?v=uituvF5KS80)
- [Spark Performance Tuning](https://spark.apache.org/docs/latest/sql-performance-tuning.html)
- [`Broadcast` `join` in Spark (including how to configure Spark auto `broadcast`)](https://medium.com/@ashwin_kumar_/broadcast-join-in-spark-e128a7c21b94)
- [Usage of “`Broadcast` Variable” in “Joins” in “Apache Spark”](https://oindrila-chakraborty88.medium.com/usage-of-broadcast-variable-in-joins-in-apache-spark-4c3f07aed4be)
- [`Join` Strategies in Apache Spark— Deep Dive](https://medium.com/@ghoshsiddharth25/apache-spark-join-strategies-deep-dive-26bf7e85db28)
- [Primer on Spark `Join` strategy](https://faun.pub/primer-on-spark-join-strategy-134e7340f7a6)
