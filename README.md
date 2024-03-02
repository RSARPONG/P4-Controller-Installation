# P4-Installation

This tutorial shows how to install p4 and its dependencies from scratch. PI (An implementation framework for a P4Runtime server), p4c (P4_16 reference compiler) and behavior-model (The reference P4 software switch), which are all the components you need to run p4 programs, will be installed. When finishing installation, you can play with p4 tutorial.

The installation is tested on Ubuntu 22.04 LTS. The whole installation may take 2 hours depending on network condition. At least 25 Gbytes of free disk space is needed.

It is recommended to install the parts following the order listed below. The version of the packages and repos must follow the instructions. Any mismatch version may cause unsuccessful installation.

To check latest package and repo versions, visit https://github.com/p4lang/tutorials/blob/master/vm/user-bootstrap.sh and get new versions from the bash file.

# Step1: Install dependencies
install basic dependencies needed for p4.

sudo apt-get install -y cmake g++ git automake libtool libgc-dev bison flex libfl-dev libgmp-dev libboost-dev libboost-iostreams-dev libboost-graph-dev llvm pkg-config python3 python3-scapy python2-minimal python2 dh-python python-is-python3 tcpdump graphviz golang libpcre3-dev libpcre3 curl mininet lsb-release


Also, you can install doxygen (optional)

sudo apt-get install -y texlive-full doxygen


# 2 install dependencies needed for p4 behavior model version 2
