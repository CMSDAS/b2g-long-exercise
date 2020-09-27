---
title: "Optimising the analysis"
teaching: 30
exercises: 60
questions:
- "What aspects of our analysis do we want to optimize?"
- "How can we quantify selections to help decide SR/CR definitons."
- "Do some variables affect signal and BG differently/similarly?"
- "Are there any correlated varibles?"
- "What final selections are going to be applied to the analysis"
objectives:
- "Identify optimizable parts of analysis."
- "Use Punzi significance and other measures to optimise selections."
- "Obtain a close-to-optimal selection to define SR."
keypoints:
- "Preselection is not enough to be sensitive to signal, we need to tighten selection to increase significance of signal to background."
- "The traditional way of optimizing selections is to apply N-1 (all-but one) cuts and findng peak of significance curve in removed cut."
- "Boosted Decision Trees and other multivariate optimization techniques are also widely used."
---

## Setup Ahead the Session
To more efficiently help with debugging, we are going to use remote access of your terminal through [tmate](https://tmate.io/). The way this application works is that you can initialize it (by calling 'tmate') and it'll start a session of your terminal that is viewable and editable online. In the website linked it shows how that looks like. If you feel comfortable letting the facilitators use this with you, follow the steps below to install tmate. 

To download this you'll have to use homebrew (another application). To check if you have homebrew installed, 
~~~bash
brew help
~~~
{: .source}

If you get some output, you're set. Otherwise it'll give you an error saying the 'brew' command doesn't exist. In that case, download [homebrew](brew.sh) by
~~~bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
~~~
{: .source}

Once installed, use homebrew to install tmate by
~~~bash
brew install tmate
~~~

In addition, pull all the latest changes from the repositories:
~~~bash
cd ~/CMSVDAS2020/b2g-long-exercise/
git fetch --all
git pull origin gh-pages
cd ../CMSSW_11_0_1/src/
rm -rf timber-env
cmsenv
virtualenv timber-env
source timber-env/bin/activate
cd TIMBER/
git fetch --all
git checkout cmsdas_dev
python setup.py install
source setup.sh
cd ../BstarToTW_CMSDAS2020
git fetch --all
git pull origin master
~~~

## Wrapping-Up From Before

> ## Question: What is our preselection?
> Generally your preselection should include the intersection of your signal and control regions, cutting out unnecessary data.
> Discuss for the next ~15 minutes what the preselection should be. Feel free to use your plots as evidence supporting your argument. 
> Think about what the preselection is supposed to be cutting on (e.g., remember to leave space for estimating the BG).
>
> > ## Solution
> >
> > The pre-selections chosen for the all-had b*->tW (B2G-19-003) analysis were
> > - Standard filters and JetID
> > – p<sub>T</sub>(t), p<sub>T</sub>(W) > 400 GeV
> > - |&eta;| < 2.4
> > > 2.4
> > - |&Delta;&phi;| > &pi;/2
> > - |&Delta;y| < 1.6
> > - W-tag: $tau;<sub>21</sub> < 0.4/0.45 and 65 < m<sub>SD</sub> < 105 GeV
> > - (Later) m<sub>tW</sub> > 1200 GeV
> > {: .output}
> {: .solution}
{: .challenge}

## Optimizing: But How?
Perhaps you may have already thought 'How do I make the *best* cuts to the variables available to me?' as you were deciding your preselection. 

## Tightening Selections in an Optimal Way

Now that we understand what the minimal selection is that we want to apply to our signal and background, we need to think harder about what are the final (tighter) selections that we want to apply to define our signal and control regions.

> ## Question: What is a control region and why do we need it?
>
> What are some parts of the analysis you think could be optimized?
>
> > ## Solution
> >
> > The selections could be further tightened from what we decided for preselection. 
> >
> > {: .output}
> {: .solution}
{: .challenge}


> ## Question: What should we optimize?
>
> What are some parts of the analysis you think could be optimized?
>
> > ## Solution
> >
> > The selections could be further tightened from what we decided for preselection. 
> >
> > {: .output}
> {: .solution}
{: .challenge}

There are many variables that behave similarly in signal and the background processes, cutting on such variables may not be beneficial since you would need to cut hashly on signal to remove background. Sometimes, the variables can help discriminate more obviously but the exact value which to cut on may be hard to identify. 

> ## Question: How can we quantify an optimal selection of a variable?
>
> > ## Solution
> >
> > What we are trying to do is keep signal and reject background. 
> > The basic significance measure is signal/sqrt(BG), and we want to plot how this significance changes as you cut harder on the variables.
> > Try generating these plots and finding the values on the variables to cut on to maximize the measure of significance.
> >
> > {: .output}
> {: .solution}
{: .challenge}
