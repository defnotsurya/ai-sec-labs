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
### Feature Scaling

Network traffic features can vary widely in scale. For example:

Packet sizes may range between 0–1500 bytes

Flow duration may reach millions of microseconds

To normalize these values, the dataset is standardized using a StandardScaler.

This transformation ensures all features contribute equally during model training.
```
scaler = StandardScaler()

X_train = scaler.fit_transform(X_train)
X_test = scaler.transform(X_test)
```

### Model Training

The intrusion detection system uses a Random Forest classifier, an ensemble learning method that combines multiple decision trees.

Each decision tree learns patterns in the traffic data that distinguish benign behavior from attack activity.

The final prediction is determined through majority voting across the trees.
```
model = RandomForestClassifier(
n_estimators=100,
random_state=42,
n_jobs=-1
)

model.fit(X_train, y_train)
```
]

## Model Evaluation

### Confusion Matrix

The confusion matrix shows how predictions compare to actual labels.

Example structure:

Actual - BENIGN, DDoS, PortScan

Predicted - BENIGN, DDoS, BENIGN

This helps identify:

False positives (benign traffic flagged as attack) and ]]False negatives (attacks missed by the system)

|                   | Predicted BENIGN | Predicted ATTACK |
| ----------------- | ---------------- | ---------------- |
| **Actual BENIGN** | 37765            | 0                |
| **Actual ATTACK** | 34               | 384              |

<img width="196" height="81" alt="Screenshot 2026-03-16 175627" src="https://github.com/user-attachments/assets/66e0ed1a-279c-4e08-b33c-cd1018aaf923" />

### Classification Metrics

The following metrics provide deeper performance insight:

Metric	Meaning
Precision	Percentage of predicted attacks that were actually malicious
Recall	Percentage of real attacks correctly detected
F1 Score	Balance between precision and recall

For intrusion detection systems, high recall is critical, since missed attacks represent security risks.

<img width="414" height="169" alt="Screenshot 2026-03-16 175636" src="https://github.com/user-attachments/assets/44b21488-7391-467b-9d8f-c65ac00ab76e" />

## Feature Importance Analysis

Random Forest models provide feature importance scores that indicate which network traffic characteristics influenced predictions the most.

The following visualization highlights the top features used for attack detection.

<img width="1029" height="520" alt="image" src="https://github.com/user-attachments/assets/45536bf9-db76-4a0b-bd73-40c6e52286ca" />

These features correspond to known attack behaviors such as traffic flooding, abnormal packet patterns, and high-volume scanning activity

## Security Insights

The analysis demonstrates that machine learning can identify abnormal traffic patterns associated with common cyber attacks.

For example:

Attack Type	Detected Pattern
DDoS	Extremely high traffic rate
Port Scans	High number of short flows
Botnet Activity	Repeated automated packet behavior

These insights highlight how AI driven detection systems can augment traditional network security monitoring.



































