# BibReview

Bibliographic review and general notes for my PHD.

## Machine Learning



**Takamoto, M., Morishita, Y., Imaoka, H., 2020. An efficient method of training small models for regression problems with knowledge distillation. http://dx.doi.org/10.48550/ARXIV.2002.12597.**


### Applications to Reservoir Simulation

**Gabriel Cirac, Jeanfranco Farfan, Guilherme Daniel Avansi, Denis José Schiozer, Anderson Rocha,
Deep hierarchical distillation proxy-oil modeling for heterogeneous carbonate reservoirs,
Engineering Applications of Artificial Intelligence,
Volume 126, Part C,
2023,
107076,
ISSN 0952-1976,
https://doi.org/10.1016/j.engappai.2023.107076.**

* Used a metamodel to estimate production curves from a reservoir model data. The method used is called 'deep hierarchical distillation', where the input data (geological, fluid, relative permeability) is compressed in 3 different sizes and the neural network is retrained in this series of incremental quantity of data. The output of one NN is used as input to the next NN (after the convolutional layers).

* The NN was trained on a fraction of the simulation models that would normally be used in a risk analysis (from an ensemble). The input data from the remaining reservoir models were fed into the NN to generate the additional results.

* Results were [a little?] better than k-nearest neighbors (KNN) in both predicting the risk curve and the individual production curves.


**Da Silva, L.M., Avansi, G.D., Schiozer, D.J., 2020. Support vector regression for petroleum reservoir production forecast considering geostatistical realizations. SPE Reserv. Eval. Eng. 23 (04), 1343–1357. http://dx.doi.org/10.2118/203828-PA.**


## Surrogate Modelling


## Reservoir Simulation


## Others