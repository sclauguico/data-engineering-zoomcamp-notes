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

To start editor, it can be VSCode, Sublime, Atom, PyCharm???

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
- If this is displayed: ???the input device is not a TTY. If you are using mintty, try

      prefixing the command with 'winpty?????? 

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

        Note: Successfully displayed: ???job finished successfully

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

## Docker Compose and Running Postgres with Docker
### How to run Postgres on Docker?

To run Postgres, use the official Docker image of Postgres

```yaml
services:
  pgdatabase:
    image: postgres:13
    environment:
      - POSTGRES_USER=root
      - POSTGRES_PASSWORD=root
      - POSTGRES_DB=ny_taxi
    volumes:
      - "./ny_taxi_postgres_data:/var/lib/postgresql/data:rw"
    ports:
      - "5432:5432"
  pgadmin:
    image: dpage/pgadmin4
    environment:
      - PGADMIN_DEFAULT_EMAIL=admin@admin.com
      - PGADMIN_DEFAULT_PASSWORD=root
    ports:
      - "8080:80"
```
#### What is a Docker compose?

A way of running multiple Docker images

**On the terminal (Git Bash):**

To open editor, enter

```docker
code .
```

**On the editor (VS Code):**

To paste the Docker image, create a new file on the editor and paster the image above.

To create the command to run Postgres, configure the following

```docker
winpty docker run -it \
  -e POSTGRES_USER="root" \
  -e POSTGRES_PASSWORD="root" \
  -e POSTGRES_DB="ny_taxi" \
  -v $(pwd)/ny_taxi_postgres_data:/var/lib/postgresql/data \
  -p 5432:5432 \
  postgres:13
```

To create a local file system to be mapped to a container, create a folder on the directory named ny_taxi_postgres_data

