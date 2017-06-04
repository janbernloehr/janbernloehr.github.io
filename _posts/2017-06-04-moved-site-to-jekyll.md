---
layout: post
title : "Moved website and blog to github pages"
date : 04.06.2017 23:39:00
tags: [github, jekyll]
---
{% include JB/setup %}

Today, I moved my website and my blog over to [github pages](https://pages.github.com/). Previously, my blog has been hosted at vb-magazin.de, a community for visual basic developers which due to the tremendous success of [stackoverflow](https://stackoverflow.com/) and the like became redundant and eventually abandoned. The site vb-magazin (and with it my blog) was shutdown in December 2016 and even tough I had ample time to prepare for the shut down, I did not take time to move until now.

Merging my site and my blog has been on my todo list even long before the shut down notification. My blog was running on [telligent's community server 2007](https://en.wikipedia.org/wiki/Telligent_Community) - back then bleeding edge but today unnecessarily complex and also quite outdated. My site, on the other hand, has been a very simple [Azure Web App](https://docs.microsoft.com/en-us/azure/app-service-web/) based on [asp.net core](https://docs.microsoft.com/en-us/aspnet/core/). It read [markdown](https://guides.github.com/features/mastering-markdown/) files from an Azure Storage Account, transformed them into html, and served them to the users. Markdown takes away all the html ugliness of the web and lets me focus on the content. Updating my site was a bliss. Of course, my site does not change all that often and these transformations add some overhead which could be avoided - but conveniently updating the site was king for me. Moreover, asp.net + azure made the site _very_ fast so I did not bother.

Even more, I appreciate markdown so much that I moved on to writing my blog posts in markdown. Since community server did only understand bare html, I then transformed the blog post into html before publishing and just pasted that into the community server web interface. You can blame this overhead to my workflow for me blogging very rarely ;).

Now with github page's [jekyll](https://jekyllrb.com/) I can solve both overhead issues at once. Github pages uses jekyll to generate _static_ webpages from the markup files in a [specially prepared](https://pages.github.com/#user-site) github repository. Serving these static sites is very efficient for the server because it just sends the file it has on disk to the browser without making any transformations or other dynamical changes. Also, such data can be caced very well. Still, thanks to the static site generator jekyll, I can write my blog posts and website pages in Markdown. When I commit new versions of the markdown files to my repository, github fires up jekyll to transform these files into static bare html websites. This has to be done only once when the page is updated instead of each and every time the page is requested from the server. Plus, I get a complete version control for my whole website out of the box.
One should also add that the term _static_ doesn't quite cut it anymore, since it only applies to the server but not the client side. In times of javascript abundance, one can do all kinds of dynamic things even with static pages.

For my site + blog I am using the [jekyll bootstrap](http://jekyllbootstrap.com/) template which is based on the popular [twitter bootstrap](https://getbootstrap.com/). This allowed me to copy over the whole layout I previously set up for my website - the latter was also based on bootstrap. Further, [MathJax](https://www.mathjax.org/), which allows to display mathematical formulas, is supported by jekyll [out of the box](https://jekyllrb.com/docs/extras/).

Moving the distinct pages of my website over to jekyll was tantamount to adding two lines of metadata to each of the markdown files: `title` and `group`. It took me less then five minutes.

Moving my blog posts over required a little more effort. Fortunately, I had a backup of the community server's sql server database containing the blog posts. I used LinqPad to access the posts in an object oriented manner, transformed the stored html to markdown using this [Markdown2Html](https://github.com/baynezy/Html2Markdown) library, and fine tuned the result using a number of regex replace calls to take care of syntax highlighting and html tags left over by Markdown2Html. Transforming ~ 100 blog posts took me about 2-3 hours.

I also merged the following PRs fixing a few bugs in jekyll bootstrap
- https://github.com/plusjade/jekyll-bootstrap/pull/319
- https://github.com/plusjade/jekyll-bootstrap/pull/303
- https://github.com/plusjade/jekyll-bootstrap/pull/299

To close, I am quite satisfied how little time I had to invest to achieve this outcome. So if you are still not convinced to move your site / blog over to github pages, throw in free hosting from github and free https protection adding [significantly to the privacy of your visitors](https://www.howtogeek.com/181767/htg-explains-what-is-https-and-why-should-i-care/). 

Further reading
- [Why Static Site Generators Are The Next Big Thing](https://www.smashingmagazine.com/2015/11/modern-static-website-generators-next-big-thing/)
- [A collection of awesome Jekyll goodies](https://github.com/planetjekyll/awesome-jekyll)
