# SWYNEX - UNSW-NB15 Data Preparation

## Overview

This repository contains the data preparation and quality analysis
performed on the UNSW-NB15 network intrusion detection dataset as
part of the SWYNEX Technologies Data Science Internship.

The objective of this task was to inspect, validate, and prepare a
public cybersecurity dataset for subsequent data-science and
machine-learning work.

## Dataset

The project uses the UNSW-NB15 dataset, which contains network
traffic records representing normal activity and multiple
categories of network attacks.

### Files used

- `UNSW_NB15_training-set.csv`
- `UNSW_NB15_testing-set.csv`
- `UNSW-NB15_features.csv`

The dataset files are not included in this repository because of
their size.

### Dataset Source

Official UNSW-NB15 dataset:

https://research.unsw.edu.au/projects/unsw-nb15-dataset

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
Compared feature documentation with the prepared dataset schema.
Compared training and testing datasets for column and dtype
consistency.
Investigated unusual values in is_ftp_login.
Evaluated the id column as a record identifier.
Performed final validation of the prepared datasets.
Key Findings
Dataset dimensions
Dataset	Rows	Columns
Training	175,341	45
Testing	82,332	45
Data quality

The audit found:

No missing values in the training or testing data.
No duplicate rows.
No infinite numerical values.
No negative numerical values.
Matching columns between training and testing datasets.
Matching Pandas data types between training and testing datasets.

Because no missing or duplicate records were identified, no
missing-value imputation or duplicate-row removal was required.

Target Variables

The label column provides a binary target:

0 = Normal
1 = Attack

The training dataset contains:

56,000 normal records
119,341 attack records

The attack_cat column provides more detailed attack categories,
including:

Normal
Generic
Exploits
Fuzzers
DoS
Reconnaissance
Analysis
Backdoor
Shellcode
Worms

The relationship between label and attack_cat was also checked.
The Normal category corresponds to label = 0, while the attack
categories correspond to label = 1 in the training data.

Feature Documentation Analysis

The supplied feature-description file contains 49 documented fields,
while the prepared training and testing datasets contain 45 columns.

Several differences were found to be naming variations rather than
missing values. Examples include:

Documentation	Dataset
Spkts	spkts
Dpkts	dpkts
Sload	sload
Dload	dload
smeansz	smean
dmeansz	dmean
res_bdy_len	response_body_len
Sintpkt	sinpkt
Dintpkt	dinpkt

The documented fields srcip, sport, dstip, dsport, Stime,
and Ltime are not present in the prepared train/test files used
for this project. These fields were not reconstructed or imputed.

Categorical Feature Analysis

Categorical variables were inspected to understand their
distributions and consistency.

Some categories were present in only one dataset split.

For example:

proto: icmp and rtp occur only in the training data.
state: ECO, PAR, URN, and no occur only in training,
while ACC and CLO occur only in testing.

These categories were retained because their absence from one split
does not by itself indicate invalid data.

is_ftp_login Investigation

The feature documentation describes is_ftp_login as a binary
feature. However, the prepared training data contains the values:

0, 1, 2, 4

The values 2 and 4 are rare and occur in FTP records.

An additional analysis showed that is_ftp_login and ct_ftp_cmd
have identical values across the observed training records,
including the values 2 and 4.

Because the supplied documentation does not provide a justified
rule for recoding the values 2 and 4, the original values were
retained rather than being arbitrarily modified.

Identifier Analysis

The id column contains 175,341 unique values for 175,341 training
records, indicating that it functions as a unique record identifier.

The identifier was retained in the prepared dataset to preserve the
source record structure. It should not be treated as a predictive
feature during subsequent machine-learning modeling.

Cleaning Decisions

The analysis found that the dataset did not require extensive
row-level cleaning.

The following actions were taken or documented:

Column names were standardized by removing leading and trailing
whitespace.
No missing values were imputed.
No duplicate rows were removed.
No infinite values were removed.
No negative values were removed.
Rare categorical values were retained.
Extreme numerical values were retained because their magnitude
alone did not establish that they were invalid.
Dataset/documentation naming differences were documented.
The is_ftp_login inconsistency was investigated and retained
without arbitrary recoding.
id was identified as an identifier rather than a predictive
feature.
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
Tools Used
Python
Pandas
NumPy
Matplotlib
Seaborn
Google Colab
GitHub
Notebook

The complete analysis is available in:

notebook/01_data_preparation.ipynb

Future Work

This prepared dataset can be used for subsequent analysis and
machine-learning work, including:

Exploratory data analysis
Binary intrusion detection
Multiclass attack classification
Feature analysis
Model evaluation
Network intrusion detection applications

These activities are outside the scope of this data-preparation
task.
