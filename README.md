# EVE-NG_Lab_Topo_Generator
The idea behind this effort is firstly to quickly generate a lab topology with all the connectivity in place. This helps avoid the clicking and selecting and refreshing of the EVE topology page.

For now, the topology_details.yml will be filled out manually with the router or switch templates available to you in your EVE environment. 

The pb.gen.topology.yml will create the .unl file under the ```/eveng_unl_file``` folder. This file can be copied to your ```/opt/unetlab/labs/``` directory on your EVE server.

Below is an example of how to format the ```lab_facts.yaml```. Jinja templates are used to format the UNL file and I have created several templates for common router and switch templates I use in EVENG. These are found in the ```/node_templates``` folder.

## How to use the Ansible Playbook
Works with any ansible version 2.9 and up.
* usage - ```ansible-playbook pb.gen.topology.yml```
* I updated the playbook changed up the YAML file used to create the UNL file. This is a little shorter from a total lines perspective. Also removed some keys that were not needed and instead placed those into the Jinja template because they should be incremented up anyway. This way the incremental increase is done for you.
* A delay count has been added to the templates so that you can simply turn on all nodes in EVENG at the same time and they will start in 30 second increments and hopefully not clog your CPU.
* UUIDs are generated somewhat randomly with the use of the random range function in Jinja combined with the function to convert that number to a UUID.
* The device mac address will simply increment the 3rd octet from the right side by 1 number each time the Jinja template loops.

