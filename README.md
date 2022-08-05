# GSOC Midterm Evaluation

Task: Graph Neural Networks for End-to-End Particle Identification with the CMS Experiment

## Graph Representation

### Dataset Description

The boosted top tau particle dataset has 8 channels: (pt, d0, dz, ECAL, HCAL, BPIX1, BPIX2, BPIX3)

- (pt, d0, dz) are tracker layers
- ECAL and HCAL are electromagnetic and hadronic calorimeter deposits respectively
- BPIX are the pixel layers which are integer values representing the number of collisions occurring in the pixel layers.

### Graph Representation

In order to avoid data sparsity issue, we count non-zero pixels in X_jets as node in graph. The node has three to eight feature dimensions depending which channels combination we use. The edge has static 

#### Using different channels

We can choose to use different combination of channels and experiment what gives the best performance. What I have experimented by now includes:

- using full channels
- tracker layers combined with ECAL&HCAL
- only ECAL&HCAL

#### Static graph

The static graph is constructed in k nearest neighbors way.

First, we can set different k to have different graph. When k becomes larger, the graph becomes more complicatedlly connected. By far we have tried k = 10, 15, 25, 50.

Second, we can decide the mode is connectivity or distance. When we use connectivity, the edge weights are cosists of 0 and 1, 1 when two node is connected. When we use distance, the weight is computed with distance metrics, default metrics is minkowski distance.

#### Dynamic graph

The edge is not pre-defined and fixed, it is constructed during convolution with k-nearest neighbors methods. To do this, we apply dynamic edge convolution model.

## Models

### Graph Convolution

<img src="img/GraphConv" alt="image-20220805213034048" style="zoom:80%;" />

### Graph Attention

<img src="img/GAT" alt="image-20220805212901314" style="zoom:80%;" />

### Graph SAGE

<img src="img/GraphSAGE" alt="image-20220805213125453" style="zoom:80%;" />

### Dynamic Edge Convolution

<img src="img/EdgeConv" alt="image-20220805213214390" style="zoom:80%;" />

### CNN-based model

<img src="img/CNN" alt="image-20220805213319543" style="zoom:80%;" />

## Performance

Since experiment is still undergoing, so more performance comparison are still being added.

| Model             | Test AUC | Valid AUC |
| ----------------- | -------- | --------- |
| CNN               | 0.794    | 0.801     |
| Graph Conv        | 0.802    | 0.813     |
| Graph Attention   | 0.778    | 0.784     |
| Graph SAGE        | 0.782    | 0.786     |
| Dynamic Edge Conv | 0.796    | 0.809     |