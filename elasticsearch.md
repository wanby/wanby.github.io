1) create a 14 days free es service from https://found.elastic.co/
  the http link is http://5059dcdbef6203b0053fd62a6d6377d6.ap-northeast-1.aws.found.io:9200
2) use mingw32 (:cn: execute 'git bash') to insert data into es:
  $ curl -XPUT --socks5-hostname 127.0.0.1:1080 'http://5059dcdbef6203b0053fd62a6d6377d6.ap-northeast-1.aws.found.io:9200/dept/employee/1' -d '{"name": "wanby",
 "age": 30}'

3) get data from es by document id:
  $ curl -XGET --socks5-hostname 127.0.0.1:1080 'http://5059dcdbef6203b0053fd62a6d6377d6.ap-northeast-1.aws.found.io:9200/dept/employee/1'
  
  the meta data will be found in the '_source' field.

  get all data:
  $ curl -XGET --socks5-hostname 127.0.0.1:1080 'http://5059dcdbef6203b0053fd62a6d6377d6.ap-northeast-1.aws.found.io:9200/dept/employee/_search'
  
  get data by condition:
  $ curl -XGET --socks5-hostname 127.0.0.1:1080 'http://5059dcdbef6203b0053fd62a6d6377d6.ap-northeast-1.aws.found.io:9200/dept/employee/_search?q=name:beibei'
  
  or
  $ curl -XGET --socks5-hostname 127.0.0.1:1080 'http://5059dcdbef6203b0053fd62a6d6377d6.ap-northeast-1.aws.found.io:9200/dept/employee/_search' -d '{ "query":
{ "match": {"name": "beibei"}}} '

4) web tool
 install es-head tool:
 * plugin -install mobz/elasticsearch-head
 * http://localhost:9200/_plugin/head
