# elasticsearch-splitbrain-simulator
General idea is to build a 5-node (in my case) ElasticSearch cluster with Vagrant, provision it with Ansible and use as a spitbrain simulator.

## Usage
First, spin up the cluster (this example works for macOS):

	brew cask install virtualbox
	brew cask install vagrant
	
	...(possible reboot for the kernel extensions)...
	
	git clone https://github.com/apyshonkin/elasticsearch-splitbrain-simulator.git
	cd elasticsearch-splitbrain-tester
	vagrant up
	
It should bring up a 5-node ES6 cluster, 4 nodes of which (es1-4) are master-eligible and one works as a coordinating node without any other roles. 
Empty cluster is a sad thing to watch. So it seems like a good idea to fill it up with data:

	git clone https://github.com/oliver006/elasticsearch-test-data.git
	cd elasticsearch-test-data
	pip3 install -r requirements.txt
	for index in $(echo test{1,2,3,4,5,6,7,8,9,10}); do python3 es_test_data.py --es_url=http://192.168.33.71:9200 --index_name=$index --num_of_shards=4 --num_of_replicas=4 --count=100000; done

So you can start the chaos engineering, I used the firewall. Feel free to change the settings, but be aware that some of the variables are set into the Vagrantfile.
