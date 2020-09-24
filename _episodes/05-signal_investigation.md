---
title: "Investigating the signal topology"
teaching: 15
exercises: 45
questions:
- "What is the signal we are trying to select for?"
- "What observables should be investigated for our final state?"
- "What do kinematics in the signal look like?"
- "How do observables change as a function of resonance mass?"
- "Is there an observable that directly probes the signal?"
objectives:
- "Familiarisation with the ntuple format."
- "Plot several kinematic variables at parton, particle, and reconstructed level. (Bonus: Investigate relative resolution)"
- "Reconstruct the invariant resonance mass."
keypoints:
- "In an all-hadronic state, we veto leptons (electrons/muons) and study jet properties."
- "Jets are clustered Particle Flow candidates interpreted as a 4-vector with momenta, energy, and mass."
- "The resonance searched decays to all particles in final state, thus if we add all jets in vector-form we can reconstruct the resonance."
---
One of the first questions we should ask ourselves is 'what does our signal look like?' Understanding the answer to this will help us decide what selections we should apply to keep as much of the signal as we can, while rejecting as much background as possible. Before making any decisions, we should open up the signal sample we have and get ourselves acquainted with what information is available to us. Sometimes, samples are 'slimmed' (removal of unnecessary infomation/branches) or 'skimmed' (application of loose selections to reduce number of events) in order to reduce the size of the sample files.

To check the information inside a sample, open it up with root and checkout it's branches
~~~bash
root -b xrdcp root://cmsxrootd.fnal.gov//eos/store/lcorcodilos/samples/signal.root
.ls
myTree->Print()
myTree->Scan("JetPt")
myTree->Scan("JetPt","JetPt<400")
myTree->Scan("JetPt","JetPt<300")
myTree->Draw("JetPt")
~~~
{: .source}

There are more sophisticated ways to investigate the branches available as well as their min/max values in root and python.

> ## Question: Can you find any previous selections that have been applied to the signal sample? If so, why do you think they were done?
>
> > ## Solution
> > The jet pT has been required to be 350 GeV, and the absolute value of the jet eta is 2.5.
> > 
> {: .solution}
{: .challenge}

Bonus: Write a python script that reads the signal sample and outputs each branch name along with the min/max value of that branch into a txt file.
Bonus+: Write a pyRoot script that reads the signal sample and outputs plots of each branch, using the min/max values you found above in the binning definition. 

You may notice that some variables appear in several variations, e.g. JetPt, PFcandsPt, truthTopPt. What are the different types of collections available (reconstructed, particle, parton, truth, etc.)?

Bonus++: Plot the relative resolutions of the reconstructed objects you have with respect to their truth values. For example, the relative resolution of jet transverse momentum is JER=(recoJetPt-truthJetPt)/truthJetPt

Finally, let's look into one of the most important observables of the signal: the effective mass of the t+W system. What does this effective mass mean? Does it represent anything physical? Before plotting, what do you think the distribution should look like, and why?
