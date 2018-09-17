### Nasty Nested LOops

A zoo of typical operations on collections in HEP analyses.<br>
HEP analysts often need to express operations of the form:
```python
for e in events:
  for p in particles:
    do_stuff()
```
in which case `particles` would be a "nested collection".

The zoo is meant to be small but representative of what's out there in the wild, both in terms of requirements/usecases.
For each problem we also present a non-exhaustive set of solutions. Typical approaches to express nasty nested loops are
- work with collections imperatively
- transparently aggregating SoA's in AoS's (as xAODs do, so you have to deal with `Electrons` rather than several different arrays each carrying one electron property) and work on these imperatively
- using smart array/tensor/dataframe object ([ROOT's `RVec`](https://root.cern/doc/master/classROOT_1_1VecOps_1_1RVec.html), numpy-like associative arrays like [awkward-array](https://github.com/scikit-hep/awkward-array), pandas-like dataframes like [xframe](https://github.com/quantstack/xframe))
- introducing a domain specific language as in [TTree::Draw](https://root.cern/doc/master/classTTree.html#a8a2b55624f48451d7ab0fc3c70bfe8d7) or [LINQtoROOT](https://github.com/gordonwatts/LINQtoROOT)

The zoo can be found [here](https://github.com/bluehood/NNLOops/blob/master/NNLOops.md).<br>
Pull requests and issues are welcome.
