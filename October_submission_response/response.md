We thank the reviewers for their time spent providing such detailed and constructive feedback. All of their points are valid and we will use them to help improve our paper. Below we give our perspective on their points and clarify some of the issues raised. We hope that the reviewers agree that, given the work that we are currently pursuing addresses a subset of the points, that the remaining updates are within the scope of a revision.

### Insufficiently different from existing work, particularly reference [36] (R1, R2)
We agree that [36] is strongly related, however, we believe that our work goes substantially further (addresses R1.O1, R2.O2):
- From a practical point of view, [36] only covers gathering information about selection output cardinalities in a scenario in which even the plan is considered black-box (i.e., just-in-time compiled). Selections are generally not the most costly operators and therefore only mildly interesting for optimization. Our work applies to selections as well as costly operators (‘group by’ and ‘join’). We agree with the reviewers that this point has not be made sufficiently clear in the paper and we will revise it to rectify that.
- From an intellectual point of view, we propose a solution that is only superficially related to [36]: we propose a white-box micro-adaptive operator design whereas [36] assumes black box plans. We also propose a general framework that is applicable to the common relational operators. In our opinion, it is the lack of such a framework that has hurt adoption of the ideas outlined in [36]. We will revise the paper to emphasize this intellectual novelty.

### Limited evaluation (R1, R2, R3)
We agree with the reviewers that the evaluation is unsatisfactorily narrow and have been working to rectify this:
- We are currently integrating our approach into a broader system which will allow us to run full queries and quantify the performance uplift versus existing systems. Thus far the results appear sound and we have not identified any factors that will prevent a full integration with the system (addresses R1.O2, R1.O4, R1.D3, R3.O1, R2.O1).
- The evaluation will be performed on modern intel and AMD hardware (addresses R1.D4, R1.D5, R3.O6).

### Lack of discussion of how well approach works in practice (R1, R2, R3)
The reviewers highlighted a lack of discussion and evaluation of how well our approach works in practice and across different platforms. We acknowledge the lack of discussion and proof in this area and hope that our broadened evaluation will go some way to rectifying this. Beyond this:
- We will include a more detailed discussion of the automated calibration (setting of heuristics) that we use and naturally evaluate its robustness in our broadened evaluation across multiple platforms (addresses R1.O3, R1.D2, R2.O3). It is this automated calibration which allows our approach to be easily portable across platforms (addresses R2.O4).
- Our system does require access to the PMU, necessitating bare-metal setups, however, only counters of simple micro-architectural events are required. Since performance counters exist on a per core basis the required number of counters will always be available. We will make these points clear in the paper (addresses R3.O2).
- Furthermore, we will justify why our heuristics do not require dynamic reconfiguration: per core events such as branch mispredictions or sTLB store misses are counted per core and therefore are isolated from other processes or concurrent operators. In addition, where our operators use shared resources they are naturally adaptive to contention of this resource (such as last level cache usage) (addresses R3.O3, R3.O7). 
- Additionally, we will discuss our approach with respect to ARM platforms (addresses R1.D6).

### Limited performance impact in some cases whilst adding significant complexity (R1)
- We are in the process of integrating our approach into a larger system and have not found the complexity to be prohibitively high for the associated benefits. We will make this complexity and the associated benefits clear in our revised evaluation (addresses R1.O4, R1.D8).

### Proof of motivation (R2)
- We agree with the reviewer that further citations are required to substantiate the claim that non-adaptive approaches that measure the sortedness of data are are complicated and brittle, and we will update the paper accordingly (addresses R2.O5).

### Clarity of explanation (R1, R2, R3)
The reviewers identified several areas where the clarity of our explanation was lacking. We agree that our explanations in these areas were not sufficient and we will update them in the paper:
- We argue that the use of cardinality and other order-independent metrics cannot be used to correctly identify the optimal physical operator implementation to use. However, we do assume that these metrics are available to be used for other purposes such as setting hash table size. This is in the same fashion as other DBMSs and is unrelated to our contributions (addresses R2.O6, R2.O8).
- ‘Probes’ are test runs of hazard-affected algorithms. If tuples are to be processed by the hazard-robust algorithm these can be left unprocessed until after all hazard-affected algorithms are run and then processed by a second pass over the input by the hazard-robust algorithm. This is why the hazard-robust algorithm knows the number of tuples to process (addresses R2.O7, R3.O5).
- We will highlight the tipping points on figure 6, and update the charts so they are visible in b/w (addresses R2.O9).
- The hazard-robust radix partitioning algorithm uses a fan-out that is less than the sTLB capacity, therefore it will not suffer sTLB misses and is hazard-robust (addresses R1.D10).
- Of our three operators, only ‘group by’ necessitates that the order of tuples is changed, with the implementation of the other two allowing the order of tuples to remain unchanged. We agree that this is a downside for ‘group by’ and will make this clear within the paper (addresses R3.O4).

### Minor remarks
- We thank the reviewers for their attention to detail regarding minor comments such as typos, definitions, figure sizes, defining acronyms, etc. and will update the paper to reflect these comments.