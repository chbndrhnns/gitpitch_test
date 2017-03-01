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

