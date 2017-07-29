---
title: On the Harmonic Oscillator Model for Aromaticity (HOMA)
author: Wallace D. Derricotte
layout: post
permalink: /harmonic-oscillator-model-for-aromaticity/
categories:
  - coding
tags:
  - problem solving
---

<html>
<head>
<script type="text/javascript" src="http://latex.codecogs.com/latexit.js"></script>
</head>
<body>

Aromaticity is a concept taught early to chemists in any introductory organic chemistry course. From the moment we learn about the Huckel 4n+2 rule, this concept is engrained in our brain. This simple Huckel rule is sufficient for monocyclic aromatic compounds, however doesnt formally expand to polycyclic aromatic hydrocarbons (PAHs). Defining aromaticity in PAHs has become an extremely controversial topic in physical organic chemistry with a pleathora of methods being applied to the issue, creating an alphabet soup of methods to choose from (HOMA, HOMED, FLU, NICS, I, etc). The relative usefulness of each of these methods alone is also a great point of debate in the current literature. Most researchers often employ multiple different metrics in order to spot consistent trends to support their conclusions. Many of these metrics are based on molecular geometry and are extremely simple to implement. <br> <br>

One of the more basic metrics based on molecular geometry is called the harmonic oscillator model for aromaticity (HOMA). This simple theory is based on the assumption that the harmonic oscillator energy of extension or compression of a bond depends on the bond lengths. It employs an emprical parameter called <span lang="latex">R_{\rm opt}</span>. If we consider a bond between atoms <span lang="latex">n_1</span> and <span lang="latex">n_2</span>, within the harmonic oscillator model (talk to your local PCHEM Prof) <span lang="latex">R_{\rm opt}</span> can be understood as the distance at which equal energy is required to extend a bond to the length of a single bond or compress it to the length of a double bond. <br><br>

<center><div lang="latex">
R_{\rm opt} = (R_{n_1-n_2} + 2R_{n_1=n_2})/3
</div></center> <br> 

The values to obtain <span lang="latex">R_{\rm opt}</span> are typically taken from X-ray crystal structures of model systems. For example, for a CC bond the single and double bond lengths in ethane and ethene were taken as reference values. Once reference values are obtained for a particular bond, the HOMA formula can be expressed as:

<center><div lang="latex">
{\rm HOMA} = 1 - \frac{1}{n}\sum^{n}_{i}\alpha(R_{\rm opt} - R_i)^2
</div></center>

where <span lang="latex">\alpha</span> is an empirical normalization constant such that HOMA=0 for nonaromatic systems and HOMA=1 for a system where all bonds are equal to <span lang="latex">R_{\rm opt}</span>. Calculating this by hand or implementing this function into any mathematics software is trivial, however a general implementation is a bit more involved. I have cooked up a quick general implementation of this function that only needs the xyz coordinates of your molecule. I utilized the general quantum chemistry file parser CCLIB in order to to easily import and parse the coordinates and store structural information. In python, implementing this function is as simple as defining the following:<br><br>

<!-- HTML generated using hilite.me --><div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;"><pre style="margin: 0; line-height: 125%"><span style="color: #008800; font-weight: bold">def</span> <span style="color: #0066BB; font-weight: bold">homa</span>(n,homa_params):
        sum <span style="color: #333333">=</span> <span style="color: #6600EE; font-weight: bold">0.0</span>
        <span style="color: #008800; font-weight: bold">for</span> i <span style="color: #000000; font-weight: bold">in</span> <span style="color: #007020">range</span>(n):
                sum <span style="color: #333333">+=</span> homa_params[i][<span style="color: #0000DD; font-weight: bold">0</span>]<span style="color: #333333">*</span>(homa_params[i][<span style="color: #0000DD; font-weight: bold">1</span>] <span style="color: #333333">-</span> homa_params[i][<span style="color: #0000DD; font-weight: bold">2</span>])<span style="color: #333333">**</span><span style="color: #6600EE; font-weight: bold">2.0</span>
        <span style="color: #008800; font-weight: bold">return</span>(<span style="color: #6600EE; font-weight: bold">1.0</span> <span style="color: #333333">-</span> (<span style="color: #6600EE; font-weight: bold">1.0</span><span style="color: #333333">/</span>n)<span style="color: #333333">*</span>sum)
</pre></div> <br> <br>

In this example the function <code>homa(n,homa_params)</code> takes two arguments, the first one is an integer that represents the number of bonds in the molecule the other is a tuple that contains the necessary values for <span lang="latex">R_{\rm opt}</span> and <span lang="latex">\alpha</span>. For that specific bond. By utilizing the free library CCLIB, I can immediately parse any xyz coordinate file and have access to the bond lengths necessary to do the HOMA Analysis. I have also implemented code necessary to specify fragments in the molecule. By using CCLIB I can parse outputs from multiple programs and have access to AO overlaps and density matrices necessary to implement more involved aromaticity indices. Here is an example of an output from my program on benzene:<br><br>

<img src="{{ site.baseurl }}/images/benzene_example.png" style="width:800.01px;height:325.36px;"> <br><br>

As expected, HOMA is close to one since benzene is pretty aromatic. This code is useful and will serve as a platform for further development.