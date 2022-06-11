# Fast face recognition

The task is to store face embeddings in the most efficient way.
To be considered:

1. Speed of searching
2. Speed of building
3. Precision
4. Face Capacity
5. Simplicity of rebuilding

## Python-based entities - Annoy and HNSW

There are metrics scores for n=1.000 faces, n=10.000 faces and n=40.000 faces.
Table reflects three metrics: 
1. "intersection" - amount of faces retrieved by both approaches
2. "apk" - Average Precision@k
3. "intersection" - intersection with respect to retrieved order of ranking

|                | n=1 000      |      |                           | n=10 000     |      |                           | n=40 000     |      |                           |
|----------------|--------------|------|---------------------------|--------------|------|---------------------------|--------------|------|---------------------------|
| Request\Metric | Intersection | APT  | Intersection, fixed order | Intersection | APT  | Intersection, fixed order | Intersection | APT  | Intersection, fixed order |
| #1             | 0.7          | 0.51 | 0.56                      | 0.3          | 0.09 | 0.25                      | 0.3          | 0.09 | 0.19                      |
| #2             | 0.8          | 0.8  | 0.73                      | 0.6          | 0.39 | 0.47                      | 0.8          | 0.79 | 0.67                      |
| #3             | 0.8          | 0.78 | 0.74                      | 0.2          | 0.04 | 0.07                      | 0.3          | 0.17 | 0.26                      |
| #4             | 0.9          | 0.9  | 0.84                      | 0.5          | 0.31 | 0.43                      | 0.7          | 0.56 | 0.62                      |
| #5             | 0.8          | 0.63 | 0.62                      | 0.4          | 0.22 | 0.3                       | 0.5          | 0.32 | 0.48                      |
| #6             | 0.6          | 0.36 | 0.52                      | 0.1          | 0.01 | 0.04                      | 0.2          | 0.07 | 0.15                      |
| #7             | 0.7          | 0.62 | 0.6                       | 0.6          | 0.31 | 0.42                      | 0.0          | 0.0  | 0.0                       |
| #8             | 0.5          | 0.31 | 0.39                      | 0.4          | 0.17 | 0.31                      | 0.1          | 0.01 | 0.02                      |
| #9             | 0.5          | 0.31 | 0.39                      | 0.6          | 0.49 | 0.52                      | 0.5          | 0.35 | 0.4                       |
| #10            | 0.6          | 0.49 | 0.54                      | 0.5          | 0.19 | 0.27                      | 0.4          | 0.22 | 0.29                      |
| Mean           | 0.69         | 0.49 | 0.539                     | 0.42         | 0.22 | 0.3                       | 0.38         | 0.25 | 0.25                      |



Time results:

1. Annoy, n=1000
```
Overall Building time 303.6290888786316
Overall Building time per face 0.0
Only Building (reBuilding) time 0.04105257987976074
Searching time per request 0.0
```
2. HNSW, n=1000
```
Overall Building time 177.54643487930298
Overall Building time per face 0.0
Only Building (reBuilding) time 0.002043008804321289
Searching time per request 0.0
```
3. Annoy, n=10000
```
Overall Building time 1291.201500415802
Overall Building time per face 0.0
Only Building (reBuilding) time 0.3404364585876465
Searching time per request 0.0
```
4. HNSW, n=10000
```
Overall Building time 999.343019247055
Overall Building time per face 0.0
Only Building (reBuilding) time 0.0
Searching time per request 0.0
```
5. Annoy, n=20000
```
Overall Building time 1671.1036043167114
Overall Building time per face 0.0
Only Building (reBuilding) time 0.22203779220581055
Searching time per request 0.0
```
6. HNSW, n=20000
```
Overall Building time 1715.2805714607239
Overall Building time per face 0.0
Only Building (reBuilding) time 0.007437229156494141
Searching time per request 0.0
```
7. Annoy, n=40000
```
Overall Building time 3671.860947370529
Overall Building time per face 0.0
Only Building (reBuilding) time 0.46132349967956543
Searching time per request 0.0
```
8. HNSW, n=40000
```
Overall Building time 3456.0494883060455
Overall Building time per face 0.0
Only Building (reBuilding) time 0.0019981861114501953
Searching time per request 0.0
```

### Conclusion:
1. Speed of searching - extremely high for **both**
2. Speed of building - approximately the same for **both**, relatively **high** (in terms of considerablr amount of faces)
3. Precision - the more faces - the less precision. Possible solution: organize local storages of city (or restuaran) level
4. Face Capacity - **both** approaches cope **well** with high amount of faces
5. Simplicity of rebuilding - very simple **BUT not for Android**. It uses already built indexing files for searching => new face addicion might be very **complicated**. HNSW is preferable coz it can incrementally add points to the index
