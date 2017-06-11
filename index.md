---
layout: page
title: Home
tagline: Things are only impossible until they're not
---
{% include JB/setup %}

### about me

My name is Jan Bernlöhr and I am a Software Engineer working on autonomous driving at [Bosch](https://www.bosch.com). I received my PhD from the [I-MATH](http://math.uzh.ch) of the [University of Zurich](http://www.uzh.ch) under supervision of [T. Kappeler](https://www.math.uzh.ch/index.php?id=professur&L=&key1=113&key2=&key3=&keySemId=) where I worked on [completely integrable PDEs](academia). Loving and breathing the [.net technology stack](https://www.microsoft.com/net).


### blog

<ul class="posts">
  {% for post in site.posts limit:5 %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

### links
- [Publications](academia/publications)
- [Academia](academia)

### you can find me on
- [GitHub](https://github.com/janbernloehr)
- [arXiv.org](https://arxiv.org/a/molnar_j_1.html)
- [researchgate](https://www.researchgate.net/profile/Jan_Cornelius_Molnar)
- [Twitter](https://twitter.com/janmnet)
