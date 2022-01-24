# A Dataset Bundle for Building Automation and Control Systems
**useful for Security Analysis and to study the normal operation of these systems**

This document describes a dataset bundle with diverse types of attacks, and also a not poisoned dataset. The capture was obtained in a real house with a complete Building Automation and Control System (BACS). The data is available online and also reachable publicly at the IEEE DataPort repositóry via DOI ***XXXXXXXX***. This document describes the several included datasets and how their data can be employed in security analysis of KNX based building Automation.

The future of modern buildings and their comfort conditions
combined with the ecological need for a sustained planet will necessarily lead to increases in building automation systems. The benefit of using these technologies is however associated with new attack possibilities and system intrusions as a result of increasingly connectivity and remote accessibility. [Graveto and al.] (https://www.sciencedirect.com/science/article/pii/S0167404821003515 *"Security of Building Automation and Control Systems: Survey and future research directions"*) presents a state of the art about the security, safety and privacy issues in Building Automation and Control Systems (BACS). As a result, all the players evolved in building automation are concerned about the need of improvement of service quality, security and reliability of those systems.
The search for required datasets to enhance the models and the methods used to study and improve BACS expose the lack of such tools and even full lack of datasets.

## Collection process

Data collection was carried out in a single-family house, consisting of three floors. This house is equipped with a KNX home automation system that controls the lighting, blinds, the heating system that uses radiators and the alarm system. Some local environments are also controlled, with light and motion detectors. The building has devices for monitoring the environmental conditions outside, as well as movement and lighting control in the backyard of the house and its annexes.

### The Scenario Description
The house topology can be described as follows:
- the 1st floor is made up of four bedrooms, all with private bathroom, one of which additionally has a dressing room the suite;
- the main floor comprises an entrance hall, a library, the piano room, a common room (dining and living room) with a balcony, a kitchen with storage and a pantry;
- the basement floor has a laundry room, a living room, two offices and some technical and storage areas.
- outside exists an annex, a garage and a swimming pool.

The existing home automation system can be summarized in the following points:
- there are in the annex and on each floor a power cabinet and a control cabinet;
-  there is a KNX twisted-pair data bus that allows communication between the devices in the control cabinets and the devices scattered throughout the building;
- in the lighting system, all the luminaire cables are routed to the control cabinet where the actuators are located, with sensors and switches located in each of the rooms;
- central heating is guaranteed to each room by temperature sensors of the various switches and solenoid valves individually controlled. Individual control of radiators is enabled by actuators located on the control cabinet. Thus, guaranteeing the possibility of different environments in the various rooms of the house;
- the shutters are electrically operated from the control cabinet and, locally, there are switches that allow selection of the desired functions;
- the existing motion and light sensors are used with the dual function of controlling the environment and  the alarm system;
- centralized control is also possible, enable by an existing touch panel. This device offers individual or grouped acting of the different equipment's. The use of timers and actions resulting from previously established conditions are also allowed.

### Used formats
The collected data was stored in PCAP format. Each packet contains the raw message as a byte array and the timestamp representing the instant that message was received on the bus. 

Then, during the treatment and enrichment process, a comma separated values (CSV) file is created. The collected dataset is augmented, using the exported project file of the house, obtained from Engineering Tool Software (ETS). The added information is represented on fields: ETS Function; Location; Location Type and Location Name. The CSV file contains the following fields:
- Pkt - the packet number;
- TimeStamp - the timestamp representing the instant the packet was received;
- InterMessageTime - the time between two consecutive messages;
- NumMessagesSec - the number of messages per second;
- AvgMessagesSec -the moving average of messages per second;
- MessageSize - the message size;
- MessageChecksum - the KNX message checksum;
- SA\_text - the source address (SA) as text;
- SA\_int - the source address value;
- DA\_text - the destination address (DA) as text, using dots in individual addresses (IA) or slashes in group addresses (GA);
- DA\_int - the destination address value;
- KNX\_Info - the KNX operation in text;
- KNX\_info\_int - the KNX operation value;
- KNX\_Code - the KNX code;
- KNX\_Value - the value send as the operation argument;
- Msg - the byte array of the KNX message;
- EtsFunction - the known function;
- Location - the device location;
- LocationType - the location type;
- LocationName - the location name; 
- Target - the classification field that is \textit{zero} for messages of the normal operation and \textit{one} for poison messages (either attack and the respective response from the system).


### Sampling interval
The sampling took place uninterruptedly between 3:26 pm of 7/March/2020 and 5:00 pm of 13/April/2020, leading to the storage of 381’337 packages which, after processing and eliminating some invalid data, allowed the creation of a sample with 379’875 packages.

## Provided Datasets
The dataset bundle contains five distinct groups of datasets: the first corresponds to normal house operation, and the remaining four to attack situations that are injected into that original dataset. However, in order to obtain each of the attack scenarios, the attack took place in the real installation, having been collected the messages corresponding to the attack and well as the real system responses to it. Thus, it is guaranteed that all the reactions and behaviors provoked in the system by that same attack are obtained.

The set of messages was collected in PCAP files, the poison files. Then the datasets were built with the injection of them in the original dataset, witch implied some manipulation of timestamps in order to obtain the desired datasets. The datasets are unbalanced as the attacks represent a small number of messages when compared with the total number of messages in the dataset.

The temporal windows used for the different attacks are different, thus allowing their overlapping, which could be useful and will allow the creation of multiclass classifiers.

### Not poisoned dataset
This dataset corresponds to the normal use of the home automation system of a single-family house, inhabited by a couple with two older children for a period slightly longer than two months. Its use will be useful in calibrating situations and operating models devoid of any intrusion or attack.

The following datasets use, as a base, this same dataset in which the poison files are merged in the specified conditions.

### Line scan
The *Line scan* consist of a sequence os messages at the KNX transport layer of type TConnect, that is a connection request to establish a one-to-one. In this attack the requests are sent in sequence to all possible individual addresses (IA) 0 to 255 of a certain line as the destination address (DA). The sender address (SA) was always a valid existing address IA 1.1.102 (a switch located at the piano room, simulating it was hacked for the purpose of this attack). The characteristics were the following ones:
- attack started at 2:30:00 am of 20/March/2020
- attack ended at 2:30:32 am of the same day
- duration of 32 seconds
- 1649 poison messages


Whenever a valid device exists on that destination IA a reply exists with a TDisconnect. Then the initial sender (the attacker) sends a new TConnect and a DeviceDescription request, followed by TACK and DeviceDescription messages from the destination. Finally the attacker sends a TACK and TDisconnect messages ending that conversation.

### Device info
The *Device info* consists of the request of information of two known devices IA 1.1.142 (a light and motion sensor on the leaving room of basement floor) and IA 1.1.136 (the leaving room switch) in a sequence. Again the send was a valid compromised switch with IA 1.1.102. This same attack was inject three times in the original dataset, resulting in a total of 8'820 poison messages, with the following characteristics:

- Situation 1 - Attack started at 1:30:00 am of 16/March/2020 and was repeated 10 times in a row. The attack stopped at 1:39:07 am;
- Situation 2 - Attack started at 5:30:00 am of 27/March/2020 and was repeated 20 times in a row. The attack stopped at 5:48:13 am;
- Situation 3 - Attack started at 2:00:00 pm of 6/April/2020 and was repeated 15 times in a row. The attack stopped at 2:13:40 am;

Each *Device info* request is composed of the attacker sending  TConnect and DeviceDescription messages. The device replies with TACK and DeviceDescription messages. The attacker sends TACK and MemoryRead messages witch are replied TACK and MemoryResponse messages. The attacker sends TACK and ADCRaad replied with TACK and TACK and ADCREsponse. The last two exchanges are repeated some times to gather the device information and finally the attacker ends the conversation sending a last TACK and TDisconnect.

### Message injection
The *Message injection* consists on sending into the bus valid KNX messages with different rates and contents simulating that the attacker has the purpose of disturbing the BACS.

#### Slow rate attack
This *Slow rate attack* uses valid KNX messages but with invalid SA or DA that are injected at random intervals on the bus. Three different datasets are provided with different poison densities: 1\%, 5\% or 10 \% of the total messages in the dataset.  

#### High rate attack - DoS
The *High rate attack* consists of a Denial of Service (DoS) attack where a large number os messages (15'000) are injected into the bus with high density rate (one each millisecond). The used message is a valid KNX message with valid SA and DA (0xBC116E1403E100804A) that is the instruction used to open the dinning room blind using the switch IA 1.1.110.  A message  of type GroupValue\_Write is sent to GA 2/4/3 with value 0x00. The attack starts at 9:55:10 pm of 17/March/2020 and has the duration 15 seconds.

### Invalid context attack

In this example we intend to explore the possibility of a valid message with valid addresses being issued out of context. Pressing a switch in the suite bedroom will turn on the light in the same room. However, it will not be possible for a human to physically access the switch without being flagegd by the presence detector in the suite's vestibule room. Thus, a light activation on behalf of the switch without this presence detection will certainly be a message improperly inserted in the system and out of context.

The motion detector, with IA 1.1.120, each time it detects movement in the suite vestibule, emits a message of type GroupValueWrite to GA 3/0/6 (to turn on the vestibule room light). The switch, with IA 1.1.122, when pressed emits also a GroupValueWrite message to GA 3/0/19, with a value of 0 or 1 (turn off or on the suite bedroom light). And finally the actuator IA 1.1.5 when receiving a message with GA 3/0/9 will turn off or on the suite bedroom light.

The analysis of the original network trace with a relatively short time window will allow, in normal situations, to verify that whenever there is a pressure on the switch, there is also a motion detection. Thus, the existence of switch messages not associated with the existence of movement ones are abnormal and liable to be interpreted as attacks that where injected into the network.

The present dataset was built by injecting messages from the switch at random intervals without any corresponding detection messages. Three datasets were created with different densities of poisonous messages with 1\%, 5\% and 10\% of the total messages in the dataset.

## Dataset Bundle Structure and Files

The dataset bundle is available as a zipped file in which there are five folders:
- Original - The dateset that represents the normal opetation of the BACS, without any attack;
- LS - The files of the Line Scan Attack
- DI - The files corresponding to the device info attack
- MI - The message injection attacks with two folders
	- SR - The slow rate attack
	- HR - The high raate attack
- IC - the invalid context attack


In all situations, the raw capture files will be provided, in PCAP format. The files in CSV format with the data augmentation, which in addition to the enrichment information also contains a classification field name *tagret*. There is a one-to-one relationship between packets in PCAP format and CSV format. When necessary, there will be an additional extension in the filename to indicate the poison density in the file **\_1**, **\_5** or **\_10** (respectively for 1\%, 5\% and 10\%).




