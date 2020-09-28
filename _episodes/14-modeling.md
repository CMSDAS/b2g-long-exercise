---
title: "Modelling the background with 2DAlphabet (a wrapper for Combine)"
teaching: 10
exercises: 20
questions:
- "How do I use the background model in a statistical analysis?"
objectives:
- "Have the background model implemented in RooFit/Combine."
- "Perform background fits to pseudo data and data control regions."
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---

# Introduction to the 2DAlphabet package

2DAlphabet uses `.json` configuration files. Inside of these the are areas to add uncertainties which are used 
inside of the combine backend. The 2DAlphabet serves as a nice interface with combine to allow the user to use 
the 2DAlphabet method without having to create their own custom version of combine.

## A look around

The configuration files that you will be using are located in the `BstarToTW_CMSDAS2020` repository in this path `BstarToTW_CMSDAS2020/configs_2Dalpha/`
Take a look inside one of these files, for example: `input_dataBsUnblind16.json`

The options section. What does it do? [FIXME]

The process section is basically the same as the process section in combine, but laid out in an easier to read format.
Each process refers to a root file containing data or Monte Carlo simulated data. A `"CODE"` of > 0 means that the process is a background event
a `"CODE"` of <=0 means that a process is a signal event. The `"SYSTEMATICS"` list contains the names of the systematic 
uncertainties for that process. The systematic uncertainties are assigned values in the `"SYSTEMATIC"` section. 
[FIXME] explain HISTPASS and HISTFAIL

## Running 2DAlphabet

In order to run the 2DAlphabet package on our json configuration files, we will first make a symbolic link to
the configuration files to make things easier. 

~~~bash
# start inside the directory containing the BstarToTW_CMSDAS2020 repo
cd CMSSW_11_0_1/src/BstarToTW_CMSDAS2020/
# Now move to the directory containing the 2DAlphabet
cd ../../../CMSSW_10_6_14/src/2DAlphabet
# now create a symlink (symbolic link) to the config file directory
ln -s ../../../CMSSW_11_0_1/src/BstarToTW_CMSDAS2020/configs_2Dalpha/ configs_2Dalpha
~~~

Now run one of the configuration files.

~~~bash
cmsenv
# now run one of the files 
python run_MLfit.py configs_2Dalpha/input_dataBsUnblind.json --tag=bsTest
~~~

This will create some output plots and combine cards. The output is stored in `bsTest/SR16/`.

# Uncertainties

Write some stuff about uncertainties. [FIXME]

## Where to add the uncertainties

Let's take a look at adding uncertainties in the `.json` files. The systematic uncertainties are defined in the `SYSTEMATIC` section:

