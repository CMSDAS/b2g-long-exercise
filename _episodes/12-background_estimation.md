---
title: "Background Estimation: 2DAlphabet"
teaching: 10
exercises: 50
questions:
- "How are we going to estimate the background?"
- "What is 2DAlphabet?"
objectives:
- "Familiarize with 2DAlphabet method"
keypoints:
- "We can simultaneously fit several backgrounds by defining the appropriate observables/axes, i.e. m<sub>t</sub> vs m<sub>tW</sub>."
- "2D Alphabet is a method of fitting a polynomial function to a space (except the SR), to model the multi-jet background, and also serves as an interfae to other tools." 
---

## Idea of 2D Alphabet
Normally, we do a shape hunt along some observable and measure our background is near-by kintemtic regions to estimate its expected yield in the signal region. For our case, there is a clever way to do all these tasks simultaneously. Since we are scanning across m<sub>tW</sub>, with a window designated as a Signal Region, we can use all the non-window space to fit a smooth function that extrapolates over the SR window.
<img src="../fig/2DAlphabetShape.png" alt="2DAlphabetShape" style="width:500px"> 

Since we define our SR as passing exactly one top tag (most importantly with a mass window cut), we can use the adjacent region of passing 2 top tags for a ttbar/singletop constrol region. We can then use the smooth function from above to estimate the multi-jet contribution to the 2-top measurement region. Whereas the multi-jet contribution's shape is determined by the smooth function fit, the shape of the ttbar and singletop background is set by simulation and the normalization is allowed to float.

These steps are all done simultaneously; as the multijet estimation fit optimizes, the ttbar/singletop region feeds back its normalizations to subtract from the data used in the fit optimization.
<img src="../fig/2DAlphabetFit.png" alt="2DAlphabetFit" style="width:500px"> 
