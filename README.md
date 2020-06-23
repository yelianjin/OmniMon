# Testbed Programs

## Install Dependencies：

cmake >= 3.12

iniparser:
```
    git clone http://github.com/ndevilla/iniparser.git
    cd iniparser
    make
    sudo cp lib* /usr/lib/
    sudo cp src/*.h /usr/include
```
zmq: `sudo apt install libczmq-dev -y`

pcap: `sudo apt install libpcap-dev -y`

mtcp:
```
git clone https://github.com/mtcp-stack/mtcp.git
cd mtcp
git submodule init
git submodule update
sudo apt install libnuma-dev libgmp-dev autoconf -y
sudo ifconfig eth0 down  # Suppose one dpdk NIC is eth0
sudo ifconfig eth1 down  # Suppose another one dpdk NIC is eth1
./setup_mtcp_dpdk_env.sh [<path to $RTE_SDK>] 
    - Press [15] to compile x86_64-native-linuxapp-gcc version
    - Press [18] to install igb_uio driver for Intel NICs
    - Press [22] to setup 2048 2MB hugepages
    - Press [24] to register the Ethernet ports
    - Press [35] to quit the tool
sudo ifconfig dpdk0 10.0.0.31/24 up   # Assign ip to the mtcp dpdk0, it is recommended to use 10.0.0.0/24 network segments
sudo ifconfig dpdk1 10.0.1.31/24 up   # Assign ip to the mtcp dpdk1, it is recommended to use 10.0.1.0/24 network segments

export RTE_SDK=`echo $PWD`/dpdk
export RTE_TARGET=x86_64-native-linuxapp-gcc
./configure --with-dpdk-lib=$RTE_SDK/$RTE_TARGET
autoreconf -ivf
make -j 4
```
## Configure and Compile

## Setup Tofino

## Run Controller

## Run End-Hosts

## Result Screenshots

TBD

# MiniNet Programs

# Collective Analysis

This repo implements the collective analysis of OmniMon individually.
The analysis programe works for results generated by either testbed program or Mininet program.

## Dependencies

Install eigen library: [http://eigen.tuxfamily.org/index.php]

## Prepare input files

Each epoch has a directory of input files for the analysis. These files are generated by switches and end-hosts.

The files include:

- device\_ids.txt: specify the ids of all end-hosts and switches
- path.txt: specify the expected path along which each flow travels
- src\_X: output file of each source end-host whose id is X. This file containing all flows sent from end-host X.
- dst\_X\_Y: output file of each destination end-host. This file includes flow values that are sent from end-host X and received by end-host Y.
- sX.txt: output of switch X.

We provide a simple sample in directory [collective\_analysis/sample].

## Compile and Run

1. Enter collective analysis directory:
```
cd collective_analysis
```

2. Compile
```
make
```

3. Run
```
./main [dir]
```
Here, dir is the input directory.

## Example

In our sample example, some packets of a flow are dropped by Switch 3. The result is:

