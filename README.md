# graph_saliency_map


## Saliency Map

The Saliency Map is used to visualize the elements of the input data that have the greatest influence on the model solution. In the case of graphs, it shows which nodes and edges of the graph are responsible for the discrepancy between the original graph and the reconstructed graph. In our case, we analyze a host connectivity graph that models the networking of ICS infrastructure.

## Project Purpose
1) To demonstrate the benefits of representing network communication as a host connectivity graph. 
2) To prove the expressiveness of the nature of attacks presented in CIC Modbus 2023 dataset using the proposed graph.
3) To visualize what types of attacks are manifested in the interactive maps.

## How to use this expressiveness analysis? 
To understand in which elements of the graph (edges or vertices) the attack manifests itself, it is necessary to compare the graphs that are in the attack folder with the graph of normal behavior (benign). 
 ### How can you interact with the map?
The vertices of the graph are colored in gradations of red and green according to the model's evaluation of these vertices: abnormal and normal. The more saturated a color is, the more it exhibits the properties of its color. For example: a dark red vertex indicates a clear abnormality, a light red vertex indicates suspicious behavior, a bright green vertex would mean a normal action that is often repeated. 
When hovering over a vertex in a floating window, we can see the vertex (device or data stream) parameters. 

Anomalies can appear as in a specific vertex of the graph, indicating atypical fields of the transmitted packet. Example: if you open the file benign/normal_1.html and compare it with the graph in the file attack/scada_Stacked_Modbus_Frames.html, you will see atypical request.quantity and response.quantity values of 20, 125, 2000. Also, the attack may be distributed across multiple nodes at once, which would indicate a violation in the communication topology. When comparing the graphs from the benign folder and the attack/scada_Replay-Complete.html file, you may notice a topology mismatch between the graphs. Replay Attack: An attacker can intercept and resend old packets without waiting for a correct response from the device. The packets may contain old or corrupted data. Normal behavior is characterized by the SCADA (185.175.0.3) polling the same requests to the PLC (host 185.175.0.4 and 185.175.0.8). However, for PLC 185.175.0.5 requests unique, only for this PLC are sent. The attack graph shows that all three PLCs exchange the same packets - topology violation.

The proposed representation allows us to take into account not only the parameters of packets transmitted through the network, but also structural behavioral patterns of interaction of industrial devices: how nodes are connected, how data moves between them and the interaction of data flows in the network.
To prove this, we show an example of a complex attack in the file attack/scada_Payload_injection-Complete.html. This attack is manifested by brute-force on the data manipulation function at address 1. The compromised SCADA in order to find out what type of data is at address 1 requests the READ_HOLDING_REGISTERS, READ_DISCRETE_INPUTS, READ_INPUT_REGISTERS, READ_COILS functions.

An attack by an external attacker is manifested as a new node appearing in the graph with ip 185.175.0.7 (see attack/external_attack.html).

## Where is it used?
The saliency map is widely used in machine learning: in computer vision to analyze images, in natural language processing to extract meaningful words, and in graph neural networks to analyze graphs. The application in GNNs is a special case of its use.

## Why?
It helps to understand which elements of the graph the model considers key in making decisions, which is important for the interpretability of the model.

## How is the saliency map constructed?
The map is constructed by estimating the contribution, i.e. reconstruction error, also called anomalous скор of each data element (node or edge of the graph) to the final model prediction. The greater the impact of an element, the higher its importance in the map.

## What is the benefit?
The saliency map visualizes which elements influence the outcome of the model. This tool can be used in conjunction with a human-readable explanation to highlight to the information security analyst in particular the critical elements of the graph that affect the model outcome and indicate what triggered the alert. This is useful for identifying anomalies, finding errors in the model, and increasing confidence in its solutions.
