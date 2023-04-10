# PMap

a port scanning tool that efficiently discovers the majority of open ports from all 65K ports in the whole network.

PMap uses the correlation of ports to build an open port correlation graph of each network, using a reinforcement learning framework to update the correlation graph based on feedback results and dynamically adjust the order of port scanning.

## Offline Recommendation System

![Offline Recommendation System](offline_recommendation_system.gif)

An offline recommendation system is used to analyse performace of the algorithm. It comsumes port scan logs, uses a small portion of it as ground truth to recommend ports to be scanned for the rest of the addresses.

Source Code: `recommend_offline.cpp`

### How to compile

```bash
# Choose one of the compile parameters below

# Only print statstics
g++ recommend_offline.cpp -o recommend_offline -O3
# Print port recommendations
g++ recommend_offline.cpp -o recommend_offline -D DEBUG
# Print the whole correlation map
g++ recommend_offline.cpp -o recommend_offline -D DEBUG_GRAPH
```

### CLI usage

```bash
# By default PMap recommends 100 ports for each IP with the epsilon of 0.3.
./recommend_offline <Log File to Read>
# or you can define recommend rounds and epsilon yourself.
./recommend_offline <Log File to Read> <Recommend Rounds> <Epsilon>
```

### Log file format

The log files it consumes show  must be in such format in each line: `<IP>*[<OpenPort1>, <OpenPort2>, ...]`

Sample log file:

```
192.168.1.1*[80, 443, 8443]
192.168.1.2*[22, 80, 443]
192.168.1.3*[22]
192.168.1.4*[80]
192.168.1.5*[443]
```