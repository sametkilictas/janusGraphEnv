
# janusGraphEnv
This is a dockerized environment For [JanusGraph + ElasticSearch + Cassandra + GraphExp] 
What you will get : 
* Elasticsearch : 6.2.4
* Kibana : 6.2.4
* JanusGraph : 0.2.0
* Cassandra : 2.1
* portainer : latest
* graphexp : latest

## Getting Started : 
To run the stack you need to set the correct permissions to edit the data folders.
### Prerequisites : 
For the elasticsearch container : 
run : `sysctl -w vm.max_map_count=262144` (to know more about it : [Virtual memory Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html#vm-max-map-count)
### Go :
1. To setup the permissions run at **the root  folder of the project :  janusGraphEnv/** : 
`sudo chmod -R 777 dataCassandra/ dataES/ portData/`
2. Then to build & run the stack : 
`docker-compose up`
3. Go to : [http://localhost:9000](http://localhost:9000) & setup for a **local docker** engine.
4. You will need to restart the [janusgraph] container because it search at the startup to connect immediately to cassandra, which is not yet running at the startup of your stack. **Note : you will have to do this each time you restart your entire stack**
5. You are good to go !

If you want to have information to your elasticsearch database content, please download [elastic-head plugin](https://chrome.google.com/webstore/detail/elasticsearch-head/ffmkiejjmecolpfloofpjologoblkegm) and connect to http://localhost:9200
### Connect remotely to your JanusGraph container & run commands:
As you can see there is an apache-tinkerpop client in the project folder, it has the right version to connect to janusgraph and therefore can be used as a remote gremlin-client for the gremlin-server embedded in JanusGraph.
1. To connect to it you will have to edit the file :
`vim apache-tinkerpop/conf/remote.yaml`    
 In the **hosts:[localhost]** please **replace** the localhost by the ip of the janusgraph container (you can find it using portainer > go to the network of your stack > get the ip of the [janusgraph] container)
 2. Run a terminal & execute the bash script on :
 `bash apache-tinkerpop/bin/gremlin.sh`
2. You need to connect to the gremlin-server on the janusgraph container with this command : 
`:remote connect tinkerpop.server conf/remote.yaml session`
3. You need to specify that you want to execute **remote** commands : 
`:remote console`
4. Now you can run commands with your console in remote to the janusgraph container !

### To visualize your Work :
This part is in progress as the graphExp project is currently in progress and need to mature.
Still it is a simple lighweight solution for visualizing janusgraph content in a browser Using D3.js graphic library.
just go to : http://localhost:8183 to access graphExp

If you want to visualize datas by default, you can load the janusGraph god graph by running this command :
graph = JanusGraphFactory.open('/opt/local/janusgraph/conf/gremlin-server/janusgraph-cassandra-es-server.properties')
