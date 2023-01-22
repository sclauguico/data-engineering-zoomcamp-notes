### Data Engineering Zoomcamp Notes
This repository contains my notes for the DataTalksClub Data Engineering Zoomcamp 2023 by Alexey Grigorev.

#### Notes Using the Cornell Note-Taking System with Notion 
<a href="https://www.notion.so/DataTalksClub-Data-Engineering-Zoomcamp-2023-b42377b6ca354579a3f11ec1cb0a4ef3">Click on this link!</a>

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

### Why should a DE care about Docker?
<li>Allows reproducibility</li>
<li>Runs on any app, any OS, anywhere</li>
<li>Local experiments</li>
<li>Integration test (CI/CD)</li>
<li>Running pipelines on the cloud (AWS Batch, Kubernetes jobs)</li>
<li>Spark</li>
<li>Serverless (AWS Lambda, Google Functions)</li>

### Creating, running, and building a Docker image

**On the terminal (Git Bash):**

To make a directory (mkdir) or create a folder named 01-docker-sql

```docker
mkdir 01-docker-sql
```

To change director (cd) or move into the folder named 01-docker-sql

```docker
cd 01-docker-sql
```

To start editor, it can be VSCode, Sublime, Atom, PyCharm…

```docker
code .
```

**On the editor (VS Code -  Dockerfile):**

Create a file and name it Dockerfile (no extension needed, the Docker logo 

will show up to verify that the file is correct)

**On the terminal (Git Bash):**

To test Docker, run the command

```docker
docker run hello-world
```

       Note: This will go to the Docker Hub where Docker stores all the images

**On the terminal (Git Bash):**

To test running an app, in this case it will download Python

```docker
docker run -it python:3.9
```

       Note:

    

- *-it* means interactive, allows us to type/enter into the terminal
- Python is the image that we want to run on Docker
- If this is displayed: “the input device is not a TTY. If you are using mintty, try

      prefixing the command with 'winpty’” 

      TRY

```docker
winpty docker run -it python:3.9
```

**On the terminal (Git Bash):**

To test a Python code upon running the Python image, enter the command after >>>

```docker
>>>
print("Hello world")
```

To install pandas, exit from >>> first by pushing CTRL + D on Windows or CMD + D on MAC 

Then enter

```docker
$
docker run -it --entrypoint=bash python:3.9
# to install pandas
pip install pandas
# to enable Python
python
```

Just in case input device is not a TTY. ALWAYS PREFIX winpty before docker 

if the input device is not a TTY

```docker
$
winpty docker run -it --entrypoint=bash python:3.9
#
pip install pandas
python
```

**On the terminal (Git Bash):**

To import pandas library

```docker
>>>
# to import pandas
import pandas
# to verify pandas is imported
# get version
pandas.__version__
```

**On the terminal (Git Bash):**

To understand how image works, exit on the terminal and run a new Python image

```docker
$
docker run -it --entrypoint=bash python:3.9
>>>
python
import pandas
```

    Note: We expect a no module found display. When we exit from the first image     created and create another image, importing pandas in that new image will not    find a pandas module because it was only installed in the first image. Need to pip install pandas here again

**On the editor (VS Code - Dockerfile):**

To specify all the instructions, all the things we want to run, 

write in the **Dockerfile to be built as an image**

```python
FROM python:3.9

# TO run this here instead on the CLI
RUN pip install pandas

ENTRYPOINT [ "bash" ]
```

**On the terminal (Git Bash):**

To build an image from Dockerfile

```docker
$
# to build an image in this directory
# and download pandas dependencies
docker build -t test:pandas .
# to run the image
docker run -it test:pandas

#
python

>>>
import pandas
pandas.__version__
```

**On the editor (VS Code - pipeline.py):**

Create a new file called pipeline.py

```python
import sys

import pandas as pd

print("job finished successfully")
```

Back to Docker file, add the copy command and working directory

```python
FROM python:3.9

RUN pip install pandas
# to specify the working directory /app
# the location of the image where the 
# file is copied
WORKDIR /app

# to specify soruce name and destination name
COPY pipeline.py pipeline.py

ENTRYPOINT [ "bash" ]
```

**On the terminal (Git Bash):**

To build and run the image from Dockerfile, exit from previous image

```docker
$
# to build the image
docker build -t test:pandas .
# to verify that the working directory is 
# app (set in Dockerfile)

docker run -it test:pandas
app#

pwd
ls
# to run the app pipeline.py
python pipeline.py
```

        Note: Successfully displayed: “job finished successfully

       To call this a data pipeline, the container has to be self-sufficient

       We can add parameters if we want to run to pull a data on a specific day,

       or apply transformation, and even save

**On the editor (VS Code - pipeline.py):**

To pass the command line arguments in the script

```python
import sys

import pandas as pd

print(sys.argv)
day = sys.argv[1]

print(f'job finished successfully = f{day}')
```

**On the editor (VS Code - Dockerfile):**

```python
FROM python:3.9.1

RUN pip install pandas

WORKDIR /app
COPY pipeline.py pipeline.py

# to set fefault parameters that cannot be overridden 
# while executing Docker Containers
ENTRYPOINT ["python", "pipeline.py"]
```

**On the terminal (Git Bash):**

Exit from previous image

```docker
$
docker build -t test:pandas .
# to run on a specific day
docker run -it test:pandas 2023-01-19
```

Note: This is called as parameterizing data pipeline scripts
