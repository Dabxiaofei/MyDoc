1、添加索引器

```sh
hbase-indexer add-indexer --name HDR_OUT_VISIT --indexer-conf /usr/local/freeSearch/hbase_indexer_files/HDR_OUT_VISIT.xml --connection-param solr.zk=hadoop02:2181,hadoop03:2181,hadoop04:2181/solr  --connection-param solr.collection=HDR_OUT_VISIT  --zookeeper hadoop02:2181,hadoop03:2181,hadoop04:2181
```

2、查看实时索引器


```sh
hbase-indexer list-indexers
```

3、删除实时索引

```sh
hbase-indexer delete-indexer -n HDR_ALLERGY --zookeeper  hadoop02:2181,hadoop03:2181,hadoop04:2181
```