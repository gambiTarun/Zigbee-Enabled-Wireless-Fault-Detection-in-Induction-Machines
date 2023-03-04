# Data-Compression-and-Transmission-for-Fault-Detection-of-Induction-Machine
Wireless transmission using Raspberry pi for fault detection and Diagnosis of Stator Winding Faults due to Insulation Failure in Industrial Induction Machine

## Introduction

Fault detection of Induction motors using the current signals is a growing technology to detect the fault of an induction motor. Definite harmonic signals of the line current are located by a popular method known as Motor Current Signature Analysis. The actual fault detection by using the human involvement is widely replaced by the automated technology, like an on-line condition monitoring approach. Here we implement a real-time computer-aided advanced diagnosis system comprising of feature extraction, feature selection and feature classification.  

![blockdig](https://user-images.githubusercontent.com/22619455/222921899-797798e2-0eb6-48e7-b79c-d6c4025692e4.png)

## ZigBee

In the current world, we have many high data rate communication techniques available, but none of these are able to meet the communication standards of sensors and control devices. These communication standards require high data rate at low-latency and low-energy consumption  even for smaller bandwidths. Zigbee technology is a wireless low power and low cost technology .It has excellent and superb characteristics which have made this communication most suitable for embedded applications ,home automation etc. It is especially built for sensor networks on IEEE 802.15.4 standard for wireless personal area networks (WPANs), . It is a product from Zigbee alliance. This communication technology defines physical and Media Access Control (MAC) layers for handling many devices at low-data rates. Zigbee WPANs work at 868 MHz, 902-928 MHz and 2.4 GHz frequencies. The best suited data rate is 250 kbps for periodic or intermediate two way transmission of data between controllers and sensors . Zigbee is a  low-cost and power network mostly deployed to control and monitor areas where we need to cover only 10-100 meters within the range. This is a less expensive communication system and it is simpler than other proprietary short-range wireless sensor networks  as Bluetooth and Wi-Fi.

Zigbee system structure mainly has three different types of devices 
●	**Zigbee coordinator**: This forms the root of the network. The Mandatory node for all zigbee networks which has all the information of the network including the keys and acts as a trust centre  playing a key role in the security. This device can never sleep.
●	**Zigbee Router**:  This node can run an application function as well as act as a relay station for other zigbee devices in the network. The devices on this node can never sleep too.
●	**End device**:  End devices have a very limited work which is to communicate with the parent nodes such that the battery power is saved. These device can sleep to save power.

In the network currently implemented, there are only two nodes. One acting as the coordinator at the ground station and the other acting as an End point near the sensors. We used 802.15.4 protocol to achieve higher point to point transmission speed but this restricts the network to be only a point to point with no possibility of forming meshes of nodes. 

<img width="401" alt="image" src="https://user-images.githubusercontent.com/22619455/222922899-6fad0005-23b9-4342-946a-90e900fa86d3.png">


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


## Steps Involved

- **Data Acquisition**  - DAQ is used to transmit data from the sensors to raspberry using wired connection.
- **Data Processing** - Based on sampling frequency (here 10kHz) we process the data, extract features and transmit relevant features through wireless connection.
- **Data Transmission** - Zigbees are used for wireless transmission from raspberry to computer where fault detection and analysis is done. 
- **Classification Model** - Artificial Neural network, Support vector Machine and random forest models are used for Fault Detection and Diagnosis.

## Data Processing

Data needs to be transmitted from Raspberry to a local computer. Artificial neural networks is trained to check the condition of the machine,whether it is in healthy or faulty state. In simple terms, Zigbees are used for wireless transmission from raspberry to computer where fault detection and analysis is done. 

Transmitting high frequency data using wireless medium economically is unrealistic due to the datacap on the transmission modules. Hence, features like skewness and kurtosis are extracted at regular intervals from the input stream before the transmission.
The data acquisition system samples vibration data at 10 kHz in file format of .lvm extension in one second which are then sent as only 10 data points with best and minimum possible features.
These features are converted to .csv extension and then read by python as DataFrame which are then transferred through wireless medium.

## Features

- **Mean(x̅)**:The arithmetic mean (or simply mean) of a sample , is the sum of the sampled values divided by the number of items in the example.
- **Standard deviation(σ)**:The Standard Deviation is a measure of how spread out numbers are.Its symbol is σ(the greek letter sigma).
- **Skewness**:Skewness essentially measures the relative size of the two tails.
- **Shape factor** :It is the  value that is affected by an object's Shape but is independent of its dimensions. 
- **Impulse Factor**:  It is the maximum value divided by the mean of the absolute values of data entries.
- **Margin Factor**:  It is the maximum value divided by the square of the mean of the absolute values of data entries.
- **Current**: current source of I1,I2,I3 are considered as 3 separate categorical features.

In total we extrapolated 15 features from dataset which will be reduced using feature selection.

## Classification Models

- **Artificial Neural Network**: 
We trained a model with inputs as the statistical features of the current data and output as the state of the Induction Motor.
- **Support Vector Machines**: 
In machine learning, support-vector machines are supervised learning models with associated learning algorithms that analyze data used for classification and regression analysis.
- **Random Forests**: 
Random forest algorithm is a supervised classification algorithm.this algorithm creates the forest with a number of trees.The more number of trees, the more robust the forest looks like.

## Feature Selection

Here we will be training three different models on the training dataset and testing the performance of the trained model on a test dataset. The feature selection for the dataset is varied from 3 features to 12 features out of maximum 15 features and their results are shown as below.

- Best 3 features as per our model are standard deviation, skewness and RMS value as per our anova test:
<img width="311" alt="image" src="https://user-images.githubusercontent.com/22619455/222922677-88fcadd7-1e46-4813-a156-bca97292cca4.png">
Pair Plot:
<img width="790" alt="image" src="https://user-images.githubusercontent.com/22619455/222923022-d1aafc2c-9098-4c62-a7b7-99569407e178.png">

- Best 6 features as per our model are standard deviation, skewness, RMS value, max, pk-pk, and Margin factor as per our anova test:
<img width="340" alt="image" src="https://user-images.githubusercontent.com/22619455/222922732-bea57e31-f08f-448e-892a-0934c00582d4.png">

- Best 9 features in our model are standard deviation, skewness, RMS value, max, pk-pk, Margin factor , kurtosis, min and Impulse factor as per our anova test:
<img width="358" alt="image" src="https://user-images.githubusercontent.com/22619455/222922784-2e6875bf-3b0b-48fc-b5a7-8472f2f86993.png">
Pair Plot:
<img width="908" alt="image" src="https://user-images.githubusercontent.com/22619455/222923031-4942f61b-2a74-415a-9b83-9615ab845dad.png">

- Selecting all 12 features and 3 categorical variables(current)
<img width="305" alt="image" src="https://user-images.githubusercontent.com/22619455/222922812-318d2714-6955-459a-a54e-c37644630599.png">

- **Conclusion**: 3 best features model is selected as accuracy obtained is high and less data needs to be transferred via Zigbee.







