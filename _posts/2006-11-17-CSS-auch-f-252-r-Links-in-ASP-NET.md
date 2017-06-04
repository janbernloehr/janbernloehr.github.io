---
layout: post
title : "CSS auch f&#252;r Links in ASP .NET"
date : 17.11.2006 19:23:32
tags: [css, asp.net]
---
{% include JB/setup %}

Definiert man eine Klasse in CSS und referenziert diese von einem Link, wird man vielleicht überrascht sein:

CSS
``` css
 .my-link {
 
 color: #ffc700;
 
 }
```

ASP .NET

``` html
<asp:Hyperlink runat="server" id="lnk1" cssclass="my-link" text="Link 1" />
```

Der Link ist nicht wie erwartet gelb, sondern hat die Browser Standard Farbe für Links. Das liegt daran, dass Links mit eigenen Pseudo-Klassen definiert werden (:link, :active, :hover und :visited).

CSS

``` css
 .my-link:link, .my-link:active, .my-link:hover, .my-link:visited {
 
 color: #ffc700;
 
 text-decoration: none;
 
 }
 
 .my-link:hover{
 
 text-decoration: underline;
 
 }
 
 .my-link:visited{
 
 color: gray;
 
 }
```

Nun erscheint der Link wie erwartet.  
Interessant ist auch, dass gleich mehrere Klassen durch ein Komma getrennt definiert werden können. Wird eine Eigenschaft später im CSS Dokument erneut gesetzt, wird deren Wert einfach überschrieben.

Das ganze kann auch hier betrachtet werden: [http://www.dev-jc-vb.de/dev-jc-vb/Samples/VbMagazin/cssdemo_link_pseudo.htm](http://www.dev-jc-vb.de/dev-jc-vb/Samples/VbMagazin/cssdemo_link_pseudo.htm "http://www.dev-jc-vb.de/dev-jc-vb/Samples/VbMagazin/cssdemo_link_pseudo.htm")
