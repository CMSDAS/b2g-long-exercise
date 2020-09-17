---
title: "Developing a pre-selection"
teaching: 10
exercises: 50
questions:
- "What cuts could you apply that follow the signal topology"
- "What are background processes that could enter this selection?"
- "How do I normalise background processes?"
objectives:
- "Develop a signal selection cutflow."
- "Make stack plots of background processes."
- "Make N-1 plots to understand the effect of selection cuts."
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---
# Introduction

A preselection serves two purposes, first to ensure that passing events only utilize a "good" region of the detector with
appropriate filters and second to start applying simple selections motivated by the physics of the signal topology. 

## Defining a "Good" Region of the Detector

A "good" region of the detector depends heavily on the signal topology. The muon system and tracker extend to about 
eta = 2.4 while the calorimeter extends to eta = 3.0 with the forward calorimeter extending further. Thus, if the signal
topology relies heavily on tracking or muons, then a useful preselection would limiting the region to eta < 2.4. Some 
topologies, like vector boson fusion (commonly called VBF) have two forward (high eta) jets, so placing a preselection 
that requires two forward jets is a useful preselection.

> ## Exercise (5 min)
> Now, let's take a more detailed look at our signal topology and see how it fits in with the detector. The b-star is produced 
from the interaction of a bottom quark and a gluon, will this production mode yield any characteristic forward jets?
> In this topology, the b-star decays to jet from a W boson and jet from top quark. What is characteristic of a top jet decay?
What about a W jet? How does this impact the region of the detector needed? What eta and phi in the detector do we need? 
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
> > restrict |eta| < 2.4. There are no detector differences in phi that should impact this search, so there should be no restriction
> > in phi.
> > {: .source}
> {: .solution}
> {: .source}
{: .callout}

## Finding Appropriate Filters




## Simple Selections

The preselection should also include a set of simple selections based on our physics knowledge of the signal topology. Consider a 
heavy resonance decaying to two Z bosons that produce jets to create a dijet final state. In this case, the energy of the collision
would go into producing a heavy resonance with little transverse momentum, so conservation of momentum tells us that the jets should
be well separated in phi, ideally they should have a separation of pi in phi.

In the preselection, these selections should be relatively light and simple stacked "N-1" histograms should be utilized to ensure 
that the preselection is not causing a signal loss. More about these plots is described below.

# Applying our selection and monitoring the MC response

## Stack Plots to monitor signal and background

## N-1 plots

{% include links.md %}

