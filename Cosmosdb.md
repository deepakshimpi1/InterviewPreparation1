- CosmosDB is a NoSQL database in Azure.
- Azure Cosmos DB scale horizontally rather than vertically. So it has boomed the big data demands and can handle big data volume.
- Rather than upgrading hardware on a single node to serve requests faster, Cosmos DB distributes that work across multiple nodes so requests can be served concurrently
- Cosmos DB distributes that works across multiple nodes so requests can be served concurrently.
- Cosmos DB is schema-less meaning structure is not enforced on any data entering the database.
- Two documents in the same collection can have a completely different structure without any issue. 
- And as a result, developers can and structurally variable data into the database and later build, change or add functionality as use for that data evolves. 
    - This combination of flexible schema and horizontal scaling is what makes NoSQL different.
- Cosmos has a SQL API with document and key-value support built-in. 
    - Documents and key-value pairs go hand in hand in most NoSQL databases 
    - because documents are just key-value pairs in which the key is a document and the value is a JSON object that we just call a document.
- Horizontal scaling or scaling out is the consequence of two techniques: `partitioning` and `replication`.

### Partitioning

- There are two kinds of partitions in Azure Cosmos DB.
- Partitioning our data across nodes improves database latency

1. Physical
    - It is an actual unit of storage that physically sits in the Azure region.
    - This is a piece of hardware holding our data somewhere in the cloud.

1. Logical
    - It is logical groups across items within our data set.
    - We reference these groups through something called `partition key`.
    - The partition key is important because it tells our database engine where to look for our data.

    ### Replication

    1. Replication within a region
        - Now within a region, our data is replicated four times as a redundancy measure, improving fault tolerance.
        - Replication within a single region increases fault tolerance.

    2. Geo-replication outside of a region
        - Outside of a region, our data is geo-replicated into any additional Azure regions we select resulting in higher availability.
        - Geo-replication into additional Azure regions increases availability.

---
### Choosing a Partition Key in Cosmos DB

- A partition key consists of a path. This can be something like /firstname or /name/first, or a nested property, as long as it is a JSON property from the documents in the container. 
- The value of the key shouldn’t change. The last name might not be a good one as that might change when people get married or divorced.
- The key should have a large range of values. The first name will be different for many people, and something like ID works very well when a unique value is used. This optimizes the use of partitions and enhances performance.
- A partition key can have values of string or numeric types, and once you’ve created a key for a container, you can’t change it anymore.

**Note**: There’s one exception. If your container is large and read-heavy and large, that means it has 30,000 or more RUs assigned to it or it is larger than 100 gigabytes.

- If that’s the case, then the partition key should be something that your queries filter on a lot. If your queries filter on user Id a lot, that might be a great partition key. When you choose the right partition key for your Azure Cosmos DB container, you optimize performance. 