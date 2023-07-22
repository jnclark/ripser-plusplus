This is a single purpose driven fork of the software
[ripser++](https://github.com/simonzhang00/ripser-plusplus) (original
authors Simon Zhang, Mengbai Xiao and Hao Wang) with modifications by
Killian Meehan and Jacob Clark for use in [Topological
Node2vec](https://github.com/killianfmeehan/topological_node2vec).

This fork modifies the behavior of `Ripser++` in the following ways:

The return type `birth_death_coordinate` has been replaced by an
`arbitrary_simplex` to identify the precise simplices responsible for
births and deaths in order to make gradient updates when looking at
the gradient of the persistence diagram with respect to the generating
point cloud, see [this paper](https://arxiv.org/abs/1506.03147).  With
this change, the function `ripserplusplus.run` returns a dictionary
indexed by homology dimension, of which the values are arrays of
vectors of the form

```python
(b1,b2,0,0,0,d1,d2,d3,0,0) # for homological dimension 1 features
(b1,b2,b3,0,0,d1,d2,d3,d4,0) # for homological dimension 2 features
(b1,b2,b3,b4,0,d1,d2,d3,d4,d5) # for homological dimension 3 features
```

where the `bi` and `di` are integers corresponding to the total order
on the rows/columns of the distance matrix (corresponding to points in
the original point cloud), writing out the precise birth death
simplices of the features of the associated homological dimension. For
example:

```python
import ripserplusplus
X = ripserplusplus.run('--format distance --dim 2', some_distance_matrix)
rpp_hom_dict = {}
for dim in X.keys():
    if dim == 0:
        # presently, our edited ripser-plusplus code does not
        # accommodate homology dimensions of d == 0 or d > 3
        continue
    rpp_hom_dict[dim] = []
    for l in X[dim]:
        rpp_hom_dict[dim].append([[l[i] for i in range(dim+1)],
                                  [l[5+i] for i in range(dim+2)]])
print(rpp_hom_dict)
```

where `some_distance_matrix` is a distance matrix of the reader’s
choice.

Installation prerequisites and instructions remain unchanged from the
original project and can be found alongside additional information in
the original README below.

[Original README](README.orig.md)
