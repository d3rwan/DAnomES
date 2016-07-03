Anomaly detection with ElasticSearch
====================================

Anomaly detection with ElasticSearch

### Build and Run

```
  # build images
  docker build --rm -t d3rwan/elasticsearch ./elasticsearch
  docker build --rm -t d3rwan/kibana ./kibana
  docker build --rm -t d3rwan/logstash ./logstash

  # remove images 
  docker stop elasticsearch kibana logstash
  docker rm elasticsearch kibana logstash
  
  # run elasticsearch
  docker run --name elasticsearch -d -p 9200:9200 -p 9300:9300 --volumes-from datastore -v "$PWD/elasticsearch/config":/usr/share/elasticsearch/config d3rwan/elasticsearch

  # run kibana
  docker run --name kibana -d -p 5601:5601 --link=elasticsearch:elasticsearch d3rwan/kibana
  
  # index data
  cd logstash/infogreffe/data
  for i in `ls *.zip` ; do unzip $i; done; 
  docker run --name logstash --link=elasticsearch:elasticsearch -v "$PWD/logstash/infogreffe/data":/data -v "$PWD/logstash/infogreffe/config":/config -ti d3rwan/logstash logstash -f /config/infogreffe.config 

```


### Divers

* Creer un conteneur data
```
  docker create -v /usr/share/elasticsearch/data --name datastore d3rwan/elasticsearch /bin/true
```

* Problème de # de fichiers : 
```
  sudo sysctl -w vm.max_map_count=262144
```

* rafraichir la date des fichiers JSON
```
  for i in `ls logstash/infogreffe/data/*.json` ; do touch $i; done; 
```

* generation des donnees
```
  docker run --name logstash1 --link=elasticsearch:elasticsearch -v "$PWD/logstash/infogreffe/data":/data -v "$PWD/logstash/infogreffe/config":/config -ti d3rwan/logstash /bin/bash 
  logstash -f /config/infogreffe_prepare.config
```
