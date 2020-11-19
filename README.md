# elasticsearch-splitbrain-tester
General idea is to build a 5-node (in my case) cluster with Vagrant, provision it with Ansible and use as a spitbrain simulator.

## Usage
First, spin up the cluster:

	git clone https://github.com/apyshonkin/elasticsearch-splitbrain-tester.git
	cd elasticsearch-splitbrain-tester
	vagrant up
	
It should bring up a 5-node ES6 cluster, 4 nodes of which (es1-4) are master-eligible and one works as a coordinating node without any other roles. So you can start the chaos engineering, I used the firewall. Feel free to change the settings, but be aware that some of the variables are set into the Vagrantfile.
