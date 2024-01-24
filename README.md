Bibliographic review and general notes for my PHD.  
Citations don't follow a single pattern, and are listed only for reference.

# Machine Learning and Statistics

Goodfellow, I., Bengio, Y. and Courville, A., 2016. **Deep learning.** MIT press. https://www.deeplearningbook.org/.

---

Michael A. Nielsen, "**Neural Networks and Deep Learning**", Determination Press, 2015. http://neuralnetworksanddeeplearning.com/.

---

Takamoto, M., Morishita, Y., Imaoka, H., 2020. **An efficient method of training small models for regression problems with knowledge distillation.** http://dx.doi.org/10.48550/ARXIV.2002.12597.


## Time-Series

Runge, J. **Causal network reconstruction from time series: From theoretical assumptions to practical estimation.** Chaos Interdiscipl. J. Nonlinear Sci. 28, 075310. https://doi.org/10.1063/1.5025050 (2018).

---

Hyndman, Rob J., and George Athanasopoulos. **Forecasting: principles and practice.** OTexts, 2018. https://otexts.com/fpp2/.

---

Lines, J. & Bagnall, A. **Time series classification with ensembles of elastic distance measures.** Data Min. Knowl. Disc. 29, 565–592 (2015). https://link.springer.com/article/10.1007/s10618-014-0361-2.

---

Ma, Q., Zheng, J., Li, S. & Cottrell, G. W. **Learning representations for time series clustering.** In Advances in Neural Information Processing Systems Vol. 32 (eds Ma, Q. et al.) 3781–3791 (Curran Associates Inc, 2019). https://papers.nips.cc/paper_files/paper/2019/hash/1359aa933b48b754a2f54adb688bfa77-Abstract.html.


## Applications to Reservoir Simulation

Gabriel Cirac, Jeanfranco Farfan, Guilherme Daniel Avansi, Denis José Schiozer, Anderson Rocha, **Deep hierarchical distillation proxy-oil modeling for heterogeneous carbonate reservoirs**, Engineering Applications of Artificial Intelligence, Volume 126, Part C, 2023, 107076, ISSN 0952-1976, https://doi.org/10.1016/j.engappai.2023.107076.**

