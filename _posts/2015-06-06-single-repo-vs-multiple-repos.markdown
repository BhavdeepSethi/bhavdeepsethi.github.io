---
layout: single
title: "Monolithic Repo vs Micro Repos"
date: 2015-06-06 22:21:51 -0700
date_formatted: June 06, 2015
comments: true
tags:
  - engineering
  - git
---

Recently, I have been working with a single repo which had the entire codebase of the company. It was basically a hierarchial maven project with each service being a sub-project/module. Prior to this, I've always worked in an environment where each service had it's own repository. I guess there are pros and cons of each method.

Some of the points in favor of the single codebase approach I’ve heard are:

<!--more -->

- You don’t have to worry about versioning. You end up having a single version for your product. This helps communications with QA, clients and managers. Since everyone has to pull in the same changes, everyone has access to the same thrift sources as well.
- Increased collaboration. Since, you have access to the entire codebase, you can look/learn/contribute to the code written by other teams as well.
- Reduces code duplication. Code discovery is much easier and reduces the amount of code duplicated across teams.

However, there are problems with this approach as well:

- Every time someone modifies code in a separate module, you have to recompile/generate sources to fix the compilation errors. Not to mention compiling takes a lot of time as well.
- I've seen Intellij giving weird path errors even though everything is fine. A synchronize with re-importing the pom fixes this but, it's still annoying.
- Unnecessary dependency. I've seen people continue using a particular framework because it's available and the entire code base uses it. A lot of times people tend to force their solutions to the existing framework. With Micro repos, each service can use the framework best suited for the job.

I personally prefer working with separate repos. It's a lot cleaner and for me, it symbolizes true service oriented architecture.Considering most of the time I'll be developing on my local machine, I need that environment to be super fast. Having a good build tool probably makes the distinction between single vs micro repos even more obscure.

Yet, the monolithic repo seems to be the pre-dominant approach. I believe Google and Facebook do this. But the build environments in these companies are quite matured as well. So, what about the startups out there? Is there a preference?
