# SWYNEX - UNSW-NB15 Data Preparation

## Overview

This project focuses on preparing the UNSW-NB15 network intrusion
detection dataset for data-science and machine-learning work.

It was completed as part of the SWYNEX Technologies Data Science
Internship.

The broader project is focused on network intrusion detection:
using network traffic characteristics to distinguish normal network
activity from malicious activity and, in later stages, identify
different categories of attacks.

This repository specifically covers the **data preparation stage**.
The dataset was inspected, validated, and documented before being
used for subsequent exploratory analysis and machine-learning work.

---

## Project Description

Network traffic contains measurable characteristics such as
protocol, service, connection state, packet counts, byte counts,
traffic rates, timing information, and connection statistics.

These characteristics can be analyzed to identify patterns
associated with normal and malicious network activity.

The UNSW-NB15 dataset provides both normal network records and
multiple categories of attacks, making it suitable for studying
network intrusion detection using data-science techniques.

The broader project can be approached as two related classification
problems:

1. **Binary intrusion detection**

   Determine whether a network record represents normal activity or
   an attack.

2. **Multiclass attack classification**

   For malicious traffic, identify the category of attack.

The current repository focuses on preparing and validating the data
for these later stages.

---

## Current Scope

This repository covers the data-preparation stage of the project.

The work includes:

- Loading the dataset and feature-description file
- Inspecting dataset dimensions and structure
- Examining data types
- Checking missing values
- Checking duplicate records
- Checking infinite numerical values
- Checking negative numerical values
- Investigating categorical variables
- Examining target variables
- Comparing training and testing datasets
- Comparing dataset columns with the supplied feature documentation
- Investigating unusual values in `is_ftp_login`
- Evaluating the `id` column
- Performing final data-quality validation
- Documenting preparation decisions and assumptions

Machine-learning modeling, feature encoding, model comparison, and
deployment are outside the scope of this Task 1 repository.

---

## Dataset

The project uses the **UNSW-NB15** network intrusion detection
dataset.

The dataset contains network traffic records representing normal
network activity and multiple categories of network attacks.

### Dataset Files Used

- `UNSW_NB15_training-set.csv`
- `UNSW_NB15_testing-set.csv`
- `UNSW-NB15_features.csv`

The dataset files themselves are not included in this repository
because of their size.

### Dataset Source

Official UNSW-NB15 Dataset:

https://research.unsw.edu.au/projects/unsw-nb15-dataset

---

## Project Structure

