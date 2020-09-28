---
title: "Developing a pre-selection"
teaching: 10
exercises: 50
questions:
- "What is the purpose of a preselection?"
- "What loose cuts could you apply that follow the signal topology?"
- "What are background processes that could enter this selection?"
- "How do I normalise background processes?"
objectives:
- "Define a 'good' region of the detector and apply MET filters."
- "Develop a simple signal selection cutflow."
- "Make stack plots of background processes."
keypoints:
- "Preselection reduces data size, but further signal optimization is done later"
- "Preselected events should be in good regions of the detector with appropriate filters"
- "Stacked histograms are an important tool for creating cuts"
---
Recording files of this session are in [cernbox](https://cernbox.cern.ch/index.php/s/uibnZgUrfC0HAqn)

# Introduction

A preselection serves two purposes, first to ensure that passing events only utilize a "good" region of the detector with
appropriate noise filters and second to start applying simple selections motivated by the physics of the signal topology. 

## Defining a "Good" Region of the Detector

A "good" region of the detector depends heavily on the signal topology. The muon system and tracker extend to about 
|&eta;| = 2.4 while the calorimeter extends to |&eta;| = 3.0 with the forward calorimeter extending further. Thus, if the signal
topology relies heavily on tracking or muons, then a useful preselection would limiting the region to |&eta;| < 2.4. Some 
topologies, like vector boson fusion (commonly called VBF) have two forward (high eta) jets, so placing a preselection 
that requires two forward jets is a useful preselection.

<img src="../fig/cmsDetectorCrossSection.png" alt="cmsDetectorCrossSection" style="width:400px"> 

> ## Discuss (5 min)
> Now, let's take a more detailed look at our signal topology and see how it fits in with the detector. The b-star is produced 
from the interaction of a bottom quark and a gluon, will this production mode yield any characteristic forward jets?
> In this topology, the b-star decays to a jet from a W boson and a jet from a top quark. What is characteristic of a top jet?
What about a W jet? How does this impact the region of the detector needed? What |&eta;| and &phi; in the detector do we need? 
Think about this while looking at the Feynman diagram and the signal topology.
>
> <img src="../fig/bstarFeynman.png" alt="bstarFeynman" style="width:200px"> 
> <img src="../fig/bstarTopo.png" alt="bstarTopo" style="width:200px"> 
> 
> > ## Solution
> > The production mode does not have any characteristic forward jets, but the final state has two jets. The top quark decays to a 
> > b jet and W jet, where the b jet is typically identified by making use of it's characteristic secondary vertex. This secondary
> > vertex is identified in the tracker. Both the W jet and top jet have unique substructure that can be used to distinguish them 
> > from QCD jets. Therefore it is crucial to use a region of the detector with good tracking and granular calorimetery,so we should
> > restrict |&eta;| < 2.4. There are no detector differences in phi that should impact this search, so there should be no restriction
> > in &phi;.
> {: .solution}
{: .challenge}

## Finding Appropriate MET Filters

Missing transverse momentum (called MET) is used to identify detector noise and MET filters are used to remove detector noise. The 
MET group publishes recommendations on the filters that should be used for different eras of data.

> ## Exercise (5 min)
> The recommended MET filters for Run II are listed on this [twiki](https://twiki.cern.ch/twiki/bin/viewauth/CMS/MissingETOptionalFiltersRun2).
> Use this [twiki](https://twiki.cern.ch/twiki/bin/viewauth/CMS/MissingETOptionalFiltersRun2) to create a list of MET filters to use in the preselection.
{: .challenge}


## Simple Selections

The preselection should also include a set of simple selections based on our physics knowledge of the signal topology. These "simple"
selections typically consist of loose lower bounds only, which help to reduce the number of events which will get passed to the rest of
the analysis while still preserving the signal region. 

Consider a heavy resonance decaying to two Z bosons that produce jets to create a dijet final state. In this case, the energy of the 
collision would go into producing a heavy resonance with little longitudinal momentum, so conservation of momentum tells us that the jets should
be well separated in &phi;, ideally they should have a separation of &pi; in &phi;. Therefore placing a selection of &Delta;&phi; > &pi;/2 should 
not cut out signal, but will reduce the number of events passed on to the next stage. 

> ## Reflect
> Why not use a selection close to &Delta;&phi; = &pi;?
> > ## Solution
> > The jets can recoil off of other objects creating a dijet pair that is less than &Delta;&phi; = &pi;
> {: .solution}
{: .callout}

This also a good stage to place a lower limit on the jet p<sub>T</sub>. In a hadronic analysis, it is common to place a high lower limit on the p<sub>T</sub>.
For this example, a lower limit of p<sub>T</sub> = 400 GeV should be good.

> ## Reflect
> Why place such a high lower limit on jet p<sub>T</sub>?
> > ## Solution
> > This is a tricky question. It relates to the trigger. Hadronic triggers have a turn on at high H<sub>T</sub> or high p<sub>T</sub>, so the 
> > lower limit ensures that that the analysis will only investigate the fully efficient region.
> > <img src="../fig/triggerTurnOn.png" alt="triggerTurnOn" style="width:200px"> 
> {: .solution}
{: .callout}

A jet originating from a Z boson should also have two "prongs" (regions of energy in the calorimeter), these "prongs" are part of the jet
substructure discussed in the earlier lessons. For a two pronged jet like a Z jet, it is good to place a lower limit on the &tau;<sub>21</sub> ratio. 
Another useful substructure variable to use in the preselection is the softdrop mass. The softdrop algorithm will help to reduce the amount
of pileup that is used when measuring the jet mass. The preselection is a good place to define a wide softdrop mass region. For this example,
a wide region around the Z boson mass would be ideal, such as 65 < m<sub>SD</sub> < 115 GeV. 

It is important to emphasize that the preselections should be relatively light. It is important to check that the preselection is not eliminating
large amounts of signal. A good way to monitor this is to utilize stacked histograms. More about these plots is described below.

> ## Discuss (5 min)
> Again, use the images above to think about the signal topology. What "simple" selections can be used in the preselection? Any
&Delta;&phi; or p<sub>T</sub> criteria? What about substructure?
>
> 
> > ## Solution
> > In this signal topology the t and W should be well separated, so a light &Delta;&phi; cut should be placed. Think about a reasonable 
selection and investigate the result in the plotting exercise. Same for the jet p<sub>T</sub>. Both the top jet and the W jet should have substructure.
> > The top jet should have three prongs and W jet should have two prongs. Think about the softdrop regions and n-subjettiness (&tau;) 
ratios that should be used and investigate them in the plotting exercise.
> {: .solution}
{: .challenge}

# Applying our selection and monitoring the MC response

When applying the preselection, the selections will be placed serially in the code creating a "cutflow". The filters are applied first 
to ensure that the data was taken in "good" detector conditions. Then the kinematic/substructure cuts are applied. It is important to 
monitor the signal and background in between these physics inspired cuts.

> ## Exercise (20 min) Plots to Monitor Signal and Background
> Find where the filters are applied in the `exercises/ex4.py` script, check that all the filters are there, and then create a histogram 
displaying &tau;<sub>21</sub> &tau;<sub>32</sub> for the leading and subleading jet. This can be done using the `exercises/ex4.py` script 
from the `BstarToTW_CMSDAS2020` repository. 
> ~~~bash
> cd CMSSW_11_0_1/src # where you saved the TIMBER and BstarToTW_CMSDAS2020 repositories during the setup
> cmsenv
> python -m virtualenv timber-env
> source timber-env/bin/activate
> python exercises/ex4.py -y 16 --select
> ~~~
>
> What criteria would you use for &tau;<sub>21</sub>? What about &tau;<sub>32</sub>?
> 
> > ## Solution
> > The filters are listed as flags
> > ~~~python
> > flags = ["Flag_goodVertices",
> >        "Flag_globalTightHalo2016Filter", 
> >        "Flag_eeBadScFilter", 
> >        "Flag_HBHENoiseFilter", 
> >        "Flag_HBHENoiseIsoFilter", 
> >        "Flag_ecalBadCalibFilter", 
> >        "Flag_EcalDeadCellTriggerPrimitiveFilter"]
> > ~~~
> > Then they are applied using the `Cut` function
> >
> > ~~~python
> > # Initial cuts
> > a.Cut('filters',a.GetFlagString(flags))
> > ~~~
> > 
> > Now, we want make histograms for m<sub>SD</sub>, &Delta;&phi;, and leading/subleading jet p<sub>T</sub>. 
> >
> > Let's modify the `ex4.py` script to include leading jet p<sub>T</sub>. To do this, first add a new string
to `varnames`. Then, define the new quantity. Finally, add an if statement to adjust the bounds of the histograms.
> > ~~~python
> > # To the varnames
> > 'lead_jetPt':'Leading Jet p_{T}',
> >
> > # To the definition
> > a.Define('lead_jetPt','FatJet_pt[jetIdx[0]]')
> > 
> > # add an if statement by the hist_tuple
> > if "tau" in varname :
> >     hist_tuple = (histname,histname,20,0,1)
> > if "Pt" in varname :
> >     hist_tuple = (histname,histname,30,400,1000)
> > ~~~
> > 
> > The histograms for m<sub>SD</sub>, &Delta;&phi;, and subleading jet p<sub>T</sub> are left as homework.
> {: .solution}
>
{: .challenge}

> ## Homework 
> Make histograms for m<sub>SD</sub>, &Delta;&phi;, and subleading jet p<sub>T</sub>. Then, use these plots to agree 
> on a preselection using the discord chat (Or choose your own if this is after DAS).
> Finally, add your groups' decided on preselection to the `bs_select.py` script in `BstarToTW_CMSDAS2020`
>
{: .testimonial}


{% include links.md %}

