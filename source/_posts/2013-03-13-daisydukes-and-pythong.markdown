---
layout: post
title: "DaisyDukes-and-Pythong"
date: 2013-03-14 20:39
comments: true
categories: [python, pythong, bootstrapping, daisydukes, packaging]
---

A little while ago my friend [David (oddshocks)](http://oddshocks.com/) created
a tool called pythong, which is a tool for creating a good default project layout
for python projects. After this had been under development for a little while I was
chatting with another friend [Ryan (ryansb)](http://ryansb.com/) and we decided
that to accompany pythong, there should be a tool to package and move around pythong
projects. This tool is [DaisyDukes](https://github.com/rossdylan/daisydukes).
The current feature set of daisydukes includes

- Creating archives of a project
    - Supports tar.(gz,bz2,lzma,xz) and zip
- Uploading your project somewhere
    - Currently supports pypi
    - Currently working on S3 and FTP uploads
- Packaging your project as a RPM or DEB
    - This feature is still in the planning stage.

Right now the develop branch of daisydukes supports all the archive functions, as well
as uploading to pypi. Work on the other upload methods is underway.
Now for some usage examples
    $ daisydukes archive --zip|gzip|bzip|lzma|xz --extrafiles files...

The archive command supports most common formats, and has a flag to allow for the addition of
extra files to the archive.
    $ daisydukes upload --pypi [--register]

The --register flag registers the project on pypi before uploading a sdist. Daisydukes is definitely
alpha software, there are bound to be rough edges. You can get the latest version on github, and
a relatively stable version on pypi.
