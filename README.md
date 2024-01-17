# BibReview

Bibliographic review and general notes for my PHD.

## Machine Learning and Statistics



**Takamoto, M., Morishita, Y., Imaoka, H., 2020. An efficient method of training small models for regression problems with knowledge distillation. http://dx.doi.org/10.48550/ARXIV.2002.12597.**


## Time-Series

**Runge, J. Causal network reconstruction from time series: From theoretical assumptions to practical estimation. Chaos Interdiscipl.
J. Nonlinear Sci. 28, 075310. https://doi.org/10.1063/1.5025050 (2018).**

**Hyndman, Rob J., and George Athanasopoulos. Forecasting: principles and practice. OTexts, 2018. https://otexts.com/fpp2/.**

**Lines, J. & Bagnall, A. Time series classification with ensembles of elastic distance measures. Data Min. Knowl. Disc. 29, 565–592
(2015). https://link.springer.com/article/10.1007/s10618-014-0361-2.**

**Ma, Q., Zheng, J., Li, S. & Cottrell, G. W. Learning representations for time series clustering. In Advances in Neural Information
Processing Systems Vol. 32 (eds Ma, Q. et al.) 3781–3791 (Curran Associates Inc, 2019). https://papers.nips.cc/paper_files/paper/2019/hash/1359aa933b48b754a2f54adb688bfa77-Abstract.html.**


### Applications to Reservoir Simulation

**Gabriel Cirac, Jeanfranco Farfan, Guilherme Daniel Avansi, Denis José Schiozer, Anderson Rocha,
Deep hierarchical distillation proxy-oil modeling for heterogeneous carbonate reservoirs,
Engineering Applications of Artificial Intelligence,
Volume 126, Part C,
2023,
107076,
ISSN 0952-1976,
https://doi.org/10.1016/j.engappai.2023.107076.**

* Summary

    * Used a metamodel to estimate production curves from a reservoir model data. The method used is called **Deep Hierarchical Distillation**. First the input data (geological, fluid, relative permeability) is compressed (dimensional reduction) using [UMAP](https://umap-learn.readthedocs.io/en/latest/).

    * Three NN are fed the input data with a fully connected layer, that outputs in 3 different sizes (before the convolutional layers). Each subsequent NN increases the size of the output of the first fully connected layer. The output of one NN is used as input to the next NN (after the convolutional layers).

    * The NN was trained on a fraction of the simulation models that would normally be used in a risk analysis (from an ensemble). The input data from the remaining reservoir models are then fed into the NN to generate the additional results.

* Personal notes

    * The proposed methodology was better than the other alternatives tested, but not *much better* than k-nearest neighbors (KNN) in both predicting the risk curve and the individual production curves.

    * The output curves from the first NN are too close to one another. The filter size might have been too restrictive.

    * The author combined spatial data and scalar data and then used a series of convolutional layers. Does that make sense with the scalar data (fluid, relative permeability, …)?

    * It is not clear what was done to create additional data points from the initial data points. The author simply states a *previous* study defined the methodology applied (no reference).

**Castro, Manuel, Pedro Ribeiro Mendes Júnior, Aurea Soriano-Vargas, Rafael de Oliveira Werneck, Maiara Moreira Gonçalves, Leopoldo Lusquino Filho, Renato Moura et al. "Time series causal relationships discovery through feature importance and ensemble models." Scientific Reports 13, no. 1 (2023): 11402. https://doi.org/10.1038/s41598-023-37929-w.**

* Summary

    * The paper aims to establish causal relationships from time-series. The idea is to incrementally build ML models that forecast some feature using data from the past. A new ML model is built using the additional potential *driver*, and if the ML model yields better results than the *current* ML model, this *driver* is added to the *current* ML model.

        * It is proposed a way to estimate the *relative importance* of each *driver* in the output under evaluation. The features from a *driver* are shuffled and the quality of the resulting model is compared to the *current* ML model. If the probability of the *current* ML model being better by chance is smaller than a threshold, this driver is considered to impact the target.

        * Regression models used: [Random Forest](https://link.springer.com/article/10.1023/A:1010933404324) and [CatBoost](https://arxiv.org/abs/1706.09516).

    * The authors suggest using dimensionality reduction (UMAP) as a way to deal with time-series that are too similar to one another.

    * The authors applied the proposed methodology to production data from a (synthetic and real data) pre-salt field. The results seem to agree to the available tracer data.

        * The authors used a 6-month window to analyze the data (avoid injector changing fluid). The target was the producers' BHP, with past the producers' past BHP, injectors' BHP and fluid injection.

        * Lags are approximately 1 to 8 days.

* Personal notes

    * The author states the time series must be transformed to be [stationary](https://otexts.com/fpp2/stationarity.html). Why is this needed? What transformations were made to the production data?

    * It is not clear, but it seems only the BHP from the producer under evaluation was used. The results only depict producer-injector relationships. Maybe it would be interesting to add the other producers and their respective fluid productions to the evaluation.

**Ferreira, Vitor Hugo de Sousa, Castro, Manuel, Moura, Renato, Werneck, Rafael de Oliveira, Zampieri, Marcelo Ferreira, Gonçalves, Maiara Moreira, Linares, Oscar, Salavati, Soroor, Lusquino Filho, Leopoldo Andre Dutra, Ribeiro Mendes Júnior, Pedro, Mello Ferreira, Alexandre, Davolio, Alessandra, Schiozer, Denis Jose, and Anderson Rocha. "A New Hybrid Data-Driven and Model-Based Methodology for Improved Short-Term Production Forecasting." Paper presented at the Offshore Technology Conference, Houston, Texas, USA, May 2023. doi: https://doi.org/10.4043/32167-MS**

* Summary

    * Uses a recurrent NN (DD model) to forecast short-term production data (1 month) and then to help limit the number of simulation models (from an ensemble) in a 6-month risk assessment.

        * Compares the 1-month forecast of all simulation models in the ensemble to the DD model. The k-best models (closest to the DD model) are then used to make a 6-month forecast.

    * The DD model is the [*Gated Recurrent Unit* GRU2_10](https://doi.org/10.1016/j.petrol.2021.109937), trained for each variable of each well (Qo, Qg, Qw, BHP).

* Personal notes

    * Limited the analysis to a single DD model. Could have use more models as a way to assess the variability of the forecast.

    * It seems the authors didn't verify if the production controls for the forecast were appropriate in each simulation model, for they lack continuity going from the history period into the prediction.

**Da Silva, L.M., Avansi, G.D., Schiozer, D.J., 2020. Support vector regression for petroleum reservoir production forecast considering geostatistical realizations. SPE Reserv. Eval. Eng. 23 (04), 1343–1357. http://dx.doi.org/10.2118/203828-PA.**

**de Oliveira Werneck, R. et al. Data-driven deep-learning forecasting for oil production and pressure. J. Petrol. Sci. Eng. 210, 109937.
https://doi.org/10.1016/j.petrol.2021.109937 (2022) **


## Surrogate Modelling


## Reservoir Simulation


## Others