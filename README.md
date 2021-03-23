# EVE-NG_Lab_Topo_Generator
The idea behind this effort is firstly to quickly generate a lab topology with all the connectivity in place. This helps avoid the clicking and selecting and refreshing of the EVE topology page.

For now, the topology_details.yml will be filled out manually with the router or switch templates available to you in your EVE environment. 

The pb.gen.topology.yml will create the .unl file under the ```/eve_lab_topo``` folder. This file can be copied to your ```/opt/unetlab/labs/``` directory on your EVE server.

Below is an example of how to format the ```topology_details.yml```. Jinja templates are used to format the UNL file and I have created several templates for common router and switch templates I use in EVENG. These are found in the ```/node_templates``` folder.

## How to use the Ansible Playbook
Works with any ansible version 2.9 and up.
* usage - ```ansible-playbook pb.gen.topology.yml```
* name, version, author, description - This is the info that will appear in the EVE gui for the lab
* topology - Everything nested under topology will be the devices that will appear in your lab. I will refer to these nested elements with the follow structure - ```topology["foo"]["bar"]```
* ```topology["name"]``` - This will be the name of the node placed on the topology map.
* ```topology["id"]``` - This numner is the ID given to the device EVE typically starts at 1 and incriments up as you add nodes to the topology
* ```topology["template"]``` - This should coincide with your installed node templates and should match the name used within your EVE installation.
* ```topology["ethernet"]``` - This sets the number of ethernet interfaces on your node. Note that some of the KVMs might require a specific number and you should observe thos corner cases. Ex. XRv9000 and vMX-CP, vMX-FP uses several interfaces for internal communication within the KVM. 
* ```topology["map_left"] topology["map_right"]``` - This will dictate where on the Topology map the node will appear. 
* ```topology["interfaces"]["name"]``` - The name of the interface of the template you are using. It is important to observe the node template naming conventions
* ```topology["interfaces"]["int_id"]``` - This is the id given by eve to each interface. This always starts at zero regardless of the interfaces naming convention.
* ```topology["interfaces"]["net_id"]``` - This refers to the bridge that is created and listed at the bottom of the UNL and the ```topology_details.yml``` file. For each Point to point connection there is an invisible bridge created on the EVE topology map. You can utilize any numbering that you wish. I've been able to use 3 digit numbers with no issues so far. I've chosen to use the node ID numbers to keep the connection straight in this example, but feel free to use whatever suits you. Keep in mind that the bridge number used here will be reused in the network section at the bottom of this example.
## Other things to note
* UUIDs are generated somewhat randomly with the use of the random range function in Jinja combined with the function to convert that number to a UUID.
* The device mac address will simply increment the 3rd octet from the right side by 1 number each time the Jinja template loops.

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

    - name: R2
      id: 2
      template: vios
      ethernet: 8
      map_left: 614
      map_top: 43
      interfaces:
        - name: Gi0/0
          int_id: 0
          net_id: 12
        - name: Gi0/1
          int_id: 1
          net_id: 26
        - name: Gi0/2
          int_id: 2
          net_id: 23
        - name: Gi0/3
          int_id: 3
          net_id: 222
        - name: Gi0/7
          int_id: 7
          net_id: 1

    - name: R3
      id: 3
      template: vios
      ethernet: 8
      map_left: 514
      map_top: 33
      interfaces:
        - name: Gi0/0
          int_id: 0
          net_id: 34
        - name: Gi0/1
          int_id: 1
          net_id: 23
        - name: Gi0/2
          int_id: 2
          net_id: 37
        - name: Gi0/3
          int_id: 3
          net_id: 35
        - name: Gi0/4
          int_id: 4
          net_id: 355 
        - name: Gi0/7
          int_id: 7
          net_id: 1 

    - name: R4
      id: 4
      template: vios
      ethernet: 8
      map_left: 414
      map_top: 23
      interfaces:
        - name: Gi0/0
          int_id: 0
          net_id: 34
        - name: Gi0/1
          int_id: 1
          net_id: 14
        - name: Gi0/2
          int_id: 2
          net_id: 46
        - name: Gi0/3
          int_id: 3
          net_id: 45
        - name: Gi0/4
          int_id: 4
          net_id: 466  
        - name: Gi0/7
          int_id: 7
          net_id: 1

    - name: RR
      id: 5
      template: vios
      ethernet: 8
      map_left: 314
      map_top: 13
      interfaces:
        - name: Gi0/0
          int_id: 0
          net_id: 45
        - name: Gi0/1
          int_id: 1
          net_id: 35
        - name: Gi0/7
          int_id: 7
          net_id: 1    

    - name: R6
      id: 6
      template: vios
      ethernet: 8
      map_left: 214
      map_top: 53
      interfaces:
        - name: Gi0/0
          int_id: 0
          net_id: 67
        - name: Gi0/1
          int_id: 1
          net_id: 26
        - name: Gi0/2
          int_id: 2
          net_id: 46
        - name: Gi0/3
          int_id: 3
          net_id: 644  
        - name: Gi0/7
          int_id: 7
          net_id: 1   

    - name: R7
      id: 7
      template: vios
      ethernet: 8
      map_left: 114
      map_top: 53
      interfaces:
        - name: Gi0/0
          int_id: 0
          net_id: 67
        - name: Gi0/1
          int_id: 1
          net_id: 17
        - name: Gi0/2
          int_id: 2
          net_id: 37
        - name: Gi0/3
          int_id: 3
          net_id: 733  
        - name: Gi0/7
          int_id: 7
          net_id: 1

    - name: PE1
      id: 11
      template: vios
      ethernet: 8
      map_left: 714
      map_top: 53
      interfaces:
        - name: Gi0/0
          int_id: 0
          net_id: 111
        - name: Gi0/1
          int_id: 1
          net_id: 1111 
        - name: Gi0/7
          int_id: 7
          net_id: 1

    - name: PE2
      id: 22
      template: vios
      ethernet: 8
      map_left: 714
      map_top: 53
      interfaces:
        - name: Gi0/0
          int_id: 0
          net_id: 222
        - name: Gi0/1
          int_id: 1
          net_id: 2222 
        - name: Gi0/7
          int_id: 7
          net_id: 1                                
    - name: PE3
      id: 33
      template: vios
      ethernet: 8
      map_left: 714
      map_top: 53
      interfaces:
        - name: Gi0/0
          int_id: 0
          net_id: 733
        - name: Gi0/1
          int_id: 1
          net_id: 3333 
        - name: Gi0/7
          int_id: 7
          net_id: 1
    - name: PE4
      id: 44
      template: vios
      ethernet: 8
      map_left: 714
      map_top: 53
      interfaces:
        - name: Gi0/0
          int_id: 0
          net_id: 644
        - name: Gi0/1
          int_id: 1
          net_id: 4444 
        - name: Gi0/7
          int_id: 7
          net_id: 1   
    - name: PE5
      id: 55
      template: xrv9k
      ethernet: 10
      map_left: 714
      map_top: 53
      interfaces:
        - name: Gi0/0/0/0
          int_id: 0
          net_id: 355
        - name: Gi0/0/0/1
          int_id: 1
          net_id: 5555 
        - name: Mgmt0/0/CPU0/0
          int_id: 7
          net_id: 1
    - name: PE6-CP
      id: 66
      template: vmxvcp
      ethernet: 12
      map_left: 714
      map_top: 53
      interfaces:
        - name: fxp0
          int_id: 0
          net_id: 1       
        - name: int
          int_id: 1
          net_id: 1666  
    - name: PE6-FP
      id: 67
      template: vmxvfp
      ethernet: 12
      map_left: 714
      map_top: 53
      interfaces:
        - name: int
          int_id: 1
          net_id: 1666
        - name: ge-0/0/0
          int_id: 2
          net_id: 466
        - name: ge-0/0/1
          int_id: 3
          net_id: 6666 

  network:
    MGMT: {net_id: 1, type: pnet4, visibility: 1, icon: "cloud_sm.png" }
    R1_to_R2: {net_id: 12, type: bridge, visibility: 0, icon: "lan.png" }
    R1_to_R7: {net_id: 17, type: bridge, visibility: 0, icon: "lan.png" }
    R1_to_R4: {net_id: 14, type: bridge, visibility: 0, icon: "lan.png" }
    R1_to_PE1: {net_id: 111, type: bridge, visibility: 0, icon: "lan.png" }
    R2_to_R3: {net_id: 23, type: bridge, visibility: 0, icon: "lan.png" }
    R2_to_R6: {net_id: 26, type: bridge, visibility: 0, icon: "lan.png" }
    R2_to_PE2: {net_id: 222, type: bridge, visibility: 0, icon: "lan.png" }
    R3_to_R4: {net_id: 34, type: bridge, visibility: 0, icon: "lan.png" }
    R3_to_R7: {net_id: 37, type: bridge, visibility: 0, icon: "lan.png" }
    R3_to_PE5: {net_id: 355, type: bridge, visibility: 0, icon: "lan.png" }
    R3_to_RR: {net_id: 35, type: bridge, visibility: 0, icon: "lan.png" }
    R4_to_R6: {net_id: 46, type: bridge, visibility: 0, icon: "lan.png" }
    R4_to_RR: {net_id: 45, type: bridge, visibility: 0, icon: "lan.png" }
    R4_to_PE6: {net_id: 466, type: bridge, visibility: 0, icon: "lan.png" }
    R6_to_R7: {net_id: 67, type: bridge, visibility: 0, icon: "lan.png" }
    R6_to_PE4: {net_id: 644, type: bridge, visibility: 0, icon: "lan.png" }
    R7_to_PE3: {net_id: 733, type: bridge, visibility: 0, icon: "lan.png" }
    PE6-CP_to_PE-FP: { net_id: 1666, type: bridge, visibility: 0, icon: "lan.png"}
```
### How to read and create the network section 
Formating the network this way allowed for a more concise looking format for the bridges that are typically created in most EVE topologies.
* ```network["MGMT"]["net_id"]``` - This references the above ```topology["interfaces"]["net_id"]```. This ensure those nodes are connected to the same bridge.
* ```network["MGMT"]["net_type"]``` - This tells EVE that this is a bridge. EVE has a few selection from its network options like switch and hub. You will notice in my example most of the types are bridge and one is pnet4. The pnet4 references one of the physical interfaces on the EVE host and can be used for outside connectivity if you wish. Just match it with the available PNETs on your host machine.
* ```network["MGMT"]["visibility"]``` - This is used to show the icon of the bridge or network on your topology map. EVE hides the point to point bridges to reduce clutter.
* ```network["MGMT"]["icon"]``` - You can set the icon to anything that is available in your EVE installation. EVE has lots of default images and you just need to reference the correct file name. No path is needed.