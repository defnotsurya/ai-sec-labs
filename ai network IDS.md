# AI Network intrusion Detection System

## Overview
The goal of this project is to implement a machine learning based network IDS that is able to identify malicious network traffic. The model is trained using the CICIDS2017 dataset, which has labled network flow data representing both benign and malicious network activity such as DDoS attacks, botnet traffic, and port scans.

This was the process of the project.
Dataset → Data Cleaning → Feature Processing → Model Training → Evaluation

The trained model learns patterns in the network traffic and is then able to predict whether the packet flow is benign or malicous.

## Dataset 

Link to dataset: https://www.unb.ca/cic/datasets/ids-2017.html

This project uses the CICIDS2017 Dataset which I decided upon since it has been widely used in academic research for evaluating intrusion detection systems.

The dataset includes network traffic flows with labled with different types of attacks.

Examples:
BENIGN

DDoS

PortScan

Botnet Activity

Infiltration

There are also network flow features that help identify abnormal traffic patterns.

Feature	Description
Flow Duration	
Total 
Packet Length Mean	
Flow Bytes/s	
Flow Packets/s

## Machine Learning

### Loading Data
Imported several python libraries to help with data analysis and for the ML model. Then the dataset is loaded into a pandas DataFrame for analyis and cleaning. 
```
import pandas as pd
import numpy as np

from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler, LabelEncoder
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import classification_report, confusion_matrix, accuracy_score
```
```
data = pd.read_csv(r"C:\Users\crazy\OneDrive\Music\Documents\AI sec lab\data\Friday-WorkingHours-Morning.pcap_ISCX.csv")
```
### Data Cleaning
The dataset includes lots of invalid values and extra spaces which can cause issues. Infinite values and missing values are removed to prevent erros during model training
```
data.columns = data.columns.str.strip()
```
```
data.replace([np.inf, -np.inf], np.nan, inplace=True)
data.dropna(inplace=True)
```
### Feature and Label Seperation
The dataset is seperated into Feature variables (X) - network traffic characteristics and Target variable (y) - traffic classification label
```
X = data.drop(columns=['Label'])
y = data['Label']
```
### Label Encoding
Machine learning models require numeric labels. The attack categories are converted from text labels into numeric values.

Example
Traffic Type - BENIGN, Bot, DDoS, PortScan

Encoded Value - 0, 1, 2, 3
```
encoder = LabelEncoder()
y = encoder.fit_transform(y)

print(encoder.classes_)
```

### Train/Test Split

The dataset is split into training and testing sets.

Training set (80%) is used to train the model

Testing set (20%) is used to evaluate performance

This ensures the model is evaluated on unseen data, preventing overfitting.
```
X_train, X_test, y_train, y_test = train_test_split(
    X, y,
    test_size=0.2,
    random_state=42
)
```


































