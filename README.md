### Deploy Elastic framework (contains Elasticsearch)
CLI

`dcos package install elastic --yes`

GUI
* Set Ingest node quantity to ‘1’
* Do not enable X-Pack on deployment.

### Basic Elastic test

Verify (or attach) cluster context

`dcos cluster list`

`dcos cluster attach <dcos cluster name>`

SSH into the DC/OS Master leader

`dcos node ssh --master-proxy --leader`
  
Identify an endpoint URL:port from the DC/OS UI
* DC/OS UI > Services > Elastic > Endpoints
* e.g. coordinator.elastic.l4lb.thisdcos.directory:9200
  
GET a response from the endpoint

`curl -X GET 'http:///coordinator.elastic.l4lb.thisdcos.directory:9200'`

* Expected response:
{
 "name" : "coordinator-0-node",
 "cluster_name" : "elastic",
 "cluster_uuid" : "O-HGBwVbSWmjX_u1DJUhHw",
 "version" : {
   "number" : "5.6.9",
   "build_hash" : "877a590",
   "build_date" : "2018-04-12T16:25:14.838Z",
   "build_snapshot" : false,
   "lucene_version" : "6.6.1"
 },
 "tagline" : "You Know, for Search"
}

POST a Hello World message to the above endpoint

`curl -XPOST 'http://coordinator.elastic.l4lb.thisdcos.directory:9200/helloworld/1' -d '{ "message": "Hello World!" }'`

* Expected response:
{"_index":"helloworld","_type":"1","_id":"AWZp1soFCShwhBJOBDeJ","_version":1,"result":"created","_shards":{"total":2,"successful":2,"failed":0},"created":true}

GET the above message from the endpoint
`curl -X GET 'http://coordinator.elastic.l4lb.thisdcos.directory:9200/helloworld'`
* Expected response:
"helloworld":{"aliases":{},"mappings":{"1":{"properties":{"message":{"type":"text","fields":{"keyword":{"type":"keyword","ignore_above":256}}}}}},"settings":{"index":{"creation_date":"1539373975433","number_of_shards":"5","number_of_replicas":"1","uuid":"3merMqPkTHeW5oUGxB3eqg","version":{"created":"5060999"},"provided_name":"helloworld"}}}}
