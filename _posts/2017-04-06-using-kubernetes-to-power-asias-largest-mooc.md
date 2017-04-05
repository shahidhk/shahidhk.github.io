---
layout:     post
title:      Using Kubernetes to power Asia’s largest MOOC
date:       2017-04-06 00:26:00
summary:    Last year, we ran a MOOC on application development called IMAD (Introduction to Modern Application Development) which saw over 83,000 registrations.
registrations.
categories: blog
thumbnail: cogs

---

# Using Kubernetes to power Asia’s largest MOOC

Last year, we ran a MOOC on application development called [IMAD (Introduction
to Modern Application Development)](http://imad.tech/) which saw over **83,000**
registrations.

#### **The application design**

To begin with, we were sure that asking students to use their machines for
programming would not be a good idea. The audience for an online course would
have huge variations in terms of hardware, operating systems etc. Providing
instructions or guidance for multiple platforms throughout the course would have
been an operational nightmare.

> But, everybody has a web browser, and we decided to make use of it.

As our main focus was on developing web applications, an immediate requirement
of a platform for students to publish their applications also became apparent.

At Hasura, we had been using Kubernetes for deploying production workloads for
some time then. Since, we would have needed to provide isolated runtimes for all
our students to build and deploy their applications, it became clear to us from
the very beginning that Kubernetes would be the way to go.

Our plan was to create a web application which lets users write code using a
browser and deploy it in real-time. After comparing a few different models, we
decided to create a Kubernetes deployment and service for each user, hosting
their code on GitHub and running it inside a nodeJS container. Each user would
be given a subdomain and all requests were to be reverse proxied to the
corresponding Kubernetes service using nginx.

#### The application workflow

<span class="figcaption_hack">Short demo of the online coding platform at
[https://cloud.imad.hasura.io](https://cloud.imad.hasura.io/)</span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/5bF6wQr_9_Y?ecver=1" frameborder="0" allowfullscreen></iframe>

A user logs in using GitHub and, using [GitHub API
v3](https://developer.github.com/v3/), we fork a template repository into the
user’s GitHub account. Files from this repository are then fetched for the user
to edit using [Ace Editor](https://ace.c9.io/). All edits made to the files are
committed to the user’s repository. We eliminated the need to use our own
storage backend for users’ code as well as gave all students, mostly first time
developers, a taste of GitHub by doing this. When a user triggers a deployment,
our API server creates a
[deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
and a
[service](https://kubernetes.io/docs/concepts/services-networking/service/) on a
dedicated Kubernetes cluster. The deployment has a [gitRepo
volume](https://kubernetes.io/docs/concepts/storage/volumes/#gitrepo)* *with the
user’s repository, thereby pulling the user’s latest code on to a pre-built
nodeJS docker image. An nginx
[daemonset](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/),
running on all nodes, directs incoming HTTP traffic to the user’s containers
based on the subdomain, thereby serving their web-app.

<span class="figcaption_hack">Overall architecture of the system</span>

The Kubernetes cluster was initially hosted on Microsoft Azure and then migrated
to Google Container Engine (*effortlessly*).

#### The Result

Over the duration of the course, we saw participation from [a diverse set of
students](https://medium.com/@IMAD_mooc/latest) — college students, working
professionals, stay-at-home mothers and educators, often with little or no
background in computer science or application development experience. A mind
boggling ~10000 apps were built!

The students also had a lot of good things to say about their first experience
with developing applications (*you can read more about it
*[here](https://medium.com/@HasuraHQ/tales-from-imad-indias-largest-mooc-de7dad2f6127)):

None of this would have been easy without Kubernetes (*and Hasura, of course*).

*****

You can check out the IMAD console in action here:
[https://cloud.imad.hasura.io](https://cloud.imad.hasura.io/)

Hasura is a PostgreSQL BaaS + Kubernetes PaaS. *With dollops of sass*. Check it
out here:
[https://hasura.io](https://hasura.io/?utm_source=blog&utm_medium=footer&utm_content=imad_k8s)

* [Kubernetes](https://blog.hasura.io/tagged/kubernetes?source=post)
* [Docker](https://blog.hasura.io/tagged/docker?source=post)
* [Mooc](https://blog.hasura.io/tagged/mooc?source=post)

### [Shahidh K Muhammed](https://blog.hasura.io/@shahidh)

Engineer at Hasura, Hobbyist at The WebOps Club

### [Hasura](https://blog.hasura.io/?source=footer_card)

In our humble opinions

This post first appead on [Hasura Blog: Using Kubernetes to power Asia’s largest MOOC](https://blog.hasura.io/using-kubernetes-to-power-asias-largest-mooc-2ea3bfbf1d15)
