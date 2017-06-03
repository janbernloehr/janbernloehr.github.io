---
layout: post
title : "Plot3D with RegionFunction equivalent in MATLAB"
date : 19.01.2016 17:07:00
tags: []
---
{% include JB/setup %}

Suppose you want to plot a function *g* depending on two variables *x*, *y* over a range where y depends on *x*.

Using Mathematica this can be done in essentially one line (not counting the definition of the function *g* and the lower and upper bounds *f1* and *f2*).

<div class="line"><span class="text plain"><span class="meta paragraph text"><span>f1[y_] := y^3 - 1</span></span></span></div><div class="line"><span class="text plain"><span class="meta paragraph text"><span>f2[y_] := y^3 + 1</span></span></span></div><div class="line"><span class="text plain"><span class="meta paragraph text"><span>g[x_, y_] := x^2 + y^2</span></span></span></div><div class="line"><span class="text plain"><span> </span></span></div><div class="line"><span class="text plain"><span class="meta paragraph text"><span>Plot3D[g[x, y], {x, -2, 2}, {y, -2, 2},</span></span></span></div><div class="line"><span class="text plain"><span> </span><span class="meta paragraph text"><span>RegionFunction -> Function[{x, y, z}, f1[x] < y < f2[x]]]</span></span></span></div>

The output will be like this.

![Mathematica output](http://janmolnar.blob.core.windows.net/public/blog/2016-01-19-plot3d-mathematica.png "Mathematica output")

I am not aware of an equivalent function built directly into MATLAB. However, the same functionality can be achieved with some manual coding.

The essential idea is to create a surface in 3D space and plot this using MATLAB's [*surf*](http://ch.mathworks.com/help/matlab/ref/surf.html) function. I put the code in a gist, which you can find/fork/comment [here](https://gist.github.com/janmolnar/63ae0ca1f3e1969a0757).

The output looks like this.

![MATLAB output](http://janmolnar.blob.core.windows.net/public/blog/2016-01-19-plot3d-matlab.png "MATLAB output")
