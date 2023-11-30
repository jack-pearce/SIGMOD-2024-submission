 We thank the reviewers for their time providing such detailed and constructive feedback. All of their points are valid and we will use them to improve our paper. Below we give our perspective on their points. We hope that the reviewers agree that the revision items we propose are within the scope of a revision.

### Insufficiently different from existing work, particularly reference [36] (R1.O1, R2.O2)
We agree that [36] is strongly related, however, we believe that our work goes substantially further:
- From a practical point of view, [36] only covers gathering information about selection output cardinalities and only in a scenario in which even the plan is considered black-box (i.e., just-in-time compiled). Selections are generally not the most costly operators and therefore only of limited impact for optimization. Our work applies to selections as well as more costly operators (‘group by’ and ‘join’). We agree with the reviewers that this point is not sufficiently clear in our paper and we will revise it accordingly.
- From an intellectual point of view, we propose a solution that is inspired but only loosely related to [36]: in contrast to the jit-compilation focus of [36], we propose an operator-based design that handles micro-adaptivity using a white-box approach. To this end, we propose a conceptual framework that is applicable to the common relational operators. In our opinion, it is the lack of such a framework that has hurt adoption of the ideas outlined in [36]. We will revise our paper to emphasize this intellectual novelty.

### Limited evaluation (R1, R2, R3)
We agree with the reviewers that the evaluation should be broader and based on diverse (and modern) hardware and we have already started working to rectify this:
- We are rerunning the evaluation on a modern Intel, as well as AMD, CPU to assess impact and robustness on different platforms (R1.D4, R1.D5, R3.O6, {points on robustness})
- We are currently integrating our approach into an open-source system which will allow us to evaluate more and complexer queries (R1.O2, R1.D3, R2.O1).

### Lack of discussion of how well approach works in practice (R1, R2, R3)
The reviewers highlighted a lack of discussion of the applicability of our approach to a wider range of systems. We acknowledge this and will address this through the following measures:
- We will explain the requirements and steps for integrating our approach with other systems, for example, the API, performance counter access and calibration process (R1.O3, R3.O1).
- We will include a more detailed discussion of the automated calibration (setting of heuristics) that we use (R1.D2, R2.O3).
- Our system requires access to the PMU which, with the necessary permissions, should also be available on containerized setups (https://doi.org/10.1145/1952682.1952686). Additionally, since performance counters exist on a per-core basis the required number of counters will always be available. We will make these points clear in our paper (R3.O2).
- We will justify why our heuristics do not require dynamic reconfiguration: per-core events such as branch mispredictions or sTLB store misses are counted per-core and therefore are isolated from other processes or concurrent operators. In addition, where our operators use shared resources they are naturally adaptive to contention of this resource (such as last-level cache usage) (R3.O3, R3.O7).
- As we integrate our approach into an open-source system we have not yet identified any factors that prevent a full integration with our approach.
- We will discuss our approach with respect to ARM platforms (R1.D6).

### Limited performance impact in some cases whilst adding significant complexity (R1)
- Currently, we have found that only moderate effort has been required to integrate our approach into an open-source system (R1.O4).
- The complexity of extending our approach to other operators and for use on other systems is in part addressed through our automated calibration (R2.O4).
- We will update our paper to reflect both of the points above.

### Proof of motivation (R2.O5)
- We agree with the reviewer that further citations are required to substantiate the claim that non-adaptive approaches that measure the sortedness of data are complicated and brittle, and we will update our paper accordingly (such as https://doi.org/10.1145/2463676.2465306).

### Clarity of explanation (R1, R2, R3)
The reviewers identified several areas where our paper was unclear. We agree that our explanations in these areas were insufficient and will update all of the following points:
- We argue that the use of cardinality and other order-independent metrics cannot be used to identify the optimal physical operator implementation. However, we assume that those metrics are available for other purposes such as setting hash table size. Like other systems, our approach is unaffected by stale values for these metrics (R2.O6, R2.O8).
- It appears that reviewers may have misunderstood that the micro-batches processed by the hazard-robust algorithm are processed out-of-order versus the hazard-affected algorithms, we apologize for our lack of explanation causing this misunderstanding. Where necessary processing and test runs of the hazard-affected algorithms ('probes') are completed during the first pass over the input data. On the other hand, tuples processed by the hazard-robust algorithm are processed out-of-order after the first pass over the data is complete. This is why the hazard-robust algorithm knows the number of tuples to process (R2.O7, R3.O5).
- We will highlight the tipping points in Figure 6 (R2.O9).
- The hazard-robust radix partitioning algorithm uses a fan-out that is less than the sTLB capacity, therefore it will not suffer sTLB misses and is hazard-robust (R1.D10).
- We neglected to mention that in our implementation only 'group by' requires tuples to be processed out-of-order by the hazard-robust algorithm. However, order is never retained by the 'group by' operator (R3.O4).