# EVE-NG_Lab_Topo_Generator
The point of this is to help creating a Lab Topology for EVE-NG a little easier than click and dragging nodes around.
At some point down the road, I think this can serve as groundwork to pull live topologies with modules like get_facts on routers and switches to create a lab environment that mirrors a production topology.

For now, the lab_topo_items.yml will be filled out manually with the routers or switches templates available to you in your EVE environment. 

## Here is an example of 2 nodes in a new topology. For more than 2 nodes simply copy the contents of Node1 or Node2 and increment the number however you see fit.

```
---
lab:
  name: Jinja Generated lab
  version: 1
  author: Byrn Baker
  description: This lab topology is being generated with Ansible and Jinja2 templates
  topology:
    node1: {id: 1, name: R1, int_local_id: 0, network_id: 1, type: qemu, template: vios, image: vios-adventerprisek9-m.SPA.156-2.T, console: telnet, cpu: 1, cpulimit: 0, ram: 1024, ethernet: 8 }
    node2: {id: 2, name: R2, int_local_id: 0, network_id: 1, type: qemu, template: vios, image: vios-adventerprisek9-m.SPA.156-2.T, console: telnet, cpu: 1, cpulimit: 0, ram: 1024, ethernet: 8 }
  network:
    net_1: {id: 1, type: bridge, name: "R1_to_R2", visibility: 0, icon: "lan.png" }
 ```
UUIDs are generated somewhat randomly with the use of the random range function in Jinja combined with the function to convert that number to a UUID.

The device mac address will simply increment the 3rd octet from the right side by 1 number each time the Jinja template loops.

The items under the nodes like "Name" and "id" should be unique. 
"Netwwork_id" is used to reference the network created for the P2P connection of Node1 to Node2 etc. 
This should match a single network "id" under the network portion. With EVE-NG for each P2P connection a single network bridge is created and hidden on the topology. Therefore the "visibility" item is included as you can create other networks that are mapped to physical interfaces on the EVE Host for example. You would make those visible on the topology.
