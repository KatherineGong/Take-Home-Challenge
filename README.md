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

Common Table Expressions (CTEs) can simplify queries and improve readability.

Following the instructions, I started by using two Common Table Expressions (CTEs) for distinct "SnpAssayName" to simplify the querying process. In the query, I used the CASE WHEN statement to determine the "APOE_Genotype". Furthermore, as "UniquePhenoID" serves as a unique identifier for individuals, I utilized it in conjunction with the date and the barcode to merge the two tables in the query. I performed a full JOIN because there are "UniquePhenoID" values that exist only in Table A. I exported Table 1 as a .csv file for testing purposes.

Table 2 is created by integrating the results from Table 1 into a Common Table Expression (CTE) named 'C'. Furthermore, a new column "APOE_Genotype" is introduced, which is determined based on the characteristics of the "Barcode" and "APOE_Genotype" features for each patient record. To achieve this, I created another CTE named 'D'. Finally, I utilized C to perform a left join with D to obtain the final table. 

Specifically, handling records with null values is crucial, as demonstrated in CTE 'D' of the process. For instance, when a unique barcode is present, it should be tagged as "Unverified" only if the 'APOE_Genotype' is not empty; otherwise, it should be classified as 'No Data'. Similar considerations have been accounted for throughout the creation process of CTE 'D'.

Similarly, I exported Table 2 as a .csv file for testing purposes.

# Task 12a Dockerfile
with detailed information on how you performed your analyses.
## Purpose
The objective of this task is to create a Dockerfile that constructs a Docker image containing several bioinformatics tools. Subsequently, the Docker image should be built, and a container should be launched from it.

## Output
Dockerfile: Document the commands to install STAR alignment software, Samtools software, and BEDTools software.

Task12a.pdf: Document the code, screenshots of building the Docker image, launching the interactive container, and verification of the successful installation.

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

# Task 16 Proteomics_Diff
## Output
Task16.pdf: Document the code and include comments to explain the results. 

Output.csv & Output_with_cov: Include the P-values, effect size, and false discovery rate (FDR) for each analyte ID.

## How to Solve this Problem
The main objective of this analysis is to investigate whether any proteins are associated with disease status. After loading the data, I first cleaned and preprocessed the protein dataset. For each column representing a specific protein, I checked for NA values to determine if any proteins were not present in enough samples (at least 70% of the 192 samples). Additionally, I checked for any duplicate protein names in the dataset. 

Since the NA values in the protein dataset were caused by outliers in the QA process, I did not impute them with any specific value, as the regression model will automatically exclude such values, and the NA value ratio in each column is not large. After this, I merged the protein data with the covariate data.

In exploring the relationship between disease status and protein levels, it's logical to designate disease as the independent variable and protein abundance levels as the dependent variable. Given the relatively high variance in protein abundance values, I employed log transformation to normalize them. After this preprocessing step, I opted for linear regression to model the relationship, as it facilitates result interpretation and aligns well with the nature of the analysis.

Function without covariates: log(protein_abundunce_value) ~ Disease_Status

Function with covariates: log(protein_abundunce_value) ~ Disease_Status + Age (+ Gender) + PlateId

In the model with covariates, I omitted gender from the regression model, as the dataset exclusively comprises female data. To ensure the code remains adaptable for future research involving different genders, I integrated an if statement to allow for the selection between two models.
