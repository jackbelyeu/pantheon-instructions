# pantheon-instructions
Describes how to setup and run https://github.com/StanfordSNR/pantheon/ using Oracle VirtualBox

## Install Guide
1. Install Oracle VirtualBox https://www.virtualbox.org/
2. Download Ubuntu 18.04 ISO file https://releases.ubuntu.com/18.04/
3. Using VirtualBox, create a new virtual machine with the ISO image (default and recommended settings are fine). You will create a default profile name and password.
4. (skippable) Once inside your virtual machine, you may find your default profile does not have sudo access. To fix this run ```su - ``` then ```sudo adduser [profile name] sudo```
5. Install Python2.7 ```sudo apt update```
```sudo apt upgrade```
```sudo apt install python2.7``` You may be prompted to instead install Python3. Click cancel and confirm installation.
6. Install Git

## Follow pantheon install process
1. ```git clone https://github.com/StanfordSNR/pantheon.git```
```cd pantheon```
2. ```git submodule update --init --recursive```
3. ```src/experiments/setup.py --install-deps --all``` You can pick which schemes and dependencies you install if you'd like, see full details at pantheon's repository.
4. ```src/experiments/setup.py --setup --all```
5. ```src/experiments/test.py local --all --data-dir [directory name]```

## Test Commands
These are the commands I ran to test congestion control schemes. Run ```src/analysis/analyze.py --data-dir [directory name]``` to generate all reports found in /reports/. The directory must be created with the --data-dir flag when you run the tests to generate the necessary log files. To share files from VirtualBox to your local machine install VirtualBox's Guest Additions and create a Shared Folder in VirtualBox's settings. Check 'Make Permanent' and 'Auto-Mount' but leave 'Read-Only' unchecked. You should see a folder named /media/sf_whateveryounamedit.

Test cubic, fillp, and fillp-sheep under a high bandwidth environment:
```/src/experiments/test.py local --schemes "cubic fillp fillp_sheep" --uplink-trace traces/50mbps.trace --downlink-trace traces/50mbps.trace --runtime 60 --run-times 5 --prepend-mm-cmds "mm-delay 5" --data-dir b_results_delay```

Under a high-latency environment:
```./src/experiments/test.py local --schemes "cubic fillp fillp_sheep" --uplink-trace traces/1mbps.trace --downlink-trace traces/1mbps.trace --runtime 60 --run-times 5 --prepend-mm-cmds "mm-delay 100" --data-dir b2_results_delay```

There are many more flags you can set to test different network conditions. Full documentation can be found on the main pantheon repoistory.

## Other commands
I needed to run ```sudo sysctl -w net.ipv4.ip_forward=1``` on every new launch of the virtual machine. Remember every new launch also requires you run ```src/experiments/setup.py --all```. 
