## data-engineering-zoomcamp-notes
This repository contains my notes for the DataTalksClub 2023 Data Engineering Zoomcamp by Alexey Grigorev.

# Week 1: Introduction & Pre-requisities

## Docker

### What is Docker?

<ul>
<li>An open-source platform (PaaS) that packages softwares enabling them to run and reproduce in any environment reliably and consistently which can simplify development and deployment.</li>
<li>There are three fundamental elements in the Docker Universe:</li>
  <ul>
  <li>Image - An image is a pre-built environment that contains all of the dependencies required to run a specific application. It is a lightweight, portable, and self-sufficient package that can be easily distributed and run on different environments.</li>
<li>Registry - A registry is a place to store and distribute Docker images. It can be used to share images publicly or privately, and to manage access control for different users and teams.</li>
<li>Container - A container is a running instance of a Docker image. Containers are lightweight and portable, and they run natively on Linux. They provide a consistent and reproducible environment for applications to run in, for simplifying development and deployment. Containers are isolated from the host system and from each other, so they can run securely and efficiently on a single host or across a cluster of hosts.</li>
</ul>
<li>Each of the three fundamental elements has a role in what Docker calls as the Build Ship Run Life Cycle:</li>
</ul>
