# Apollo
This Repository consists information regarding [Apollo Self Driving car course](https://www.udacity.com/apollo) taught by Udacity.

# Introduction

The Apollo technology framework consists of four layers, 
1) Reference vehicle platform(Base vehicle)
2) Reference hardware platform(Velodyne Lidar, Radar and Camera)
3) Open software platform
4) Cloud service platform

Regarding Reference vehicle Platform and Reference harware platform, 
![](https://github.com/sriharsha0806/Apollo/blob/master/Screenshot%20from%202018-07-17%2017-55-14.png)


The open software layer is divided into three sub layers:
1) RTOS(guarantees task will be completed within a given time)
2) Runtime Framework(Operating Framework of Apollo, Customized version of ROS)
3) layer of application modules

* Apollo RTOS is a combination of Ubuntu Linux operating system and the Apollo Kernel
* In order to adapt ROS to Self-Driving Cars, The Apollo team has improved functionality and performance
for shared memory, decentralization and data comparability.
![](https://github.com/sriharsha0806/Apollo/blob/master/Screenshot%20from%202018-07-17%2018-13-06.png)

* Shared memory reduces the need to copy data if different modules require access. For one to many transmission 
Scenarios shared memory supports the "write once, read multiple" pattern.
![](https://github.com/sriharsha0806/Apollo/blob/master/Screenshot%20from%202018-07-17%2018-17-13.png)

* Decentralization solves the problem of a single point of failure. If the Master Node fails teh entire system will fail.
![](https://github.com/sriharsha0806/Apollo/blob/master/Screenshot%20from%202018-07-17%2018-38-37.png)

To avoid this problem, Bring all the nodes to common domain. Each node in the domain has the information about the other nodes in the domain.
Through this decentralization scheme, the common domain has replaces the original ROS Master node. Thus, the risk of a single point fo failure 
has been eliminated.

* Data Compatability is crucial. Different ROS nondes communicate with each other through ROS Message, and interface language. 
ROS Message requires a common interface language so that each node can interpret the Message data from the other nodes. 
![](https://github.com/sriharsha0806/Apollo/blob/master/Screenshot%20from%202018-07-17%2018-43-44.png)

If the format in the Message file is even slightly different from what a node is expecting, the communication will simply fail. 
To avoid these compatability issues(Eg: When an interface is upgraded, data incompatibility will often result in system failure.
Previous recorded test data must be converted again and again to adapt to new message formats), Protobuf(another interface language) is used.
Protobuf is a method to serialize data and it is used in developing programs to communicate with each other over a wire or for storing data.
We can add new fields to message formats without breaking backwards-compatibility?
`New binaries can accept the old message format during parsing`

* Application Modules
  * MAP Engine
  * Localization
  * Perception
  * Planning
  * Control
  * Human Machine Interface
  * End-to-End driving
Each module has its own algorithm library and the relationships between the modules are complex.

* Apollo cloud services is a suite of applications that runs in teh cloud outside of the vehicle itself. 
The suite provides tools that accelerate the processing of building and training self-driving car software.
The Cloud services contain HD map, simulation, Data Platform, Security, Over the air software updates(OTA) and intelligent voice system called DuerOS.
The simulation software allows everyone to build simulation environments for their own needs. The platform also aggregates large amount of driving data 
which allows developers to verify and validate their self-driving software system. Simulation software not only helps to see the environments but also understand the role situation and the scenario.
The simulation platform has many functions
1) Allows developers to configure different driving scenarios such as obstacles, routing and traffic light states.
Execution mode provides developers a complete set up to run multiple scenarios. In execution mode, developers can upload and verify modules in Apollo environment.

Types of data required by virtual simulator
  * Traffic Lights Data
  * Obstacles with Bounding Boxes
  * Semantic Segmentation

# HD Map(Jingao Wang)
* Consists of a huge amount of driving assistance information. The most important is the accurate 
three-dimensional representation fo the road network. e.g. the layouts of intersections and locations fo signposts. It also contains lot of 
semantic information. The map might report what different colors of traffic light mean, might alos indicate the speed limit of the road and where the left turn lane begins.
The navigation map in phone achieves meter level precision but HD-Map enables the vehicle to achieve centimeter-level precision. 
We have to localize ourselves to the HDmap provided. The vehicle might look for landmarks. This matching process is a complex chain that requires preprocessing, 
Coordinate transformation, and data fusion. Preprocessing eliminates inaccurate or poor quality data. Coordination transformation converts data from different perspectives
into a uniform coordinate system. Data fusion merges data from different vehicles and different types of sensors. The entire localization process depends on the map which is why 
the vehicle needs high definition maps in order to know where it is in the world.

HD map will help the sensor narrow it's detection scope. For instance, the high definition map may tell us to look for a stop sign at a certain location and then our sensors can 
focus on that location to detect the stop sign. This is called a region of interest or ROI. `ROI can help us improve both detection accuracy and speed saving computing resources for other tasks in vehicle.

Not only localization and perception but also planning software relies on HD map. 
HD-Map will often record the precise location and height of traffic lights greatly reducing the difficult of perception. 

![Apollo HD Map](https://github.com/sriharsha0806/Apollo/blob/master/l2c5-outline-en.png)

Standard OpenDRIVE v.s. Apollo OpenDRIVE
-----------------------------------------------------------------------------
|Main Difference	|Standard OpenDRIVE|	Apollo OpenDRIVE|
|----------------|------------------|-----------------|
|Application Scenario|	Primarily used in simulation scenarios	|Primarily applied to real-world self-driving scenes|
|Elemental Form Expression|	Describe lane shape using curve equations and offsets based on reference line|	Describe element shapes using absolute coordinate sequences|
|Elemental Richness|	Provide common element types such as Road, Junction, Signal, and Object|	Refine element expression and enrich element attributes. Such as adding new no-parking areas, crosswalks, deceleration belts, stop lines, parking allowance signs, deceleration allowance signs, etc.|
|Adaptive Driverless Algorithm|	N/A	|Integrate Baidu’s driverless experience to enhance the feasibility and reliability of driverless algorithms.|

The HD map Production in Apollo involves 
1) Data Sourcing
2) Data Processing
3) Object Detection
4) Manual Verification
5) Map Products

In addition to releasing high definition maps, Apollo also publishes corresponding localization maps with 
a top-down view and three-dimensional point cloud maps. 

# Localization

# Percpetion

# Prediction

# Planning

# Control
