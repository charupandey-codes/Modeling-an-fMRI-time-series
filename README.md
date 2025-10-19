# Modeling-an-fMRI-time-series

A comparison of MCMC and SBI methods for modeling an fMRI time series with an AR (1) model.

# Modeling-an-fMRI-time-series
A comparison of MCMC and SBI methods for modeling an fMRI time series with an AR (1) model.



Aim 

This report shows two methods used to model a single fMRI time series to understand its underlying dynamics. To achieve this, I employed an autoregressive model of order 1 (AR1) and fitted the data using two different Bayesian methods, MCMC and SBI, before comparing the results obtained from these techniques. 

Data was used from the VBI library (https://github.com/ins-amu/vbi); out of 88 regions, the first brain region, BOLD data was used. 

Shape of data for all regions: (88, 208) 

Shape of data for a single region: (208,) 



<img width="1012" height="255" alt="download" src="https://github.com/user-attachments/assets/2568810c-99a7-4933-8128-fc0057277a1c" />


Methods and results 

**1\. MCMC with PyMC** The analysis was based on the following distributions: 

● **phi (ɸ):** Normal (mu=0, sigma=1) 

● **sigma (**σ**):** HalfNormal(sigma=1) 

After the tuning phase of MCMC, 2000 samples were taken from the posterior distribution. 

1  
**Results of MCMC**

| | mean | sd | hdi_3% | hdi_97% | mcse_mean | mcse_sd | ess_bulk | ess_tail |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| phi | 0.865 | 0.034 | 0.800 | 0.929 | 0.000 | 0.000 | 8279.0 | 5832.0 |
| sigma | 1.129 | 0.056 | 1.022 | 1.230 | 0.001 | 0.001 | 8054.0 | 5622.0 |

| | r_hat |
| :---- | :---- |
| phi | 1.0 |
| sigma | 1.0 |

<img width="1175" height="497" alt="MCMC" src="https://github.com/user-attachments/assets/cbc33587-0b73-4e94-8290-d4358fba5e8b" />  


The MCMC analysis concluded that the signal has a strong positive autocorrelation. 

● **phi (**ϕ**):** The posterior is tightly centered at a mean of 0.86 (94% HDI: \[0.80, 0.93\]). 

● **sigma (**σ**):** The posterior is tightly centered at a mean of 1.1 (94% HDI: \[1.0, 1.2\]). 

The model is very confident that the signal has strong momentum and a moderate, consistent noise level. 


2  
**2\. SBI with VBI and SBI library** A simulator function for the AR(1) process was created. The analysis was based on 10,000 simulations, with a broad, uninformative prior for the parameters: 

● **phi (**ϕ**):** Uniform(low=-2.0, high=2.0) 

● **sigma (**σ**):** Uniform(low=0.0, high=2.0) 

A neural network was trained to approximate the posterior distribution based on the simulation results. 

**Results of SBI** The SBI analysis concluded that the signal has a strong negative autocorrelation. 

● **phi (**ϕ**):** The posterior is concentrated on negative values, with a peak around \-0.8. 

● **sigma (**σ**):** The posterior is pushed against the upper bound of the prior, with a peak near 2.0. 

The model concludes the signal is oscillating with a very high level of noise. The number of simulations was 20,000 for the 3-parameter model. 

The single brightest point on the heat map is the combination of `phi` and `sigma` that the model believes is the most probable. 

Comparing the values of phi and sigma for the two methods 

| Parameter  | MCMC Result  | SBI Result |
| ----- | ----- | ----- |
| **phi (**ϕ**)** | Positive (≈ 0.86)  | Negative (≈ \-0.8) |
| **sigma (**σ**)** | Moderate (≈ 1.1)  | High (≈ 2.0) |


3  
<img width="806" height="853" alt="SBI" src="https://github.com/user-attachments/assets/8fe3b545-4e3b-45b7-92b2-4cff35abbe73" />


The primary finding of this analysis suggests a significant discrepancy between the results of the two methods. This evidence suggests that for this specific model and data, the outcome could be sensitive to the method's underlying assumptions, particularly the prior distribution. The BOLD fMRI signal from the first region, particularly between t \= 10s and t \= 30s, exhibits smooth and trending behavior without rapid up-and-down oscillations, which would be indicated by a strong negative phi; instead, it supports the MCMC model’s conclusion of a high positive phi. 

### 

### 

### 

### **Code and Usage**

This analysis was conducted in a single Jupyter Notebook. The notebook is organized into two main sections: one for the MCMC analysis using PyMC and one for the SBI analysis using the `sbi` library.

To run this code, you will need a Python environment with the following key libraries installed:

* `pymc`  
* `sbi`  
* `vbi`  
* `torch`  
* `arviz`  
* `matplotlib`  
* `numpy`

The notebook is self-contained and can be run from top to bottom to reproduce the results presented in this report.
