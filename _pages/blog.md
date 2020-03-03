---
layout: archive
title: "Blog"
permalink: /blog/
author_profile: true
redirect_from:
  - /blog
---

{% include base_path %}

{% if post.pdf %}
  <a href=" {{ post.pdf }} "> Latest CV <img src="/images/pdf_logo.jpeg" width="30" height="45" alt="pdf"></a></p>
{% endif %}

Education
======
* Ph.D in Computer Science, University of Utah, 2020 (expected)
* M.S. in Computer Science, Texas State University, 2014
* B.S. in Computer Engineering (software), Shiraz University, 2012

Experties
======
* HPC Correctness
* Programming Languages
* Floating Point Analysis
* Data Analysis and Visualization
* Parallel/Distributed Programming Languages: MPI, OpenMP, Pthreads, Intel TBB, Intel PIN, CUDA, GO
* HPC-Related: Spark, Hadoop, SLURM, Lustre, NFS
* Programming/Scripting: Bash, Python, Java, C, C++, MATLAB, MySQL, SQL Server, R

Publications
======
  <ul>{% for post in site.publications %}
    {% include archive-single-cv.html %}
  {% endfor %}</ul>


Work experience
======
* Research Assistant - Formal Verification Lab at University of Utah, Spring 2015 - Current
* Research Assistant - Efficient Computing Labratory at Texas State University, Fall 2013 - Fall 2014
* Graduate Assistant - Texas State University, Fall 2012 - Summer 2013
* Web Developer - Zarrin System Fars, 2012
