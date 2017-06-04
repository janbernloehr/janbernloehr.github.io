---
layout: post
title : "Delete Team Projects from Hosted TFS Servies"
date : 06.06.2013 08:45:00
tags: [tfs]
---
{% include JB/setup %}

When the TFS Services preview went live, I started moving my on premise projects to the hosted tfs services.

At some point I wanted to delete a team project created in the hosted services. For now, the web interface does not provide an option for that. However, the commandline tool **tfsdeleteproject** works fine.

**Syntax:**

    tfsdeleteproject /force /collection:https://[yourname].visualstudio.com/DefaultCollection [yourproject]

**Warning:** This operation cannot be undone!