```
#  attempting to make create the topology_details a bit easier. I want a smaller file to fill in for a large topology
---
#  Fill in the name of the lab as you want it to appear in EVENG
lab_name: TEST LAB
lab_author: Byrn Baker
lab_description: Creating the topology with an ansible playbook
#  This dictates how the nodes will be configured and connected in the lab topology map
topology:
  nodes:
    R1: # name of the node in the topology
      template: vios # EVE template for the node you will provision in the topology
      num_interfaces: 8 # number of interfaces you want on the node
      interface: # each interface you will connect in the topology to another node or network element
        Gi0/0: {int_id: 0, net_id: 12} # int_id is the number of the interface on the node which always starts at zero. net_id should correspond to the ID of the bridge in this example nodes R1:Gi0/0 and R2:Gi0/0 will be connected together and they share the same net_id
        Gi0/1: {int_id: 1, net_id: 17} 
        Gi0/2: {int_id: 2, net_id: 14}
        Gi0/7: {int_id: 7, net_id: 1}
    R2: 
      template: vios
      num_interfaces: 8
      interface:
        Gi0/0: {int_id: 0, net_id: 12}
        Gi0/1: {int_id: 1, net_id: 26}
        Gi0/2: {int_id: 2, net_id: 23}
        Gi0/7: {int_id: 7, net_id: 1}
    R3: 
      template: vios
      num_interfaces: 8
      interface:
        Gi0/0: {int_id: 0, net_id: 34}
        Gi0/1: {int_id: 1, net_id: 23}
        Gi0/2: {int_id: 2, net_id: 37}
        Gi0/3: {int_id: 3, net_id: 35}
        Gi0/4: {int_id: 4, net_id: 355}
        Gi0/7: {int_id: 7, net_id: 1}
    R4: 
      template: vios
      num_interfaces: 8
      interface:
        Gi0/0: {int_id: 0, net_id: 34}
        Gi0/1: {int_id: 1, net_id: 14}
        Gi0/2: {int_id: 2, net_id: 46}
        Gi0/3: {int_id: 3, net_id: 45}
        Gi0/4: {int_id: 4, net_id: 466}
        Gi0/7: {int_id: 7, net_id: 1}
    RR: 
      template: vios
      num_interfaces: 8
      interface:
        Gi0/0: {int_id: 0, net_id: 45}
        Gi0/1: {int_id: 1, net_id: 35}
        Gi0/7: {int_id: 7, net_id: 1}
    R6: 
      template: vios
      num_interfaces: 8
      interface:
        Gi0/0: {int_id: 0, net_id: 67}
        Gi0/1: {int_id: 1, net_id: 26}
        Gi0/2: {int_id: 2, net_id: 46}
        Gi0/7: {int_id: 7, net_id: 1}
    R7: 
      template: vios
      num_interfaces: 8
      interface:
        Gi0/0: {int_id: 0, net_id: 67}
        Gi0/1: {int_id: 1, net_id: 17}
        Gi0/2: {int_id: 2, net_id: 37}
        Gi0/7: {int_id: 7, net_id: 1}
    PE1:
      template: xrv9k
      num_interfaces: 10
      interface:
        Gi0/0/0/0: {int_id: 0, net_id: 355}
        Gi0/0/0/1: {int_id: 1, net_id: 5555}
        Mgmt0/0/CPU0/0: {int_id: 7, net_id: 1}
    PE2-CP:
      template: vmxvcp
      num_interfaces: 12
      interface:
        fxp0: {int_id: 0, net_id: 1}
        int: {int_id: 1, net_id: 1666}
    PE2-FP:
      template: vmxvfp
      num_interfaces: 12
      interface:
        int: {int_id: 1, net_id: 1666}
        ge-0/0/0: {int_id: 2, net_id: 466}
        ge-0/0/1: {int_id: 3, net_id: 6666}    

network:
    MGMT: {net_id: 1, type: pnet4, visibility: 1, icon: "cloud_sm.png" }
    R1_to_R2: {net_id: 12, type: bridge, visibility: 0, icon: "lan.png" } # name of the network element, net_id should match the connection this element facilitates. Notice the number is the same as the above example on R1 and R2.
    R1_to_R7: {net_id: 17, type: bridge, visibility: 0, icon: "lan.png" } # bridge is the type of network element you want to use in the connectivity
    R1_to_R4: {net_id: 14, type: bridge, visibility: 0, icon: "lan.png" } # visibility is set between 0 (off) or 1 (on). This is so that the point to point bridges do not clutter your topology
    R2_to_R3: {net_id: 23, type: bridge, visibility: 0, icon: "lan.png" } # icon is the visible picture of the network element. This can be anything that EVE provides (ex. lan, cloud, pc, etc.) It must have a value.
    R2_to_R6: {net_id: 26, type: bridge, visibility: 0, icon: "lan.png" }
    R3_to_R4: {net_id: 34, type: bridge, visibility: 0, icon: "lan.png" }
    R3_to_R7: {net_id: 37, type: bridge, visibility: 0, icon: "lan.png" }
    R3_to_PE1: {net_id: 355, type: bridge, visibility: 0, icon: "lan.png" }
    R3_to_RR: {net_id: 35, type: bridge, visibility: 0, icon: "lan.png" }
    R4_to_R6: {net_id: 46, type: bridge, visibility: 0, icon: "lan.png" }
    R4_to_RR: {net_id: 45, type: bridge, visibility: 0, icon: "lan.png" }
    R4_to_PE2: {net_id: 466, type: bridge, visibility: 0, icon: "lan.png" }
    R6_to_R7: {net_id: 67, type: bridge, visibility: 0, icon: "lan.png" }
    PE2-CP_to_PE2-FP: { net_id: 1666, type: bridge, visibility: 0, icon: "lan.png"}
```
### How to read and create the network section 
Formating the network this way allowed for a more concise looking format for the bridges that are typically created in most EVE topologies.
* ```network["MGMT"]["net_id"]``` - This references the above ```topology["interfaces"]["net_id"]```. This ensure those nodes are connected to the same bridge.
* ```network["MGMT"]["net_type"]``` - This tells EVE that this is a bridge. EVE has a few selection from its network options like switch and hub. You will notice in my example most of the types are bridge and one is pnet4. The pnet4 references one of the physical interfaces on the EVE host and can be used for outside connectivity if you wish. Just match it with the available PNETs on your host machine.
* ```network["MGMT"]["visibility"]``` - This is used to show the icon of the bridge or network on your topology map. EVE hides the point to point bridges to reduce clutter.
* ```network["MGMT"]["icon"]``` - You can set the icon to anything that is available in your EVE installation. EVE has lots of default images and you just need to reference the correct file name. No path is needed.