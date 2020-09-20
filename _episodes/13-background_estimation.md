---
title: "Background estimation"
teaching: 10
exercises: 20
questions:
- "Now with a final signal selection, how can we estimate the backgrounds?"
- "What are commonly used background estimation methods?"
- "When are data-driven methods used and why?"
objectives:
- "Identify final background processes"
- "Decide whether MC can be used as is, or if data-driven method would be better."
- "Define control regions."
- "Implement a background estimation technique for each process."
keypoints:
- "BGs: ttbar, Z+Jets, QCD, ttZ"
- "MC for ttbar is ok. QCD is notoriously bad, so we use data-driven methods." 
- "Since we are confusing the 'jets' part of Z+Jets with a top in the SR, this corner of the simulation suffers from the same aches as QCD and we also use a data-driven technique."
- "2DAlphabet is a novel technique to simultaneously estimate QCD and Z+Jets backgrounds."
---
