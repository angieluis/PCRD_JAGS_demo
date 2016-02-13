# PCRD_JAGS_demo
A full-capture Hierarchical Bayesian model of Pollock's Closed Robust Design and application to dolphins

This contains a tutorial of several Robust Design (RD) Bayesian Mark-Recapture models, as presented in the paper "Rankin RW, Nicholson K, Allen SL, Krützen M, Bejder L, Pollock KH. 2016. A full-capture Hierarchical Bayesian model of Pollock's Closed Robust Design and application to dolphins. Frontiers in Marine Science, in Press". This tutorial also uses data directly from the parent publication: "Nicholson K, Bejder L, Allen SL, Krützen M, and Pollock KH. 2012. Abundance, survival and temporary emigration of bottlenose dolphins (Tursiops sp.) off Useless Loop in the western gulf of Shark Bay, Western Australia. Marine and Freshwater Research 63:1059–1068." Please cite both these studies if using this code and data (See BIBTEX citations at the end of this read me) 

The RD models are useful for the analysis of marked animals, estimating their abundance and demographic parameters like survival and movement. Importantly, the Hierarchical Bayesian model creates a flexible modelling environment to address several perreniel issues encountered in analogous 'fixed-effects' and Frequentist models, such as a type of 'model-averaging' and addressing individual heterogeneity.

<b> Dependencies </b>

This demo assumes the user has both R and JAGS installed, as well as the R package "rjags" to communicate between the two. See http://cran.r-project.org/ and http://mcmc-jags.sourceforge.net/. Most Linux distro have R/JAGS binaries available directly in standard repos. For example, Ubuntu users can simply by type:
> sudo apt-get update
> sudo apt-get install r-base-core r-base-dev jags
Once in R, type: install.package("rjags")

<b> Files in the tutorial </b>

There are 3 tutorials in 3 R files. Each tutorial presents a slightly different Bayesian model:
 - R_PCRD_JAGS_firstcapt_fixedeff.R 
 - R_PCRD_JAGS_fullcapt_fixedeff.R 
 - R_PCRD_JAGS_hierarchical.R # USERS SHOULD START HERE!
The first file is a tutorial for Pollock's Closed Robust Design, conditioning on first capture and is a "fixed-effects" version. The 2nd file is also a fixed effects version, but models individual entire capture-history, thereby facilitating inference of recruitment processes (births). The 3rd file is a Hierarchical Bayesian version, which will be of interest to most users: it can accomodate individual heterogeneity, full-capture modelling, and employs a peculiar type of "hyperprior" to induce a desireable shrinkage between fully time-varying parameters and time-constant parameters. In the companion paper, the authors argue that this hyperprior specification induces a type of "model-averaging" somewhat similar to the AICc-averaging that is ubiquitous in Mark-Recapture studies (thanks to Program MARK). 

Other files are:
 - mark_capture_histories.inp
 - R_PCRD_JAGS_SOURCE.R
 - varous ".BUG" files. 
The .inp file is a MARK data file containing capture histories for 5 years of photo-ID studies of the Bottlenose dolphins in Western Gulf Shark Bay, as presented originally in "Nicholson et al. 2012." Please cite Nicholson when using this data. The SOURCE file contains some handy auxilliary functions to make it easier to initialize the JAGS models (especially generating sensible initial values for the latent states of the HMM). The BUG files are automatically generating during the above tutorial R scripts, and are not strictly necessary; they have the raw JAGS syntax for each JAGS model.

<b> Getting Started </b>

After cloning the appropriate model, users can re-analysis the data from Rankin et al. (2016) and Nicholson et al. (2012) by simply stepping through the R and JAGS code.

<b> Customizing For Other Studies </b>
Users interested in these models for their own purposes will first need to decide: i) use the fixed-effects models, or ii) the Hierarchical model. 

Users of the Hierarchical model will likely want to change the "hyperparameters" that govern the amount of shrinkage between time-varying and time-constant parameterizations. The hyper-parameters are passed to jags as 'data', called variously "pr.tauphi","pr.taug1","pr.taug2","pr.taupdmu","pr.taupd2nd","pr.taueps" in the JAGS syntax. Each of these is employed in a particular type of hyperprior called the Scaled Half Student-T with hyperparameters (mu,tau,nu), that has good shrinkage-inducing properties. The half Student-t shrinks a dispersion parameter to zero, then then enforces a time-constant parameterization for a particular parameter. For example, "pr.tauphi" will control the size of "sigma.phi", such that if "sigma.phi" is close to zero, the apparent survival parameter "phi" will be nearly time-invariant, and shrunk to its mean "phi.mu". Conversely, when "sigma.phi" is not zero, then "phi" will be time-varying, perhaps close to the Maximum Likelihood Values. The amount of shrinkage is controlled partiall by the hyperparameters pr.tauphi=c(mean=0,tau=1/0.3^2,nu=2). Large values of 'tau' set the (approximate) prior expectation of 'sigma.phi', while 'nu' controls the tail, and such that small values allow the data more lee-way in deciding the value of 'sigma.phi'. We recommend tau=1/0.3^2 and nu=2 as a 'default' value for those parameters (especially detection probabilities and gamma-double-prime)

The above are hyperpriors, and therefore should be motivated by one's prior knowledge. However, 

Users of the fixed-effects models will then need to tweak the priors of the various parameters for their own purposes, and perhaps change which parameters are time-varying vs. time-constant. Currentl
