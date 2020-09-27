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
Now that we understand what the minimal selection is that we want to apply to our signal and background, we need to think harder about what are the final (tighter) selections that we want to apply to define our signal and control regions.

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
