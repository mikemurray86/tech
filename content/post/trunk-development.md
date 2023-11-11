---
author: "Mike Murray"
title: "Trunk based development for GitOps"
description: "Using Trunk based development for GitOps based infrastructure"
date: "2023-11-07"
tags:
    - development
    - gitops
    - DevOps
    - IAC
---
# Using Trunk based development for GitOps based infrastructure

It's commonly said that using environment branches for GitOps deployments is an anti-pattern but if you're working in an org with multiple environments that you must promote changes through then
the common examples out on the web probably leave you wondering how exactly you are supposed to manage your environment. Most guides focus on applying Trunk Based Development to application code and
in the case of application code it's a natural fit. You build your application of main and tag the branch that makes up a "finished" product. The finished product doesn't always mean it's made it to a
production ready state. It could be a "finished" development build that is ready for testing before it's signed off as ready for production. Utilizing this method offers the added benefit of offering
an consistent reference for application builds at every stage of the process. 

Now that you have an idea of why you would would adopt this method let's go over what the traditional example of a repo would look like. The repo will be setup with a single branch in our case we will
use `main` as our branch. Now when you are ready to begin work on a new feature you will create a feature branch off of main to do your work by using `git checkout -b feature/cool-new-thing` that way
you're isolated while you get everything squared away.

Once you have your new feature finished up you will need to create a merge/pull request back into main. Once your request is approved and merged then you would need to create a release based on your
main branch. The release would traditionally create a tag on your main branch representing the state of your repo at that moment in time. This tag would then be used to release a production ready
version of your application. 

This workflow is great if you are working with an application because the single point of contact with the build products aligns well with getting a product out the door. The main branch can act as
your development version of the application and serves as the entrypoint for all the qa testing that needs to be done your are ready for production. The problem starts when your repo is a gitops repo
that needs to support multiple environments. To support multiple environments you will need a gitops tool that supports using tags as a source target. Once your tool supports tags then you will be
able to point your production system at the latest tag on the repo and your qa environments will point at the main branch. Now we have support for two environments in our repo! Now if you have a third
environment to support for your development environments you could instead utilize main for development the most recent tag as your qa environment and the next tag as your latest production release.
This is about the furthest you can stretch this method before it gets hard to keep track of but if you need more than three long lived environments there should probably be additional discussions with
the business to learn why that is and what measures can be taken to reduce operating costs.
