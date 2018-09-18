Requirements:
* can deal with synchronized arrays
* can deal with columns of different lengths
* can perform join-like operations with a physicist-friendly syntax
* can offer views over separate collections, e.g. can group `muon_*` branches into a `muons` table
* must come with python bindings
* short function signatures in RDataFrame

Questions:
* do we need some support for nested collections?
* how does usage in python in conjunction with RDF look like?

RVec:
+ easy to grasp, easy to use
- when dealing with synchronized arrays, requires a lot of code repetition,
  e.g. selecting the same elements from all `muon_*` arrays
- function signatures become pretty long (and, in RDF, error prone as arguments must go in lockstep with column names)

