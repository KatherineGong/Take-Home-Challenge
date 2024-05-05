# Take-Home-Challenge
# Task 11 SQL
## Purpose
Develop and execute an SQL query to ascertain the APOE genotype for each individual using the available table data, and assign a validation status to the genotype.

## Output
Task11.pdf: Outlined the steps for inputting the data, provided screenshots of the code used to create table1 and table2, and displayed the output of these two tables.

Table1.csv & Table2.csv: Two query tables for testing purposes.

## How to Use
The PDF contains instructions for inputting the data from APOE_toy_Data.csv and utilizing the provided code to complete the tasks.

## How to Solve this Problem
Utilize PostgreSQL, a powerful open-source relational database management system (RDBMS) known for its full compliance with the SQL standard and robust data integrity features. PostgreSQL benefits from a large and active community of developers and users who contribute to its development and provide support, making it an ideal choice to solve this problem.

Input the data, starting by creating the table in PgAdmin4,
```
                                        Table "public.APOE_toy_Data"
    Column     | Type | Collation | Nullable | Default | Storage  | Compression | Stats target | Description 
---------------+------+-----------+----------+---------+----------+-------------+--------------+-------------
 DateofApoe    | date |           |          |         | extended |             |              | 
 PI_Name       | text |           |          |         | extended |             |              | 
 Well          | text |           |          |         | extended |             |              | 
 Barcode       | text |           |          |         | extended |             |              | 
 UniquePhenoID | text |           |          |         | extended |             |              | 
 SnpAssayName  | text |           |          |         | extended |             |              | 
 Call          | text |           |          |         | extended |             |              | 
```
According to the Input Data Dictionary, the 'DateofApoe' column represents the date the assay was run for that barcode. Therefore, I have set this column as a Date type.

The other columns contain a mixture of numbers and characters. For the 'barcode' column, the length is not fixed, and it can be considered as a 'tag'. Therefore, based on this, all the remaining columns are set as text.

# Task 12a Dockerfile
with detailed information on how you performed your analyses.
## Purpose
The objective of this task is to create a Dockerfile that constructs a Docker image containing several bioinformatics tools. Subsequently, the Docker image should be built, and a container should be launched from it.

## Output
Dockerfile: Document the commands to install STAR alignment software, Samtools software, and BEDTools software.

task12a.pdf: Document the code, screenshots of building the Docker image, launching the interactive container, and verification of the successful installation.

## How to Use
With the Dockerfile provided, proceed to the terminal and input the commands to build the image.

`docker build -t <image name> -f "<location of the dockerfile>" .`

Next, use the image to create an interactive container.

`docker run -it <image name>`

The following commands can be used to verify the successful installation of the three tools.
`STAR-2.7.11b/bin/Linux_x86_64_static/STAR --help`

`bedtools2/bin/bedtools --help`

`samtools-1.19/samtools --help`

## How to Solve this Problem
Break the task into two parts: first, install the necessary tools; second, once the Dockerfile is completed, build an interactive container to verify the successful installation. 

apt-get is used to install the dependencies required for the compilation process.

Since all three tools are Linux-supported and have minimal version requirements, they can be installed on the Ubuntu:20.04 base image. 

Next, visit the official GitHub repository where they have provided instructions on how to install them in the Dockerfile.
[STAR](https://github.com/alexdobin/STAR)
[Samtools](https://github.com/samtools/samtools)
[BEDTools](https://github.com/arq5x/bedtools2)
