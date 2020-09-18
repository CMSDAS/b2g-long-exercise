---
title: "Introduction to All-Hadronic b*->tW and Useful Tools Like CI/CD"
teaching: 20
exercises: 40
start: true
start_time: 660
questions:
- "Who are you?" 
- "What is this exercise about?"
- "Why search for heavy resonances?"
- "What is CI/CD?" 
objectives:
- "Familiarize ourselves with peers and resources."
- "Contexualize a CMS-style search and its stages"
- "Understand motivation for physics at high masses/scales."
- "Begin making new habits with good coding practices."
keypoints:
- "First level of help is within yourselves. Ask your teammate questions you are asking yourself."
- "Ask your facilitators for help if you feel stuck, the point of this exercise is to give you 'big picture' experiences not coding challenges."
- "We're preparing for Run 3 and HL-LHC, and exploring models that are motivating and not excluded by Run 2 observations."
- "Eliminate fear-of-losing-data by using version control! Commit often, and document, document, document. Save yourself time later by setting CI/CD tests."
---
We can begin by making sure all of our base code works, including our logins, environment, proxy, etc.
*Include instructions on how to make sure environment is good to go*

Once our code is ready to be worked on, let's take a few minutes to think about working on software projects as a colloboration. The code you run today (and tomorrow) has been developed by *thousands* of folks like you trying to make everyone's life slightly easier. You will probably spend a lot of time reading other people's code and will quickly build a standard on what 'good code' looks like. Similarly when working as a group on a common piece of software, we find good and bad ways to make changes. 

To combat these problems, we agree to have a set of 'good-practices' for keeping software clean and easy to interpret. First, is the use of comments. When developing a script, begin by stating the code's intended purpose and a general roadmap to the file. For each function you define (and also important data structures), include in-line comments explaining clearly what the functions/objects are used for. 

Version control is a way of tracing the history of software projects by recording changes encoded in 'commits/pushes'. We will be using CERN's GitLab to develop our code. You may notice some of the code we will use is on GitHub instead, and there are reason for that but for our purposes we like the CI/CD interface of GitLab better. CI/CD (Continuous Integrationa and Deployment) is a technique for developing tests on changes (pushes) to the repository. One can, for example, create a test that compiles the code and returns and error if it cannot; one can also create tests for enforcing code style, or execute sample runs of analysis code. The sky is the limit for how fancy you want to make your CI/CD, what do you think should be implemented for our code?
{% include links.md %}