* Summary
    * Used a metamodel to estimate production curves from a reservoir model data. The method used is called **Deep Hierarchical Distillation**. First the input data (geological, fluid, relative permeability) is compressed (dimensional reduction) using [UMAP](https://umap-learn.readthedocs.io/en/latest/).
    * Three NN are fed the input data with a fully connected layer, that outputs in 3 different sizes (before the convolutional layers). Each subsequent NN increases the size of the output of the first fully connected layer. The output of one NN is used as input to the next NN (after the convolutional layers).
    * The NN was trained on a fraction of the simulation models that would normally be used in a risk analysis (from an ensemble). The input data from the remaining reservoir models are then fed into the NN to generate the additional results.

* Personal notes
    * The proposed methodology was better than the other alternatives tested, but not *much better* than k-nearest neighbors (KNN) in both predicting the risk curve and the individual production curves.
    * The output curves from the first NN are too close to one another. The filter size might have been too restrictive.
    * The author combined spatial data and scalar data and then used a series of convolutional layers. Does that make sense with the scalar data (fluid, relative permeability, …)?
    * It is not clear what was done to create additional data points from the initial data points. The author simply states a *previous* study defined the methodology applied (no reference).

---

Castro, Manuel, Pedro Ribeiro Mendes Júnior, Aurea Soriano-Vargas, Rafael de Oliveira Werneck, Maiara Moreira Gonçalves, Leopoldo Lusquino Filho, Renato Moura et al. "**Time series causal relationships discovery through feature importance and ensemble models.**" Scientific Reports 13, no. 1 (2023): 11402. https://doi.org/10.1038/s41598-023-37929-w.

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

---

Ferreira, Vitor Hugo de Sousa, Castro, Manuel, Moura, Renato, Werneck, Rafael de Oliveira, Zampieri, Marcelo Ferreira, Gonçalves, Maiara Moreira, Linares, Oscar, Salavati, Soroor, Lusquino Filho, Leopoldo Andre Dutra, Ribeiro Mendes Júnior, Pedro, Mello Ferreira, Alexandre, Davolio, Alessandra, Schiozer, Denis Jose, and Anderson Rocha. "**A New Hybrid Data-Driven and Model-Based Methodology for Improved Short-Term Production Forecasting.**" Paper presented at the Offshore Technology Conference, Houston, Texas, USA, May 2023. doi: https://doi.org/10.4043/32167-MS

* Summary
    * Uses a recurrent NN (DD model) to forecast short-term production data (1 month) and then to help limit the number of simulation models (from an ensemble) in a 6-month risk assessment.
        * Compares the 1-month forecast of all simulation models in the ensemble to the DD model. The k-best models (closest to the DD model) are then used to make a 6-month forecast.
    * The DD model is the [*Gated Recurrent Unit* GRU2_10](https://doi.org/10.1016/j.petrol.2021.109937), trained for each variable of each well (Qo, Qg, Qw, BHP).

* Personal notes
    * Limited the analysis to a single DD model. Could have use more models as a way to assess the variability of the forecast.
    * It seems the authors didn't verify if the production controls for the forecast were appropriate in each simulation model, for they lack continuity going from the history period into the prediction.

---

Da Silva, L.M., Avansi, G.D., Schiozer, D.J., 2020. **Support vector regression for petroleum reservoir production forecast considering geostatistical realizations.** SPE Reserv. Eval. Eng. 23 (04), 1343–1357. http://dx.doi.org/10.2118/203828-PA.

---

de Oliveira Werneck, R. et al. **Data-driven deep-learning forecasting for oil production and pressure.** J. Petrol. Sci. Eng. 210, 109937. https://doi.org/10.1016/j.petrol.2021.109937 (2022)**

* Summary
    * The focus is on short-term prediction (days) of oil production and bottom-hole pressure.
    * The method tries to forecast a *window* of futures values (multi-output), using all the past data as input. The final prediction time-series is a concatenation of the last prediction point in each window.
        * Used perturbation (such as noise) to increase the robustness of the method.
        * Used TLCC ([Time Lagged Cross-Correlation](https://www.sciencedirect.com/science/article/pii/S0375960114012766)) to evaluate the connectivity between wells and the corresponding lag.
    * For comparison with other techniques the authors used the [GluonTS](https://ts.gluon.ai/stable/) toolkit.
    * Overall results seemed inconclusive.

* Personal notes
    * Presents a quite good revision of applications to production forecast with data driven models (though I don't always agree with the author's comments), as well as for general purpose time-series forecast architectures.
    * Used z-score to remove *anomalous* data points. Maybe this should be done in a sliding window, for the well controls might lead to significant modifications in the measured values.
        * This should also be done using pressure (BHP + WHP), temperature (WHT) and rates as a group.
        * A finer-grained well rate time-series would be needed to do this kind of analysis. (`possible subject to study`)
    * As many other papers have done, well control strategies and effects associated to the production system are ignored. (`possible subject to study`).
        * The proposed methodology might be useful, but it still lacks a better understanding of the problem. External controls still need to be accounted for in the forecasting.
    * Some perturbation methods used don't seem to make sense for this physical problem.
    * Ask for the github repository!

---

Ertekin, T.; Sun, Q. **Artificial Intelligence Applications in Reservoir Engineering: A Status Check.** Energies 2019, 12, 2897. https://doi.org/10.3390/en12152897

---

Zhong, Z., Sun, A.Y., Wang, Y., Ren, B., 2020. **Predicting field production rates for waterflooding using a machine learning-based proxy model.** J. Pet. Sci. Eng. 194, 107574. http://dx.doi.org/10.1016/j.petrol.2020.107574.

---

Haochen Wang, Kai Zhang, Xingliang Deng, Shiti Cui, Xiaopeng Ma, Zhongzheng Wang, Ji Qi, Jian Wang, Chuanjin Yao, Liming Zhang, Yongfei Yang, Huaqing Zhang, "**Highly accurate oil production forecasting under adjustable policy by a physical approximation network**", Energy Reports, Volume 8, 2022, Pages 14396-14415, ISSN 2352-4847, https://doi.org/10.1016/j.egyr.2022.10.406.

---

Aram Davtyan, Alexander Rodin, Ilya Muchnik, Alexey Romashkin, "**Oil production forecast models based on sliding window regression**", Journal of Petroleum Science and Engineering, Volume 195, 2020, 107916, ISSN 0920-4105, https://doi.org/10.1016/j.petrol.2020.107916.

---

Kim, Y.D., Durlofsky, L.J., 2021. **A recurrent neural network–based proxy model for well-control optimization with nonlinear output constraints.** SPE J. 26 (04), 1837–1857. http://dx.doi.org/10.2118/203980-PA

* Summary
    * Predicts production rates as a function of BHP (varying in time) with a RNN with a LSTM cell.
        * Additional full simulations are needed to ensure the final optimization with the RNN is optimal for the full simulation.

* Personal notes
    * x

---

Song, X., Liu, Y., Xue, L., Wang, J., Zhang, J., Wang, J., Jiang, L., Cheng, Z., 2020. **Time-series well performance prediction based on long short-term memory (LSTM) neural network model.** J. Pet. Sci. Eng. 186, 106682. http://dx.doi.org/10.1016/j.petrol.2019.106682.

---

Razak, S.M., Cornelio, J., Cho, Y., Liu, H.-H., Vaidya, R., Jafarpour, B., 2021. **Transfer learning with recurrent neural networks for long-term production forecasting in unconventional reservoirs.** In: SPE/AAPG/SEG Unconventional Resources Technology Conference. OnePetro, http://dx.doi.org/10.15530/urtec-2021-5687.

---

Jolaade, M., Silva, V.L.S., Heaney, C.E., Pain, C.C. (2022). **Generative Networks Applied to Model Fluid Flows**. In: Groen, D., de Mulatier, C., Paszynski, M., Krzhizhanovskaya, V.V., Dongarra, J.J., Sloot, P.M.A. (eds) Computational Science – ICCS 2022. ICCS 2022. Lecture Notes in Computer Science, vol 13352. Springer, Cham. https://doi.org/10.1007/978-3-031-08757-8_61

---

Silva, V., G. Regnier, P. Salinas, C. Heaney, M. Jackson, and C. Pain. "**Rapid Modelling of Reactive Transport in Porous Media using Machine Learning**." In ECMOR 2022, vol. 2022, no. 1, pp. 1-9. European Association of Geoscientists & Engineers, 2022. https://doi.org/10.3997/2214-4609.202244069


# Surrogate Modelling

Forrester, Alexander, Andras Sobester, and Andy Keane. **Engineering design via surrogate modelling: a practical guide.** John Wiley & Sons, 2008. https://onlinelibrary.wiley.com/doi/book/10.1002/9780470770801.

# Reservoir Simulation

Maschio, Célio, João Carlos von Hohendorff Filho, and Denis José Schiozer. "**Methodology for data assimilation in reservoir and production system to improve short-and medium-term forecast.**" Journal of Petroleum Science and Engineering 207 (2021): 109083. https://doi.org/10.1016/j.petrol.2021.109083

* Summary
    * Performs a history matching process in an integrated model (reservoir + flow), using iterative ES-MDA (Ensemble Smoother with Multiple Data Assimilation).
    * Reservoir and production system are solved independently, but sequentially.
    * Uncertainty from the production system HM is using in the reservoir HM process (multiple VLP tables). This way the HM process will limit the VFP tables to the ones that show the *best* match.
    * It was necessary to impose a larger weight to the HM in the last month of the HM period in order to get a better well productivity adjustment.

* Personal notes
    * It seems that the greatest contribution of the paper is to highlight that uncertainties related to the production system can, and should, be included into the uncertainty assessment procedure.

---

Schiozer, Denis José, Antonio Alberto de Souza dos Santos, Susana Margarida de Graça Santos, and João Carlos von Hohendorff Filho. "**Model-based decision analysis applied to petroleum field development and management.**" Oil & Gas Science and Technology–Revue d’IFP Energies nouvelles 74 (2019): 46. https://ogst.ifpenergiesnouvelles.fr/articles/ogst/pdf/2019/01/ogst180302.pdf

* Summary
    * Proposes a 12-step methodology to perform model-based field development and management under uncertainties:
        * **Green steps**: (1) Define uncertainties, (2) Calibrate models.
        * **Red steps**: (3) Check inconsistencies in the *base case*, (4) Generate uncertainty scenarios, (5) Perform data assimilation and update *base case*.
        * **Blue steps**: (6) Define and optimize production strategy for *base case*, (7) Perform risk analysis, (8) Select representative models, (9) Define and optimize production strategy for each representative model, (10) Define production strategy under uncertainties, (11) Improve production strategy with *value of information*, flexibility and integrated modelling analyzes.
        * **Black step**: (12) Decision analysis. 
    * Used the semi-deviation instead of the variance. This accounts for the fact that a negative deviation means greater risk, and a positive deviation an upside.

* Personal notes
    * Important to optimize production strategy not only on the *base case*, but also in all representative models.
    * Representative models should be defined not only on terms of possible outputs, but also the possible inputs (uncertainties). The suggested number of models is 9.
    * An optimization with each representative model helped discover value of information and value of flexibility strategies.
    
---

Schiozer D.J., Avansi G.D., Santos A.A.S. (2017) **A new methodology for risk quantification combining geostatistical realizations and discretized latin hypercube**, J. Braz. Soc. Mech. Sci. 39, 2, 575–587. https://doi.org/10.1007/s40430-016-0576-9

---

Silva L.O.M., Santos A.A.S., Schiozer D.J. (2016) **Otimização da Estratégia de Produção sob Incertezas Geológicas e Econômicas**, in: Rio Oil & Gas Expo and Conference, 24–27 October,
Rio de Janeiro, Brazil

---

Meira L.A., Coelho G.P., Santos A.A.S., Schiozer D.J. (2016) **Selection of representative models for decision analysis under uncertainty**, Comput. Geosci. 88, 67–82. https://doi.org/10.1016/j.cageo.2015.11.012.

# Others

* Summary
    * x

* Personal notes
    * x