Training: Application Onboarding Process


Session 0

#HSLIDE


Agenda for today

1. Introduction into YAML and TOSCA

2. Create a sample VNF step by step

3. Deploy a VNF to an environment

4. It's your turn: collecting requirements

#HSLIDE


TOSCA & YAML


What is it used for in our context?

#VSLIDE


YAML intro


human-readable data serialization
tree of of elements based on whitespaces
implizit data types
every space counts (we use two-spaces indentation)


#VSLIDE


YAML example

key: value
boolean: true
string: "hello world"
dictionary:
  key: value
array:
  - val1
  - val2


#VSLIDE


TOSCA

Full reading: http://docs.oasis-open.org/tosca/TOSCA-Simple-Profile-YAML/v1.0/csprd01/TOSCA-Simple-Profile-YAML-v1.0-csprd01.html

Required elements for every node type:

properties:
      name:
      version:
      description:


#HSLIDE


VNFD quick start

#VSLIDE


1. Fire up your favorite editor!



#VSLIDE


2. Add the skeleton

Every TOSCA file needs those generic lines.

tosca_definitions_version: TOSCA_dtag_profile_for_nfv_0_0_4

description: VNF Template for the reference tenant.

# Topology Template

topology_template:
  node_templates:


#VSLIDE


3. Add a VNF

First, we are adding a VNF called demo01.

...

topology_template:
  node_templates:
    # VNF: demo01
    demo01:
      type: tosca.dtag.nodes.VNF
      properties:
        name:        demo01
        version:     '1.0'
        description: Reference VNF



#VSLIDE


4. Add a datacenter

A VNF is linked to a datacenter which is created here. Refer to our http://172.16.2.210/application_orchestration/vnfd_engine/blob/development/docs/tosca.md for a description of the properties

...

topology_template:
  node_templates:
    ...
    budapest:
      type: tosca.dtag.nodes.Tenant
      properties:
        name:        budapest
        version:     '1.0'
        description: Reference tenant in Budapest data center
        vnf:         demo01
        datacenter:  Budapest
        ipv6_prefix: '2001:0db8::'
        keys:        'ssh-rsa ...'


#VSLIDE


5. Add an external network

Without an external network, no communication with the outside is possible.

topology_template:
  node_templates:
    ...
    ext:
      type: tosca.dtag.nodes.Network
      properties:
        name:        ext
        version:     '1.0'
        description: External network for Service Chains
        tenant:      ref_bud
        target:      '65412:1'
        ipv4:
          cidr:       192.168.200.0/24
          network:    192.168.200.0
          length:     24
          gateway:    192.168.200.1
          dns:        192.168.200.2
          start:      192.168.200.10
          end:        192.168.200.99


#VSLIDE


6. Add a network

topology_template:
  node_templates:
    ...
    # service network of the virtual network function
    svc:
      type: tosca.dtag.nodes.Network
      properties:
        name:         svc
        version:      '1.0'
        description:  service network of the virtual network function
        target:      '65412:1'
        tenant:       budapest
        ipv4:
          cidr:       185.156.128.64/28


#VSLIDE


7. Add a virtual machine (1)

topology_templates:
  node_templates:
   ...
   web:
     type: tosca.dtag.nodes.InternalComponent
     properties:
       name:         web
       version:      '1.0'
       description:  webserver
       tenant:       budapest
       placement:    EXT
       flavor:       m1.large
       image:        CentOS_7
       volumes:
         - device: /dev/vdb
           size:   20
           type:   ocs-block-disk
       interfaces:
         - name: svc
       user_data: |
         #!/bin/sh
         echo "Hello, World!"


This webserver is linked with


the network svc

the datacenter budapest



#VSLIDE


7. Add a virtual machine (2)

  web:
  ...
  capabilities:
      service1:
        properties:
          name:      https
          interface: svc
          ports:
            - protocol: tcp
              range:    '443'


Capabilities define which further properties an element should have. Here, we enable TCP port 443 for the webserver over the interface svc.

#HSLIDE


GENERATOR

#HSLIDE


Deployment

#VSLIDE


Establish SSH connection

#VSLIDE


Activate Openstack credentials

#VSLIDE


Run Generator to create HOT files

#HSLIDE


Your turn

#VSLIDE


Which requirements for your applications can you think of?