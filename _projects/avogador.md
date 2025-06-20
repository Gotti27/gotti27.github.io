---
layout: page
title: Avogador
description: an academic driven code evaluation platform
img: assets/img/avogador/textLogo.svg
importance: 4
category: Software Architectures
related_publications: false
toc:
  beginning: true
  sidebar: left
---

<div class="row">    
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/avogador/textLogo.svg" title="avogador logo" class="img-fluid rounded" %}
    </div>
</div>

Avogador is an academic driven programming evaluation platform, developed together with my fellow colleague [Alessio Campanelli](https://www.unive.it/data/persone/28847327).
The name comes from Avogadori[^1], which can be summarized as judges/lawyers during the Most Serene Republic of Venice[^2].
You guess it right: we are quite into that historical setting[^3].
By the way, here's a figure of the Avogadori:

<div class="row">    
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/avogador/avogadori.jpg" title="avogadori-wikipedia" class="img-fluid rounded" %}
    </div>
</div>

Skipping history digressions, Avogador has been developed by ourselves while enrolled in the Master's Degree program at Ca' Foscari University of Venice[^4], and is currently maintained as a side project.
The goal of the project was to replace and improve all the main features of a popular judge system that contains the word _Hacker_ in its name.

# Features

As stated in the introduction of this article, Avogador is an academic oriented code execution platform and judge system.
This means that it's optimized to run large scale programming trials and project submissions in the context of an academic department.
Being fully-selfhosted, Avogador features its own storage system, providing full and local data ownership, which is essential in the education field.

Each deploy is meant to be University based, and can contain multiple courses, which can contain several trials and projects.
A trial is defined as a collection of exercises.
The development of an _exam mode_ is ongoing, however we still believe that paper & pen is more suitable exams, even programming-based ones.
Such mode grants additional security checks like onfocus page alerts and ip submission tracking.

Shifting to exercises - the core of Avogador's features - we addressed one of the most critical requirement when dealing with the academic sector, i.e. code visibility.
An Avogador's exercise is made of a description (which may contain markdown and LaTeX), a template and a set of testcases.
The template is the initial source code provided to the student, which can be split into section with different visibility rules.
Indeed, a template section can either be _Editable_, _Visible_ or _Not-visible_. While the first one is where the student is supposed to implement their solution, the latter ones are designed to perform operations such as reflection controls.
We introduced our custom file format (.strox) to support such structure.

Regarding plagiarism detection, we rely on a code similarity tool (JPlag[^5]) to highlight suspicious similarities on submissions of different students.
If you aren't a Computer Scientist, this is AI✨.

Speaking of Projects, they can be defined as a multi file task that has to be formed offline by the student and submitted through the platform after the development.
At present, only jupyter notebooks are supported: so a student can upload a directory containing a notebook and the required data.
After the execution it is either possible to check the error message or to download the generated html report.

# Architecture

Avogador's architecture is a microservice oriented architecture. Yes, we "overstimated" it, but it was a cool project to mess around with a couple of concept that we thought were valuable to explore.

<div class="row">    
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/avogador/avogador-arch.png" title="avogador-arch" class="img-fluid rounded" %}
    </div>
</div>

## Tech stack

All the Avgogador's microservices are build using Java Spring Boot, while the frontend webapp is a React App boundled using Vite.
We have in plan to switch to Next.js to allow server side rendering, however this is not yet scheduled.
Regarding submissions execution, we rely on our custom code executor, which handles the sandboxes using docker containerization.

# Screenshots

Here's some fancy screenshots of the application: TODO

# Results

Avogador is one of our largest project and we are proud of what we achieved, expecially in terms of impact and quality.
It has been used by almost 500 users so far within many courses.
However we didn't had neither time nor resources for pushing to commercialization and to push it outside of the boundaries of Ca' Foscari.

Avogador has never experienced any critical failure, nor security breaches, achieving good and stable performances, since our executor is capable of handling multiple jobs in parallel asyncronously and can also be scaled to use remote computing power.

# Join us

We are thrilled that you've read this article to this point.
If you wish to deploy Avogador in your department just [drop me an email](mailto:mario.gottardo@mostserene.eu), altough the code is not yet opensource, we have in our plans to make the repository public.
Despite that, we are open to arrange a deploy and to include new people in the project.
In case you do not want to care about the deploy, we can also think about an on-premise deploy on our infrastructure.

Having said that, the best contribution/sponsor for us would be you to try Avogador in your University.

# Conclusions and future development

We hope you have appreciated this reading so far.
Nowdays we have move on from this project, Alessio is doing a PhD in the same university while I'm currently a computer vision engineer. Despite that we still think of coming back to Avogador's development.

[^1]: [Avogadoria de Comùn - Wikipedia](https://en.wikipedia.org/wiki/Avogadoria_de_Com%C3%B9n)
[^2]: [Republic of Venice - Wikipedia](https://en.wikipedia.org/wiki/Republic_of_Venice)
[^3]: [MostSerene Software](https://mostserene.eu)
[^4]: [Ca' Foscari University of Venice](https://www.unive.it/)
[^5]: [JPlag Antiplagiarism](https://github.com/jplag/JPlag)