![image](https://user-images.githubusercontent.com/67311751/217256754-6696c788-0123-4bce-ae1b-13076929face.png)

Note: 

**On the terminal (Git Bash):**

To run Postgres on Docker, pase the following code on the terminal

```docker
winpty docker run -it \
  -e POSTGRES_USER="root" \
  -e POSTGRES_PASSWORD="root" \
  -e POSTGRES_DB="ny_taxi" \
  -v $(pwd)/ny_taxi_postgres_data:/var/lib/postgresql/data \
  -p 5432:5432 \
  postgres:13
```

**On the editor (VS Code):**

To verify that we have mapped to a folder in the container, the folder must contain the following files.

![image](https://user-images.githubusercontent.com/67311751/217257148-0a607b44-9576-4929-b013-3fa05b442191.png)

**On a new terminal (Git Bash):**

**On the terminal (Git Bash):**

To access this database, use a cli client. In this case, we are going to use pgcli, which is a Python library.

For the following Python libraries to be used, make sure to do **pip install <name-of-library> to install them.**

To login to the database, enter

```docker
pgcli -h localhost -p 5432 -u root -d ny_taxi
```

winpty pgcli -h localhost -p 5432 -u root -d ny_taxi

or 

```docker
pgcli -h 127.0.0.1 -p 5432 -u root -d ny_taxi
pgcli -h 0.0.0.0 -p 5432 -u root -d ny_taxi
```

**On the terminal (Git Bash):**

To access the database, enter the password

```docker
root 
```

To view the table, enter

```docker
\dt
```

To try a requesting from the database, enter

```docker
SELECT 1
```

Note:

The database answered with 1 column and 1 row, with a value of 1
  
  ![image](https://user-images.githubusercontent.com/67311751/217257271-5b145004-a298-4f71-858a-95323d51ebb7.png)


### How to upload data on the Postgres ran on Docker?

**On the terminal (Git Bash):**

To install Jupyter, a notebook and an interactive environment for manipulating data, enter

```docker
pip install jupyter
```

**On the terminal (Git Bash):**

To run Jupyter, enter

```docker
jupyter notebook
```

Note: VS Code or PyCharm can be used instead of Jupyter

**On the notebook (Jupyter):**

To create a new notebook, select on New on the upper right and select Python 3 and name the .ipynb file as upload-data

![image](https://user-images.githubusercontent.com/67311751/217257484-94869613-fd61-403a-be53-ed8f4cfad35b.png)
  
To access the data, download the yellow_tripdata_2021-01.csv.gz from the repo to your local directory

[https://github.com/DataTalksClub/nyc-tlc-data/releases/tag/yellow](https://github.com/DataTalksClub/nyc-tlc-data/releases/tag/yellow)

**On the terminal (Git Bash):**

To take  access the first 100 rows or records of the data, and save to a different file called yellow_ head.csv, enter

```docker
head -n 100 yellow_trip_data_2021-01.csv.gz
```

To take a look at the number of rows using wc (word count) and -l (to count lines insteaed of words). enter

```docker
wc -l yellow_trip_data_2021-01.csv.gz
```

To upload the data, manipulate it first in the Jupyter notebook. You may copy the code from the link below. ???

[data-engineering-zoomcamp-notes/upload-data.ipynb at main ?? sclauguico/data-engineering-zoomcamp-notes](https://github.com/sclauguico/data-engineering-zoomcamp-notes/blob/main/week-01/01-docker-sql/upload-data.ipynb)

**On the terminal (Git Bash):**

To continue exploring the dataset uploaded, enter
  ```sql
SELECT MAX (tpep_pickup_datetime), MIN(tpep_pickup_datetime), MAX(total_amount) FROm yellow_taxi_data;
```
  ![image](https://user-images.githubusercontent.com/67311751/217258014-c20410ea-18b8-447f-8535-54329c5234b6.png)

  
### How to connect pgAdmin and Postgres?

o view the dataset using a web-based GUI instead of the CLI, use pgAdmin and connect via Postgres

To run pgAdmin instead of installing it,  Google pgadmin docker, copy the pull command, and pull the Docker image that contains the tool

Image needed to run: docker pull dpage/pgadmin4

**On a new terminal (5th Git Bash):**

**On the terminal (5th Git Bash):**

To run pgAdmin on Docker, enter

```docker
docker run -it \
  -e PGADMIN_DEFAULT_EMAIL="admin@admin.com" \
  -e PGADMIN_DEFAULT_PASSWORD=root \
  -p 8080:80 \
  dpage/pgadmin4
```

or

```docker
winpty docker run -it \
  -e PGADMIN_DEFAULT_EMAIL="admin@admin.com" \
  -e PGADMIN_DEFAULT_PASSWORD=root \
  -p 8080:80 \
  dpage/pgadmin4
```

Note:

- The environmental variables in the command will be used as credentials for loggin in to pgAdmin.
    - 8080 is the port of our host machine mapped to port 80 on the container. pgAdmin is listening for request on port 80 mapped to 8080. And all the request we send to 8080 will be forwarded to 80 on the container

**On the browser (Chrome):**

To access pgAdmin, enter

[http://localhost:8080/](http://localhost:8080/)

It will lead to this page. To login, enter

email address: admin@admin.com

password: root

![image](https://user-images.githubusercontent.com/67311751/217258314-cc692702-60b2-4dc3-897a-8d00feb78f6c.png)
  
To create a new server, right click on Servers > Register > Server???

![image](https://user-images.githubusercontent.com/67311751/217258394-08b7b109-002b-4f24-95f3-363c0f1450c2.png)
  
To specify name, name it Local docker

![image](https://user-images.githubusercontent.com/67311751/217258500-27dcf5b9-6b7a-4279-9036-a5e96367f0d2.png)
  
In connection, as specified in the created engine using Python

host: localhost

username: root

password: root

Click save

![image](https://user-images.githubusercontent.com/67311751/217258591-244178a6-ac69-4b7c-add2-6ee8c96dbbe5.png)
  
???This will display, ???Unable to connect to server??? because we are running the pgAdmin image Docker run command inside a container, and also the localhost.

It will find Postgres on this container, but it won???t be able to find it because it does not include Postgres on this container

![image](https://user-images.githubusercontent.com/67311751/217258698-ab9b4152-d869-40c0-9c98-64399e856ff5.png)
  
![image](https://user-images.githubusercontent.com/67311751/217258759-53383abf-246a-431a-ac64-484cc166dfa1.png)
  
To connect it, link the two: Database/Postgres and pgAdmin

To do this, put them in one network in separate containers and they will be able to see each other.

![image](https://user-images.githubusercontent.com/67311751/217258862-407ad04a-5b0f-48af-9477-87d91399d423.png)
  
**On the terminal (5th Git Bash):**

To proceed, stop the current run (CTRL+C) for both pgAdmin and Postgres

To create a network, use docker network create

```docker
winpty docker network create pg-network
```

Note: We now have a network when we run Postgres. 

To run the container on this network, enter

```docker
winpty docker run -it \
  -e POSTGRES_USER="root" \
  -e POSTGRES_PASSWORD="root" \
  -e POSTGRES_DB="ny_taxi" \
  -v $(pwd)/ny_taxi_postgres_data:/var/lib/postgresql/data \
  -p 5433:5432 \
  --network=pg-network \
  --name pg-database \
  postgres:13
```

Note: 

- network - name of the network
- name - lets pgAdmin discover Postgres

**On the terminal (2nd Git Bash) where pgcli was ran:**

To check if we still have the data, enter

```sql
SELECT COUNT(1) FROM yellow_taxi_data;
```
![image](https://user-images.githubusercontent.com/67311751/217259018-911e88ff-cf0a-45db-96d1-41212472bc23.png)
  
**On the terminal (2nd Git Bash) where pgcli was ran:**

To run pgAdmin on the same network, exit from SQL (CTRL+D), then enter

```docker
winpty docker run -it \
  -e PGADMIN_DEFAULT_EMAIL="admin@admin.com" \
  -e PGADMIN_DEFAULT_PASSWORD=root \
  -p 8080:80 \
  --network=pg-network \
  --name pgadmin \
  dpage/pgadmin4
```

Note: 

- network - name of the network
- name - less important here unlike with Postgres, because this will allow pgAdmin to know how to connect to Postgres, but nobody needs to connect to pgAdmin

If there is a conflict on the container name, just change network name and pg-database and pgadmin name by adding -[any number] 

```yaml
docker network create pg-network-3

winpty docker run -it \
  -e POSTGRES_USER="root" \
  -e POSTGRES_PASSWORD="root" \
  -e POSTGRES_DB="ny_taxi" \
  -v $(pwd)/ny_taxi_postgres_data:/var/lib/postgresql/data \
  -p 5432:5432 \
  --network=pg-network-3 \
  --name pg-database-4 \
  postgres:13

winpty docker run -it \
  -e PGADMIN_DEFAULT_EMAIL="admin@admin.com" \
  -e PGADMIN_DEFAULT_PASSWORD=root \
  -p 8080:80 \
  --network=pg-network \
  --name pgadmin-2 \
  dpage/pgadmin4
```

![image](https://user-images.githubusercontent.com/67311751/217259132-947f41aa-69f9-4799-926e-b750fb399c7c.png)
  
**On the browser (Chrome):**

To access pgAdmin GUI, enter

[http://localhost:8080/](http://localhost:8080/)

and login

To create a server, right click on Servers > Register > Server???

Enter the following values for each input box

***************General***************

Name: Docker localhost

***Connection***

Host name: pg-database (so pgAdmin will know how to find Postgres)

username: root

password: root

Upon connecting, Click on Servers > Docker [localhost](http://localhost) > Databases > ny_taxi > schemas > Tables > yellow_taxi_data

![image](https://user-images.githubusercontent.com/67311751/217259296-b03bd97a-ea1f-4eda-8bf4-db7a8a068ef4.png)
To write an SQL query, Click on Tools > Query Tool
  
```
  SELECT COUNT(1) FROM yellow_taxi_data;
```
![image](https://user-images.githubusercontent.com/67311751/217259395-dd02c677-7315-468f-9c8e-34c4f1ca37f7.png)


### How to put ingestion script into Docker?
#### How to convert Jupyter notebook to a Python script?

**On the terminal (6th Git Bash):**

To convert Jupyter notebook to a Python script, enter

```docker
jupyter nbconvert --to=script upload-data.ipynb
```

**On the editor (VS Code):**

To prepare ingest_data.py

1. Open upload-data.py
2. Rename file to ingest_data.py
3. Clean the script

```
**On the terminal (6th Git Bash):**

```
from time import time

import pandas as pd
from sqlalchemy import create_engine

engine = create_engine('postgresql://root:root@localhost:5432/ny_taxi')

df_iter = pd.read_csv('yellow_tripdata_2021-01.csv.gz', iterator=True, chunksize=100000)

df = next(df_iter)

df.tpep_pickup_datetime = pd.to_datetime(df.tpep_pickup_datetime)
df.tpep_dropoff_datetime = pd.to_datetime(df.tpep_dropoff_datetime)

df.head(n=0).to_sql(name='yellow_taxi_data', con=engine, if_exists='replace')

df.to_sql(name='yellow_taxi_data', con=engine, if_exists='append')

while True: 
    t_start = time()

    df = next(df_iter)

    df.tpep_pickup_datetime = pd.to_datetime(df.tpep_pickup_datetime)
    df.tpep_dropoff_datetime = pd.to_datetime(df.tpep_dropoff_datetime)
    
    df.to_sql(name='yellow_taxi_data', con=engine, if_exists='append')

    t_end = time()

    print('inserted another chunk, took %.3f second' % (t_end - t_start))

```

#### How to parametrize the script with argparse?

**On the editor (VS Code):**

To parse command line arguments, use argparse library

```python
import os

import agparse

from time import time

import pandas as pd
from sqlalchemy import create_engine

def main(params):
    user = params.user
    password = params.password
    host = params.host
    port = params.port
    db = params.db
    table_name = params.table_name
    url = params.url
    csv_name = 'output.csv'

    os.system(f"wget {url} -O {csv_name}")

    engine = create_engine(f'postgresql://{user}:{password}@{host}:{port}/{db}')
    
    df_iter = pd.read_csv(csv_name, iterator=True, chunksize=100000)
    
    df = next(df_iter)
    
    df.tpep_pickup_datetime = pd.to_datetime(df.tpep_pickup_datetime)
    df.tpep_dropoff_datetime = pd.to_datetime(df.tpep_dropoff_datetime)
    
    df.head(n=0).to_sql(name=table_name, con=engine, if_exists='replace')
    
    df.to_sql(name=table_name, con=engine, if_exists='append')
    
    while True: 
        t_start = time()
    
        df = next(df_iter)
    
        df.tpep_pickup_datetime = pd.to_datetime(df.tpep_pickup_datetime)
        df.tpep_dropoff_datetime = pd.to_datetime(df.tpep_dropoff_datetime)
        
        df.to_sql(name=table_name, con=engine, if_exists='append')
    
        t_end = time()
    
        print('inserted another chunk, took %.3f second' % (t_end - t_start))

if __name__ == '__main__':

    parser = argparse.ArgumentParser(description='Ingest CSV data to Postgres')

    parser.add_argument('user', help='user name for postgres')
    parser.add_argument('password', help='password for postgres')
    parser.add_argument('host', help='host for postgres')
    parser.add_argument('port', help='port for postgres')
    parser.add_argument('db', help='database name for postgres')
    parser.add_argument('table-name', help='name of the table where we will write the results to')
    parser.add_argument('url', help='url of the csv file')

    args = parser.parse_args()

    main(args)
```

**On the terminal (6th Git Bash):**

To run the script, enter on 

```python
URL="https://s3.amazonaws.com/nyc-tlc/trip+data/yellow_tripdata_2021-01.csv"
python ingest_data.py \

    --user=root \
    --password=root \
    --host=localhost \
    --port=5432 \
    --db=ny_taxi \
    --table_name=yellow_taxi_trips \
    --url=${URL}
```

??? Note that we've changed the table name from??`yellow_taxi_data`??to??`yellow_taxi_trips`

#### How to Dockerize the ingestion script?

**On the editor (VS Code):**

To write the Dockerfile, 

```python
FROM python:3.9.1

RUN apt-get install wget
RUN pip install pandas sqlalchemy psycopg2

WORKDIR /app
COPY ingest_data.py ingest_data.py

ENTRYPOINT ["python", "ingest_data.py"]
```

**On the terminal (5th Git Bash):**

To Dockerize the ingestion script, build the Docker image

```python
winpty docker build -t taxi_ingest:v001 .
```

Then, run the image

```docker
winpty docker run -it taxi_ingest:v001 \
 --user=root \
 --password=root \
 --host=localhost \
 --port=5432 \
 --db=ny_taxi \
 --table_name=yellow_taxi_trips \
 --url=${URL}
```

To stop a container, first take a look at the existing containers

```docker
docker ps
```

or

```docker
winpty docker ps
```

Select the container to stop

```docker
docker kill [copy paste container id]
```

```docker
winpty docker kill [copy paste container id]
```

To run Docker on the network

```docker
winpty docker run -it \
--network=pg-network \
taxi_ingest:v001 \
 --user=root \
 --password=root \
 --host=localhost \
 --port=5432 \
 --db=ny_taxi \
 --table_name=yellow_taxi_trips \
 --url=${URL}
```

To start an http server,

```docker
python -m http.server
```

Go to browser and enter

localhost:8000

Copy the IPv4 Address by entering

```docker
ipconfig
```

To

```docker
URL = "http://172.24.208.1:8000/yellow_tripdata_2021-01.csv"
winpty docker run -it \
--network=pg-network \
taxi_ingest:v001 \
 --user=root \
 --password=root \
 --host=localhost \
 --port=5432 \
 --db=ny_taxi \
 --table_name=yellow_taxi_trips \
 --url=${URL}
```

### How to run Postgres and pgAdmin with Docker-Compose?

#### What is a Docker Compose?

It is a utility which allows putting configuration from multiple containers in one file and instead of typing a lot of things, we can just use Docker-Compose

#### How to Install Docker Compose?

If you???re using Windows or MAC, it is already available on your device as it is part of Docker Desktop

**On the terminal (Git Bash):**

To verify,  enter the following

#### How to configure Postgres Database and pgAdmin in Docker-Compose file?

**On the editor (VS Code):**

Create a file: docker-compose.yaml

To create the yaml file

```docker
services:
  pgdatabase: 
    image:postgres:13
    environment:
      - POSTGRES_USER=root
      - POSTGRES_PASSWORD=root
      - POSTGRES_DB=ny_taxi
   volumes: 
    - "./ny_taxi_postgres_data:/var/lib/postgresql/data:rw"
   ports:
    - "5432:5432"

   pgadmin: 
     image: dpage/pgadmin4
     environment:
      - PGADMIN_DEFAULT_EMAIL=admin@admin.com
      - PGADMIN_DEFAULT_PASSWORD=root
     ports: 
      "8080:80"
```

**On the terminal (Git Bash):**

To check if nothing is running, enter

```docker
winpty docker ps
```

To run the docker-compose

```docker
winpty docker-compose up
```

To shut down the docker-compose, enter

 

```docker
winpty docker-compose down
```

To run on detach mode, enter

```
**On the editor (VS Code):**

Create a file: docker-compose.yaml

To create the yaml file

```docker
services:
  pgdatabase: 
    image:postgres:13
    environment:
      - POSTGRES_USER=root
      - POSTGRES_PASSWORD=root
      - POSTGRES_DB=ny_taxi
   volumes: 
    - "./ny_taxi_postgres_data:/var/lib/postgresql/data:rw"
   ports:
    - "5432:5432"

   pgadmin: 
     image: dpage/pgadmin4
     environment:
      - PGADMIN_DEFAULT_EMAIL=admin@admin.com
      - PGADMIN_DEFAULT_PASSWORD=root
     ports: 
      "8080:80"
```

**On the terminal (Git Bash):**

To check if nothing is running, enter

```docker
winpty docker ps
```

To run the docker-compose

```docker
winpty docker-compose up
```

To shut down the docker-compose, enter

 

```docker
winpty docker-compose down
```

To run on detach mode, enter

```
winpty docker-compose up -d
```

### How to SQL?

#### How to join yellow taxi table with the zones lookup table?
    
  ```
    SELECT * 
    FROM 
    	yellow_taxi_trips t, 
    	zones zpu,
      zones zdo
    
    WHERE
    	t."PUlocationID" = zpu."LOCATIONID" AND 
      t."DOlocationID" = zdo."LOCATIONID"
    
    LIMIT 100;
 ```
    
 To join implicitly
    
 ```sql
    SELECT 
    	tpep_pickup_datetime,
    	tpep_dropoff_datetime,
    	total_amount,
    	CONCAT(zpu."Borough", ' / ', zpu."Zone") AS "pickup_loc",
    	CONCAT(zdo."Borough", ' / ', zdu."Zone") AS "dropoff_loc"
    
    FROM 
    	yellow_taxi_trips t, 
    	zones zpu,
      zones zdo
    
    WHERE
    	t."PUlocationID" = zpu."LocationID" AND 
      t."DOlocationID" = zdo."LocationID"
    LIMIT 100;
 ```
    
 To use an explicit Inner Join
    
 ```sql
    SELECT 
    	tpep_pickup_datetime,
    	tpep_dropoff_datetime,
    	total_amount,
    	CONCAT(zpu."Borough", ' / ', zpu."Zone") AS "pickup_loc",
    	CONCAT(zdo."Borough", ' / ', zdu."Zone") AS "dropoff_loc"
    
    FROM 
    	yellow_taxi_trips t JOIN zones zpu
    	  ON t."PULocationID" = zpu."LocationID"
    	JOIN zones zdo
    		ON t."DOLocationID" = zdo."LocationID"
      
    LIMIT 100;
 ```
    
    To check for records with Location ID not in the zones table
    
 ```
    SELECT 
    	tpep_pickup_datetime,
    	tpep_dropoff_datetime,
    	total_amount,
    	"PULocationID",
    	"DOLocationID"
    
    FROM 
    	yellow_taxi_trips t 
      
    WHERE
    	"DOLocationID" is NULL
    
    LIMIT 100;
```
To check if there are any IDs that are not in the yellow taxi table

```
  
  SELECT 
	tpep_pickup_datetime,
	tpep_dropoff_datetime,
	total_amount,
	"PULocationID",
	"DOLocationID"

  FROM 
	yellow_taxi_trips t 
  
  WHERE
	"PULocationID" is NOT IN (SELECT "LocationID" from zones)

  LIMIT 100;
 ```

#### How to use left, right, and outer joins when some locations are not in either tables?

To mimic this scenario when some locations are not in the table, delete some samples

```sql
DELETE FROM zones WHERE "LocationID" = 142;
```

To use LEFT JOIN

```sql
SELECT 
	tpep_pickup_datetime,
	tpep_dropoff_datetime,
	total_amount,

FROM 
	yellow_taxi_trips t LEFT JOIN zones zpu
	  ON t."PULocationID" = zpu."LocationID"
	LEFT JOIN zones zdo
		ON t."DOLocationID" = zdo."LocationID"
  
LIMIT 100;
```

To use RIGHT JOIN

```sql
SELECT 
	tpep_pickup_datetime,
	tpep_dropoff_datetime,
	total_amount

FROM 
	yellow_taxi_trips t RIGHT JOIN zones zpu
	  ON t."PULocationID" = zpu."LocationID"
	RIGHT JOIN zones zdo
		ON t."DOLocationID" = zdo."LocationID"
  
LIMIT 100;
```

To use OUTER JOIN

```sql
SELECT 
	tpep_pickup_datetime,
	tpep_dropoff_datetime,
	total_amoun

FROM 
	yellow_taxi_trips t OUTER JOIN zones zpu
	  ON t."PULocationID" = zpu."LocationID"
	OUTER JOIN zones zdo
		ON t."DOLocationID" = zdo."LocationID"
  
LIMIT 100;
```

#### How to use GROUP BY?

To get the number of trips each day

```sql
SELECT 
	CAST(tpep_dropoff_datetime AS DATE) as "day", 
	COUNT(1)
	

FROM 
	yellow_taxi_trips t 
  
GROUP BY
	CAST(tpep_dropoff_datetime AS DATE);
```

#### How to use ORDER BY?

To order by day

```sql
SELECT 
	CAST(tpep_dropoff_datetime AS DATE) AS "day", 
	COUNT(1)
	
FROM 
	yellow_taxi_trips t 
  
GROUP BY
	CAST(tpep_dropoff_datetime AS DATE)

ORDER BY "day" ASC;
```

To order by count

```
SELECT 
	CAST(tpep_dropoff_datetime AS DATE) AS "day", 
	COUNT(1) AS "count"
	
FROM 
	yellow_taxi_trips t 
  
GROUP BY
	CAST(tpep_dropoff_datetime AS DATE)

ORDER BY "count" DESC;
```

#### How to perform other aggregations?

To get the maximum amount of money that the driver made

```sql
SELECT 
	CAST(tpep_dropoff_datetime AS DATE) AS "day", 
	COUNT(1) AS "count"
	MAX(total_amount),
	MAX(passenger_count)

FROM 
	yellow_taxi_trips t 
  
GROUP BY
	CAST(tpep_dropoff_datetime AS DATE)

ORDER BY "count" DESC;
```

#### How to GROUP BY multiple fields?

```
SELECT 
	CAST(tpep_dropoff_datetime AS DATE) AS "day", 
	"DOLocationId", 
	COUNT(1) AS "count"
	MAX(total_amount),
	MAX(passenger_count)

FROM 
	yellow_taxi_trips t 
  
GROUP BY
	1, 2

ORDER BY 
	"day" ASC,
  "DOLocationId" ASC;
```

### What is Google Cloud Platform?

- Cloud computing services offered by Google
- Includes a range of hosted services for compute, storage, and application development that run on Google hardware
- Same hardware on which Google runs its service (Google Search AdWords)

### What is Terraform?

1. What is??Terraform?
    - Open-source tool by??[HashiCorp](https://www.hashicorp.com/), used for provisioning infrastructure resources with declarative configuration files
        - These resources can be virtual machines, containers, storage, and networking resources
    - Supports DevOps best practices for change management
    - Managing configuration files in source control to maintain an ideal provisioning state for testing and production environments
	
2. What is IaC?
    - Infrastructure-as-Code
        - A framework
    - Build, change, and manage your infrastructure in a safe, consistent, and repeatable way by defining resource configurations that you can version, reuse, and share.
        - A git version control but for infrastructure
	
3. Some advantages
    - Infrastructure lifecycle management
    - Version control commits
    - Very useful for stack-based deployments, and with cloud providers such as AWS, GCP, Azure, K8S???
    - State-based approach to track resource changes throughout deployments

### How to setup a GCP infrastructure using Terraform?
    
    
    1. Install Terraform from this link based on your OS and follow installation guideline