~~~json
"SYSTEMATIC": {
    "HELP": "All systematics should be configured here. The info for them will be pulled for each process that calls each systematic. These are classified by codes 0 (symmetric, lnN), 1 (asymmetric, lnN), 2 (shape and in same file as nominal), 3 (shape and NOT in same file as nominal)",
    "lumi16": {
        "CODE": 0,
        "VAL": 1.025
    },
~~~

So in our json file, we can list a systematic and use the `"CODE"` to describe the type of systematic that will be used in combine.
The help message describes the options. Here, lnN refers to log-normal normalisation uncertainty.

As an example take a closer look at the `"lumi16"` systematic. It is a symmetric uncertainty `"VAL": 1.025`. In this context, 
`"VAL"` refers to effect that the systematic has. In this case 1.025 means that this systematic has a 2.5% uncertainty on the yield.

For the moment, let's just look at our symmetric uncertainties and later we will look at a shape uncertainty. 

## Luminosity uncertainties

As we saw in the previous example, the luminosity is a symmetric uncertainty. Each year, the uncertainty on the luminosity changes. This
is because the measurement of delivered luminosity is per year.


> ## Discuss (5 min)
> Should each Monte Carlo sample have a different luminosity uncertainty? 
>
> > ## Solution
> > Monte Carlo samples for the same year should all have the same luminosity uncertainty, because the overall luminosity delivered by the
> > detector in that year is a fixed measurement.
> {: .solution} 
{: .challenge}

## Cross section uncertainties

The cross section uncertainty is another symmetric uncertainty added to each Monte Carlo sample.

> ## Discuss (5 min)
> Should the signal Monte Carlo sample have a cross section uncertainty?
> > ## Solution
> > This is because the “cross section” of the signal is exactly what we want to fit for (ie. the parameter 
> > of interest/POI/r/mu)! One might ask then why we assign an uncertainty from the luminosity to the signal 
> > when the luminosity is just another normalization uncertainty. The answer is that the luminosity uncertainty 
> > is fundamentally different from the cross section uncertainties because it is correlated across all of 
> > the processes - if it goes up for one, it goes up for all. The cross section uncertainties though are 
> > per-process - the ttbar cross section can go up and the single top contribution will remain the same.
> {: .solution}
{: .challenge}

With just the luminosity and cross section uncertainties, we only have normalization uncertainties accounted 
for and there are certainly uncertainties which affect the shapes of distributions (our simulation is not perfect 
in many ways). These can be accounted for via vertical template interpolation (also called “template morphing”). 
The high-level explanation is that you can provide an “up” shape and a “down” shape in addition to the “nominal” 
and the algorithm will map “up” to +1 sigma and “down” to -1 sigma on a Gaussian constraint. The first of these
"shape" uncertainties is the Top p<sub>T</sub> uncertainty.

## Top p<sub>T</sub> uncertainties

The TOP group has recommendations for how to account for consistent but not-understood disagreements in the 
top p<sub>T</sub> spectrum between ttbar simulation and ttbar in data (see this 
[twiki](https://twiki.cern.ch/twiki/bin/viewauth/CMS/TopPtReweighting) for more information). This analysis 
falls into Case 3.1 from that [twiki](https://twiki.cern.ch/twiki/bin/viewauth/CMS/TopPtReweighting). The 
short version of the recommendation is that one should measure the top p<sub>T</sub> spectrum in a dedicated 
ttbar control region which is exactly what the ttbar measurement region of the analysis is for.  

For this analysis the top jet mass window is 105-220 GeV which constrains the ttbar background but does not 
leave a lot of room to work with when blinded. Fortunately, m<sub>tt</sub> is correlated with top p<sub>T</sub> 
so we can just measure the correction in the 2D space while also fitting for QCD and all of the other systematics 
in the ttbar measurement region and signal region!  

> ## Bonus: Analysis details on decisions
> The decision of the correction for the ttbar top p<sub>T</sub> spectrum was pretty simple. We just took the 
> functional form (for POWHEG+Pythia8) farther down and rewrote it as e<sup>(&alpha;*0.0615 - &beta;*0.0005*p<sub>T</sub>)</sup> 
> where &alpha; and &beta; are nominally 1 (so just the regular correction on that page). Then each &alpha; and 
> &beta; are individually varied to 0.5 and 1.5 to be our uncertainty templates for the vertical interpolation. 
> These are what are assigned to Tpt-alpha and Tpt-beta. For example, if the post-fit value of Tpt-alpha was 1 and Tpt-beta was -1, 
> the final effective form of our Tpt reweighting function would be e<sup>(1.5*0.0615 -  0.5*0.0005*p<sub>T</sub>)</sup>.
{: .testimonial}

> ## Exercise
> Add the Top p<sub>T</sub> uncertainties to the appropriate processes and see the changes in combine.
> 
> The uncertainties are already included in the section on `"SYSTEMATICS"`. However, they have not been
> added to the individual processes. To start this exercise, add the `Tpt-alpha` and `Tpt-beta` uncertainties
> to the appropriate processes. Which processes should these uncertainties be added to? Then add in the
> the uncertainties to the appropriate processes and rerun the 2DAlphabet. Compare the combine cards
> using `diff` to see the changes before and after adding in the top p<sub>T</sub> uncertainty.
>
> Do this exercise for `input_dataBsUnblind16.json` and then after looking at the solution, apply to the other 
> config files.
>  
> > ## Solution
> > 
> > The Top p<sub>T</sub> uncertainties should be applied to the `ttbar` and `ttbar-semilep` processes. For example:
> >
> > ~~~json
> > "ttbar": {
> >     "TITLE":"t#bar{t}",
> >     "FILE":"path/TWpreselection16_ttbar_tau32medium_ttbar.root",
> >     "HISTPASS":"MtwvMtPass",
> >     "HISTFAIL":"MtwvMtFail",
> >     "SYSTEMATICS":["lumi16","ttbar_xsec","Tpt-alpha","Tpt-beta"],
> >     "COLOR": 2,
> >     "CODE": 2
> > }     
> > ~~~
> >
> > Now, keep the data card made before doing this change and then run the code to see the difference.
> >
> > ~~~bash
> > # From the 2DAlphabet directory, store the old card
> > cp bsTest/SR16/card_SR16.txt no_topUn_SR16.txt
> > # Then, rerun the 2DAlphabet
> > python run_MLfit.py configs_2Dalpha/input_dataBsUnblind16.json --tag=bsTest
> > # Then look at the difference
> > diff no_topUn_SR16.txt bsTest/SR16/card_SR16.txt
> > ~~~
> > 
> > To understand more about what is happening in combine and how this shape uncertainty is applied see this
> > [DAS exercise](https://cmsdas.github.io/statistics-short-exercise/#combine-part-2-shape-based-analysis)
> >
> > Now that this change has been made for one process in one configuration file, apply the same change for
> > all `ttbar` and `ttbar-semilep` processes in all 6 configuration files.
> >
> > Note that `ttbar` is all-inclusive for 2016 and hadronic only for 2017 and 2018 so the `ttbar_semilep` is included only in 2017 and 2018.
> {: .solution}
{: .challenge}


> ## Bonus Exercise: Trigger Efficiency uncertainty
>
> The trigger efficiency uncertainty is defined as a systematic in the configuration files. As a bonus exercise, add 
> this shape systematic uncertainty to the processes. How do you measure Trigger efficiency? And how is this uncertainty
> calculated?
> 
> This exercise is a bonus and not necessary to continue on. 
>
{: .callout}



