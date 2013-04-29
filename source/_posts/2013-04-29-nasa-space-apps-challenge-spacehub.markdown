---
layout: post
title: "NASA Space Apps Challenge: SpaceHub"
date: 2013-04-29 10:38
comments: true
categories: hackathon, nasa, space, spacehub, python
---
2 weeks ago I competed in the NASA Space Apps Challenge. Space Apps was a hackathon sponsered by NASA, which also provided a series of challenges that teams could work on.
I teamed up with [Ryan Brown](https://github.com/ryansb), [Sam Lucidi](https://github.com/mansam), and [Greg Jurman](https://github.com/gregjurman). We decided to tackle 
the challenge of creating a system to mirror all of NASA's open source projects on github. This was challenging for a couple of reasons. The first being the fact that 
NASA does not have a single location for all it's code. NASA hosts code with github, sourceforge, and their own sites. Another challenge is that NASA doesn't have a single
revision control system for all of its projects. It uses git, svn, and even just tarballs. We decided that we should write a webapp that lets  a user submit information for 
each project, and then the application will download the source and push it to github. This webapp is [spacehub](https://github.com/ryansb/spacehub). A demo of spacehub is
available [here](http://spacehub-factoids.rhcloud.com/app/index.html). For this project we decided to use pyramid with cornice on Openshift. We chose openshift to reduce the time needed 
to setup hosting. Cornice is a neat layer on top of pyramid that makes it really easy to create restful APIs. On the front end we used bootstrap and emeber. Currently 
spacehub only supports importing tarballs. It was decided that we should focus on supporting the most difficult to import format first, and then work on the easier ones 
like svn and git. Over all it was an excellent hackathon Spacehub came in first place in the software category at the Rochester event. Since we won in Rochester we will
now move on to be judged at the national level.
