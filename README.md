# Page-Rank-Implementation-using-Spark
This project involved implementing a Spark PageRank program designed for a synthetic graph where each page has at most one outgoing link, and subsequently analyzing Spark's RDD lineage, lazy evaluation, and caching behavior during iterative computations, alongside its performance on AWS EMR

- Implemented a **Spark PageRank program** tailored for a synthetic graph where each page has at most one outgoing link.

- **Program Details**:
  - Written in Spark Scala.
  - Parameters:
    - `k`: Graph size
    - Number of iterations
  - Graph RDDs initialized with `HashPartitioner`.
  - Graph RDD cached for reuse across iterations.

- **PageRank Computation**:
  - Performed iteratively:
    - Join graph with current ranks.
    - Reduce ranks by key.
    - Explicitly handle nodes with zero in-degree to redistribute rank.

- **Key Insights**:
  - Without explicit caching:
    - Spark’s actions trigger jobs over the entire lineage.
    - Despite this, job execution times stabilized after initial iterations.
    - Suggests Spark **implicitly reuses RDD data** rather than recomputing everything.
  - With explicit `.cache()` on ranks RDD:
    - Logs confirmed blocks **stored as values in memory** and **found block ... locally**.
    - Even without explicit caching, some similar log messages appeared, pointing to possible **implicit block management** by Spark’s storage layer.

- **Performance Evaluation**:
  - Tested on **AWS EMR**.
  - Ran on synthetic graph with `k=10,000` nodes over 10 iterations.

- **Conclusion**:
  - Spark shows intelligent data reuse in iterative workloads.
  - Explicit caching improves performance transparency and confirms in-memory storage.
