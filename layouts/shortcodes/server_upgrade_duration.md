The Chef Infra Server 14 upgrade process requires downtime for stopping the server, installing the new package, and then upgrading the server, which will include an automatic Elasticsearch reindexing operation for existing Solr users. We estimate the reindexing operation will take 2 minutes for each 1000 nodes, but the it could take more time, depending on your server hardware and the complexity of your Chef data.


{{/* moved to chef-server repo */}}