```text
SWYNEX-Data-Preparation/
│
├── data/
│   └── README.md
│
├── notebook/
│   └── 01_data_preparation.ipynb
│
├── .gitignore
└── README.md
Data Preparation Process

The dataset was examined through the following stages:

Loaded the training, testing, and feature-description files.
Inspected dataset dimensions and column names.
Examined Pandas data types.
Checked for missing values.
Checked for duplicate records.
Checked for infinite numerical values.
Checked for negative numerical values.
Investigated categorical variables such as proto, service,
and state.
Examined the target variables label and attack_cat.
Compared the feature documentation with the prepared dataset
schema.
Compared training and testing datasets for column and data-type
consistency.
Investigated unusual values in is_ftp_login.
Evaluated the id column as a record identifier.
Performed final validation of the prepared datasets.
Key Findings
Dataset Dimensions
Dataset	Rows	Columns
Training	175,341	45
Testing	82,332	45
Data Quality

The audit found:

No missing values in the training or testing data.
No duplicate rows.
No infinite numerical values.
No negative numerical values.
Matching columns between training and testing datasets.
Matching Pandas data types between training and testing datasets.

Because no missing or duplicate records were identified, no
missing-value imputation or duplicate-row removal was required.

Extreme numerical values were also investigated. Large values were
not automatically removed because their magnitude alone did not
establish that they were invalid network measurements.

Target Variables
label

The label column provides a binary target:

0 = Normal
1 = Attack

The training dataset contains:

Label	Meaning	Records
0	Normal	56,000
1	Attack	119,341
attack_cat

The attack_cat column provides a more detailed classification of
the network records.

The training dataset contains the following categories:

Attack Category	Records
Normal	56,000
Generic	40,000
Exploits	33,393
Fuzzers	18,184
DoS	12,264
Reconnaissance	10,491
Analysis	2,000
Backdoor	1,746
Shellcode	1,133
Worms	130

The relationship between label and attack_cat was also checked.

In the training dataset:

Normal corresponds to label = 0.
The attack categories correspond to label = 1.
Categorical Feature Analysis

Categorical variables were investigated to understand their
distributions and consistency between the training and testing
datasets.

Protocol

The proto feature contains multiple protocol categories.

The following categories were found only in the training dataset:

icmp
rtp

No protocol categories were found only in the testing dataset.

These categories were retained because their absence from the
testing split does not by itself indicate invalid data.

Service

The service feature contains the same set of categories in both
training and testing datasets.

The category - is present as a value in the dataset and was
treated as an observed category rather than automatically replacing
it with a missing value.

State

Some state categories differ between the training and testing
datasets.

Training-only categories:

ECO
PAR
URN
no

Testing-only categories:

ACC
CLO

These categories were retained because a category occurring in only
one dataset split does not by itself establish that the values are
invalid.

Feature Documentation Analysis

The supplied feature-description file contains 49 documented
fields, while the prepared training and testing datasets contain
45 columns.

Several differences are naming variations rather than missing
values.

Examples include:

Documentation Name	Dataset Column
Spkts	spkts
Dpkts	dpkts
Sload	sload
Dload	dload
smeansz	smean
dmeansz	dmean
res_bdy_len	response_body_len
Sintpkt	sinpkt
Dintpkt	dinpkt
ct_src_ ltm	ct_src_ltm
Label	label

The following documented fields are not present in the prepared
training and testing datasets used in this project:

srcip
sport
dstip
dsport
Stime
Ltime

These fields were not reconstructed or imputed because their values
are not available in the selected prepared dataset files.

The differences between documentation names and dataset column names
were documented rather than arbitrarily changing the dataset schema.

is_ftp_login Investigation

The supplied feature documentation describes is_ftp_login as a
binary feature.

However, the prepared training dataset contains the following
values:

0
1
2
4

The observed distribution was:

Value	Records
0	172,774
1	2,545
2	6
4	16

The values 2 and 4 occur only in FTP records.

An additional analysis showed that is_ftp_login and ct_ftp_cmd
have identical values across the observed training records,
including the values 2 and 4.

Because the supplied documentation does not provide a justified
rule for recoding the values 2 and 4, the original values were
retained rather than being arbitrarily modified.

This discrepancy is documented as a data-definition inconsistency
for consideration during later feature engineering and modeling.

Identifier Analysis

The id column contains:

175,341 unique IDs
175,341 training records

This indicates that id functions as a unique record identifier.

The identifier was retained in the prepared dataset to preserve the
original record structure.

However, id should not be treated as a predictive feature during
subsequent machine-learning modeling because it represents record
identity rather than network behavior.

Cleaning Decisions

The analysis found that the dataset did not require extensive
row-level cleaning.

The following decisions were made:

Column names were standardized by removing leading and trailing
whitespace.
No missing values were imputed because no missing values were
found.
No duplicate rows were removed because no duplicates were found.
No infinite values were removed because none were found.
No negative numerical values were removed because none were found.
Rare categorical values were retained.
Extreme numerical values were retained because their magnitude
alone did not establish that they were invalid.
Dataset/documentation naming differences were documented.
The is_ftp_login inconsistency was investigated and retained
without arbitrary recoding.
id was identified as a record identifier rather than a
predictive feature.
Final Validation

After preparation, the datasets were validated again.

Training shape: (175341, 45)
Testing shape : (82332, 45)

Missing values:
Training: 0
Testing : 0

Duplicate rows:
Training: 0
Testing : 0

Infinite values:
Training: 0
Testing : 0

Columns identical: True
Data types identical: True

The final validation confirmed that the prepared training and
testing datasets retained their expected structure and did not
contain the checked data-quality issues.

Tools Used
Python
Pandas
NumPy
Matplotlib
Seaborn
Google Colab
GitHub
Notebook

The complete data-preparation analysis is available in:

notebook/01_data_preparation.ipynb
Future Work

The prepared dataset provides a foundation for subsequent
cybersecurity data-science work, including:

Exploratory data analysis
Network traffic pattern analysis
Binary intrusion detection
Multiclass attack classification
Feature analysis
Machine-learning model development
Model evaluation
Development of a network intrusion detection application

These activities are outside the scope of the current Task 1 data
preparation repository.
