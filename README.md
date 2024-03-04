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

git clone https://github.com/p4lang/behavioral-model.git
cd behavioral-model
./install_deps.sh
cd ..

# install protobuf

sudo pip install protobuf==3.2.0

git clone https://github.com/protocolbuffers/protobuf.git
cd protobuf

sudo apt  install protobuf-compiler 

git checkout v3.2.0 OR (git checkout ${PROTOBUF_COMMIT})
export CFLAGS="-Os"
export CXXFLAGS="-Os"
export LDFLAGS="-Wl,-s"
./autogen.sh
./configure --prefix=/usr
make
sudo make install
sudo ldconfig
unset CFLAGS CXXFLAGS LDFLAGS
cd ..

protoc --version
> libprotoc 3.12.4


## Controller Installation (DOCKER - ONOS)##


*********************************************************************************************
# Uninstalling Any Conflicting Docker (Unofficial Setup)
*********************************************************************************************
sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd

*********************************************************************************************
# Installing Official Docker
*********************************************************************************************
Before you install Docker Engine for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.

Set up Docker's apt repository.

# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

To install the latest version, run:
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

Verify that the Docker Engine installation is successful by running the hello-world image.

sudo docker run hello-world


*********************************************************************************************
# ONOS Controller
*********************************************************************************************
sudo apt-get update   ========>  (Downloads the packages lists from the repositories)
sudo apt-get -y install docker.io   =========> (Install the docker.io package)
sudo ln -sf /usr/bin/docker.io /usr/local/bin/docker   =====> (Creates a symlink from where the docker io package files where installed to the directory /usr/local/bin/docker, so a linux user can run the docker cli by just running docker)


*********************************************************************************************
# Run Docker image
*********************************************************************************************
sudo docker pull onosproject/onos ========> (Download the onos image using).
sudo docker run -t -d -p 8181:8181 -p 8101:8101 -p 5005:5005 -p 830:830 --name onos onosproject/onos  ==============> (Run a single instance of ONOS)

            The previous command is configured with the following options:
            
            -t will allocate a pseudo-tty to the container
            -d will run the container in foreground
            -p <CONTAINER_PORT>:<HOST_PORT> Publish a CONTAINER_PORT to a HOST_PORT. Some of the ports that ONOS uses:
            8181 for REST API and GUI
            8101 to access the ONOS CLI
            9876 for intra-cluster communication (communication between target machines)
            6653 for OpenFlow
            6640 for OVSDB
            830 for NETCONF
            5005 for debugging, a java debugger can be attached to this port
So with the previous command we are publishing the ONOS CLI, GUI, NETCONF, and Debugger ports.

sudo docker ps  ============> (Ensure that the container is up and running) you should see something like:

![image](https://github.com/RSARPONG/P4-Controller-Installation/assets/36456236/935398f8-2553-41a9-90a3-ff73649be768)

After running the container the Onos UI should be accessible through the following URL (http://localhost:8181/onos/ui) from mininet. 
wget -O - http://localhost:8181/onos/ui > /dev/null   ============> (We can check that ONOS is running and that the UI is accessible), you should see something like:

![image](https://github.com/RSARPONG/P4-Controller-Installation/assets/36456236/99bb8742-eeef-40e2-88fd-725d72a231bb)

However this is from the mininet terminal to see it on our browser we should access with mininet's IP, to know which IP is configured run =====> ip addr | grep (Identify the ethernet port === eth0)

![image](https://github.com/RSARPONG/P4-Controller-Installation/assets/36456236/fd13ccd2-37b9-4336-96d0-e8fef3a18337)



