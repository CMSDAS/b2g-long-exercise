---
title: "Obtaining upper cross section limits"
teaching: 10
exercises: 50
questions:
- "How do we use the 2DAlphabet package to set limits?"
- "Once I unblind, did we find new physics? If not, what can we say?"
objectives:
- "Use the 2DAlphabet package to set limits"
- "Provide expected sensitivity (upper cross-section limits) before unblinding as a function of resonance mass."
- "Unblind and compare observed to expected limits."
keypoints:
- "We can define expected sensitivity of an analysis with simulation-only by assuming the data expected will be exactly what the background estimation predicts."
- "We combine the systematic variations as a 1/2-sigma envelope around nominal expectation to quantify the confidence of our measurement."
- "After unblinding, if we have a down-fluctuation of data the expected exclusion appears stronger and vice-versa with up-fluctuations and apparently weak exclusions."
---

# Limits!

This is one of the big milestones in an analysis, setting limits! After un-blinding the limit plot, you can see
if you discovered new physics.

## Running the limit setting program

The limit setting program for 2DAlphabet needs to be run for each mass point under consideration and for each year.
The fitting program must be run on each year before the limit program can be run. So make sure that you completed
the exercise from the Modeling exercise that uses `run_MLfit.py`.

> ## Make Limits
>  
> First run the limit program for each of the mass points. There are 19 mass points between 1400 and 4000 (1400, 1600, ...),
> so it might be a good idea for each of you to choose 4 mass points to do and then combine the results together.
>
> ~~~bash
> python run_Limit.py configs_2Dalpha/input_data*.json --tag=LimitLH1400 --unblindData -d <name of dir from ML fit> signalLH2400:signalLH1400 --freezeFail
> ~~~
>
> - The directory we used in the previous example was `bsTest` so replace `<name of dir from ML fit>` with `bsTest` 
> (or whatever you choose to name it). 
> - tag is the name of the output directory.
> - "signalLH2400:signalLH1400" allows us to find:replace any strigns in the configs (this is useful for swapping 
> out one signal sample for another without having to write a new set of configs - in this case, "signalLH2400" is replaced with "signalLH1400").
> - `--freezeFail` which will set the "fail" bins to their post-fit values and then make them constant.
> - optionally unblind with `--unblindData`.
> 
> ~~~bash
> python set_limit.py -s signals.txt --unblind
> ~~~
>
> The `signals.txt` file uses commas and new lines to seperate the information. Here is an example for just three mass points.
> 
> ~~~
> LimitLH1400,  LimitLH1600,  LimitLH1800
> 1400,         1600,         1800
> 0.7848,       0.3431,       0.1588
> 1.0,          0.1,          0.1
> ~~~ 
>
> Here are the meaning of the rows in order:
> - Folder location where higgsCombineTest.AsymptoticLimits.mH120.root lives (this would be your --tag from the run_MLfit.py step)
> - Mass in GeV
> - Theoretical cross section for the theory line
> - Normalized cross section (values stored in bstar_config.json)
>
> Now use these steps to run the limit setting program on all 19 mass points.
> 
{: .challenge}

> ## Understanding the limits
> Did you discover new physics? How do you know?
>
{: .testimonial}

