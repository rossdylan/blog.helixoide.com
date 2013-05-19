---
layout: post
title: "Heat: Pure python interface for linux temperature sensors"
date: 2013-05-19 11:30
comments: true
categories: python, linux, temperature, heat, sensors
---

I am a sysadmin for [Computer Science House](http://csh.rit.edu), recently we
have been having some issues with our AC units. Since summer is here, and we
are running a skeleton crew in Rochester I decided to try and make a monitoring
system to keep track of how hot our servers are running. This way we can try
and keep ahead of the game and predict what machines will be effected by thermal
emergencies first. The first part of this project is called Heat. Heat is a small
pure python library for grabbing information from a computers temperature sensors
on linux. At this point Heat is really simple. It gives you access to the
temperature in Celsius, Fahrenheit, and the sensors label. One of the neat things
about heat is that it supports both python2 and python3.
Check it out on [github](https://github.com/rossdylan/heat) and [pypi](https://pypi.python.org/pypi/heat/0.1.1)
