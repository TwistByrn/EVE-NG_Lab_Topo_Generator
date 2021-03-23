# EVE-NG_Lab_Topo_Generator
The idea behind this effort is firstly to quickly generate a lab topology with all the connectivity in place. This helps avoid the clicking and selecting and refreshing of the EVE topology page.

Next serializing the details needed to make the topology should make it a little easier to pull data from an existing production network and hopefully be able to recreate that easily in EVE-NG.

For now, the topology_details.yml will be filled out manually with the routers or switch templates available to you in your EVE environment. 

The pb.gen.topology.yml will create a .unl file under eve_lab_topo folder that can be copied to your /opt/unetlab/labs/ directory on your EVE server.

## Here is an example of the topology_details.yml. For more than 2 nodes simply copy the contents of Node1 or Node2 and increment the number however you see fit.
### net_id will be the network ID that physically connects your virtual routers and switches and should be referenced in the Network section of the topology_details.yml file.

```
---
lab:
  name: Jinja Generated lab
  version: 1
  author: Byrn Baker
  description: This lab topology is being generated with Ansible and Jinja2 templates
  topology:
    - name: R1
      id: 1
      template: vios
      ethernet: 8
      map_left: 714
      map_top: 54
      interfaces:
        - name: Gi0/0
          int_id: 0
          net_id: 12
        - name: Gi0/1
          int_id: 1
          net_id: 17
        - name: Gi0/2
          int_id: 2
          net_id: 14
        - name: Gi0/3
          int_id: 3
          net_id: 111
        - name: Gi0/7
          int_id: 7
          net_id: 1
```

* UUIDs are generated somewhat randomly with the use of the random range function in Jinja combined with the function to convert that number to a UUID.
* The device mac address will simply increment the 3rd octet from the right side by 1 number each time the Jinja template loops.
* The items under the nodes like "Name" and "id" should be unique. 
* "Netwwork_id" is used to reference the network created for the P2P connection of Node1 to Node2 etc. 
* This should match a single network "id" under the network portion. With EVE-NG for each P2P connection a single network bridge is created and hidden on the topology. Therefore the "visibility" item is included as you can create other networks that are mapped to physical interfaces on the EVE Host for example. You would make those visible on the topology.
