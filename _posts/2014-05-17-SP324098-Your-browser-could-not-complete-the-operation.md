---
layout: post
title : "SP324098: Your browser could not complete the operation"
date : 17.05.2014 09:12:00
tags: [vpn, visual-studio]
---
{% include JB/setup %}

After having changed the password of my Visual Studio Online account, Visual Studio greeted me by asking to reenter my credentials. So I clicked on the Sign In button but instead of being redirected to the Sign In page I simply got the error message **"SP324098: Your browser could not complete the operation"**.

 ![Visual Studio Error Dialog](http://janmolnar.blob.core.windows.net/public/2014-05-17-Sorry.PNG) 

 I tried several things like setting IE to be my default browser, deleting the browser history, and resetting IE to factory settings. I also installed the most recent version of Newtonsofts Json library to my GAC without having success on the Sing In page. 

 Finally, I found the answer on [Stackoverflow](http://stackoverflow.com/questions/20272942/vs-2012-tfs-stuck-on-loading-identity-providers).  
 My computer is joined to a domain network but I tried to do the sign in from home without a VPN connection established. The Visual Studio Identity provider thus could not reach my domain controller and gave the cryptic error message instead of the Sign In page. 

 **The solution** was quite simple: Establish a VPN connection such that Visual Studio can reach the domain controller and everything is fine again.

 ![Visual Studio Login Dialog](http://janmolnar.blob.core.windows.net/public/2014-05-17-Success.PNG) 
