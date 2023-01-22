### Data Engineering Zoomcamp Notes
This repository contains my notes for the DataTalksClub Data Engineering Zoomcamp 2023 by Alexey Grigorev.

# Week 1: Introduction & Pre-requisities

## Docker

### What is Docker?

<ul>
<li>An open-source platform (PaaS) that packages softwares enabling them to run and reproduce in any environment reliably and consistently which can simplify development and deployment.</li>
<br>
  
<li>There are three fundamental elements in the Docker Universe:</li>
<ul>
<li>Image - An image is a pre-built environment that contains all of the dependencies required to run a specific application. It is a lightweight, portable, and self-  sufficient package that can be easily distributed and run on different environments.</li>
<li>Registry - A registry is a place to store and distribute Docker images. It can be used to share images publicly or privately, and to manage access control for different users and teams.</li>
<li>Container - A container is a running instance of a Docker image. Containers are lightweight and portable, and they run natively on Linux. They provide a consistent and reproducible environment for applications to run in, for simplifying development and deployment. Containers are isolated from the host system and from each other, so they can run securely and efficiently on a single host or across a cluster of hosts.</li>
</ul>
<br>
  
<li>Each of the three fundamental elements has a role in what Docker calls as the Build Ship Run Life Cycle:</li>
<ul>
  <li>Build - Build refers to the process of creating a container <b>image</b>, which is a lightweight, portable, and self-sufficient environment that includes all of the dependencies required to run the application. Developers can use a Dockerfile, a simple script containing instructions on how to build the image, to automate this process.</li>
<li>Ship - Ship refers to the process of distributing the container image to different environments, such as a development environment, a testing environment, or a production environment. Container images can be pushed to a <b>registry</b>, such as Docker Hub, for easy distribution and sharing.</li>
<li>Run - Run refers to the process of starting one or more instances of a <b>container</b> image and running the application inside of it. Containers are lightweight and can be easily started, stopped, and deleted, which makes it easy to manage and scale applications.</li>
</ul>
</ul>
<br>
### Why should a DE care about Docker?
<li>Allows reproducibility</li>
<li>Runs on any app, any OS, anywhere</li>
<li>Local experiments</li>
<li>Integration test (CI/CD)</li>
<li>Running pipelines on the cloud (AWS Batch, Kubernetes jobs)</li>
<li>Spark</li>
<li>Serverless (AWS Lambda, Google Functions)</li>
