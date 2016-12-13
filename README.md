# Kinect2_network

## Maintainer

- [Andrés A. Ramírez D.] <<andres.duque@aluno.ufes.br>>, <<aaramirezd@gmail.com>>, [Laboratório de Automação Inteligente] (http://laboratoriodeautomacaointeligente.ufes.br/), Universidade Federal do Espírito Santo

## Table of contents
- [Description](#description)
- [Install](#install)

## Description

Open_PTrack porting to Ubuntu 16.04 and ROS Kinetic, (Swiss Range package obsolete)

Original Package  [Open_PTrack](https://github.com/OpenPTrack/open_ptrack), [Open source people tracking library](http://openptrack.org/)

## Install

1. Install the ROS Kinetic. [Instructions for Ubuntu 16.04](http://wiki.ros.org/kinetic/Installation/Ubuntu)
2. Install [libfreenect2](https://github.com/OpenKinect/libfreenect2):
3. Install [IAI Kinect2](https://github.com/code-iai/iai_kinect2):
4. Install [Ceres Solver](http://ceres-solver.org/installation.html#linux):
5. Install [Calibration Toolkit](https://github.com/iaslab-unipd/calibration_toolkit/tree/indigo_v0.3):
6. Clone this repository into your catkin workspace:
   - git clone https://github.com/aaramirezd/kinect2_network
   - Build opt_msgs (twice): catkin_make --pkg opt_msgs
   - Build all: catkin_make
