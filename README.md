# Data-Compression-and-Transmission-for-Fault-Detection-of-Induction-Machine
Wireless transmission using Raspberry pi for fault detection and Diagnosis of Stator Winding Faults due to Insulation Failure in Industrial Induction Machine

## Introduction

Fault detection of Induction motors using the current signals is a growing technology to detect the fault of an induction motor. Definite harmonic signals of the line current are located by a popular method known as Motor Current Signature Analysis. The actual fault detection by using the human involvement is widely replaced by the automated technology, like an on-line condition monitoring approach. Here we implement a real-time computer-aided advanced diagnosis system comprising of feature extraction, feature selection and feature classification.  

![blockdig](https://user-images.githubusercontent.com/22619455/222921899-797798e2-0eb6-48e7-b79c-d6c4025692e4.png)

## Steps Involved

- Data Acquisition  - DAQ is used to transmit data from the sensors to raspberry using wired connection.
- Data Processing - Based on sampling frequency (here 10kHz) we process the data, extract features and transmit relevant features through wireless connection.
- Data Transmission - Zigbees are used for wireless transmission from raspberry to computer where fault detection and analysis is done. 
- Classification Model - Artificial Neural network, Support vector Machine and random forest models are used for Fault Detection and Diagnosis.

## Data Processing

Transmitting high frequency data using wireless medium economically is unrealistic due to the datacap on the transmission modules. Hence, features like skewness and kurtosis are extracted at regular intervals from the input stream before the transmission.
The data acquisition system samples vibration data at 10 kHz in file format of .lvm extension in one second which are then sent as only 10 data points with best and minimum possible features.
These features are converted to .csv extension and then read by python as DataFrame which are then transferred through wireless medium.

## Features

- Mean(x̅):The arithmetic mean (or simply mean) of a sample , is the sum of the sampled values divided by the number of items in the example.
- Standard deviation(σ):The Standard Deviation is a measure of how spread out numbers are.Its symbol is σ(the greek letter sigma).
- Skewness:Skewness essentially measures the relative size of the two tails.
- Shape factor :It is the  value that is affected by an object's Shape but is independent of its dimensions. 
- Impulse Factor:  It is the maximum value divided by the mean of the absolute values of data entries.
- Margin Factor:  It is the maximum value divided by the square of the mean of the absolute values of data entries.
- Current: current source of I1,I2,I3 are considered as 3 separate categorical features.

In total we extrapolated 15 features from dataset which will be reduced using feature selection.

## Classification Models

- Artificial Neural Network
We trained a model with inputs as the statistical features of the current data and output as the state of the Induction Motor.
- Support Vector Machines
In machine learning, support-vector machines are supervised learning models with associated learning algorithms that analyze data used for classification and regression analysis.
- Random Forests
Random forest algorithm is a supervised classification algorithm.this algorithm creates the forest with a number of trees.The more number of trees, the more robust the forest looks like.

## Feature Selection

Feature selection is done by computing the ANOVA F-value for the provided sample of all the features.

- Selecting all 12 features and 3 categorical variables(current)
<img width="807" alt="image" src="https://user-images.githubusercontent.com/22619455/222922091-ff70fcc6-fb16-4409-b865-492ef07ded31.png">

- Selecting all 12 features without categorical variables.
<img width="822" alt="image" src="https://user-images.githubusercontent.com/22619455/222922115-91f39378-00c9-4eea-809e-fb2dbc79f72c.png">

- Selecting best 9 features in our model are standard deviation, skewness, RMS value, max, pk-pk, Margin factor , kurtosis, min and Impulse factor as per our anova test.
<img width="809" alt="image" src="https://user-images.githubusercontent.com/22619455/222922143-c8395f73-c180-4578-904d-44f9f6532f42.png">

- Selecting best 6 features in our model are standard deviation, skewness, RMS value, max, pk-pk, and Margin factor as per our anova test.
<img width="803" alt="image" src="https://user-images.githubusercontent.com/22619455/222922176-74be555f-01fe-42fa-b2fd-273c29b405b2.png">

- Selecting best 3 features in our model are standard deviation, skewness and RMS value as per our anova test.
<img width="788" alt="image" src="https://user-images.githubusercontent.com/22619455/222922201-fd6edc38-c5c8-43c2-958a-da1fc68dab1e.png">

## Pair plot of best 3 features
<img width="332" alt="image" src="https://user-images.githubusercontent.com/22619455/222922211-97a886bf-0301-49ab-802c-f1b06a410de4.png">

## ZigBee

- A wireless technology for high -level communication protocols
- Low power consumption and cheaper than WPANs (eg. bluetooth or Wi-Fi)
- ZigBee Coordinator: Forms root of the network tree, acts as coordinator in each network
- ZigBee Router: Acts as intermediate router, passes data from other devices
- ZigBee End Device: Talks to parent node i.e., coordinator and router

### ZigBee Protocol Architecture:
<img width="432" alt="image" src="https://user-images.githubusercontent.com/22619455/222922246-b63e2497-29d4-4cfa-bb84-de06df770be4.png">

### Transmission of data using ZigBee Protocol

- API used : digi.xbee
- Digi.xbee uses pySerial for serial communication between devices
- Specifications:250Kbps
- Achieved:48bps
- Each row in dataframe is sent as a separate bytearray.

## Transmitter’s side code

<img width="323" alt="image" src="https://user-images.githubusercontent.com/22619455/222922319-a16c4369-861c-47fa-8fca-813c83a623b6.png">
<img width="331" alt="image" src="https://user-images.githubusercontent.com/22619455/222922326-4e77e770-6f29-4e6e-a5bd-d96575c5ef49.png">

## Receiver’s Side Code

<img width="336" alt="image" src="https://user-images.githubusercontent.com/22619455/222922335-4b9a5529-e074-4265-a0e9-652d65c1dc6c.png">
<img width="287" alt="image" src="https://user-images.githubusercontent.com/22619455/222922338-915a4c9c-6e4d-4975-a579-7654cfe47a36.png">








