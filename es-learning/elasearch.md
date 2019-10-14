####一、elsearch  

***安装***



 集群运行 
  （1）curl -X GET 'localhost:9200/_cat/health?v' 查看集群健康
   绿色Green 一切都很好（集群功能齐全） 
  黄色Yellow 所有数据均可用，但尚未分配一些副本（群集功能齐全）
  红色 red - 某些数据由于某种原因不可用（群集部分功能）
  注意：当群集为红色时，它将继续提供来自可用分片的搜索请求，但您可能需要尽快修复它，因为存在未分配的分片

 curl -X GET 'localhost:9200/_cat/nodes?v'   获取所有的节点

 curl -X GET 'localhost:9200/_cat/indices?v' 列出所有的指数

 curl -X PUT 'localhost:9200/customer?pretty' 创建一个为索引名称为客户的索引
 curl -H "Content-Type: application/json" -X PUT localhost:9200/customer/_doc/1?pretty -d '{"name":"customer的第一 个文档"}' 创建文档

  curl -XGET 'http://localhost:9200/customer/_doc/1?pretty' 获取文档

 
