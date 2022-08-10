错误整理：
#ERROR Code
ERROR: [1] bootstrap checks failed. You must address the points described in the following [1] lines before starting Elasticsearch.
bootstrap check failure [1] of [1]: max virtual memory areas vm.max_map_count [262114] is too low, increase to at least [262144]
ERROR: Elasticsearch did not exit normally - check the logs at /usr/share/elasticsearch/logs/k8s-logs.log
{"type": "server", "timestamp": "2022-08-09T07:04:16,709Z", "level": "INFO", "component": "o.e.n.Node", "cluster.name": "k8s-logs", "node.name": "es-cluster-1", "message": "stopping ..." }
{"type": "server", "timestamp": "2022-08-09T07:04:16,721Z", "level": "INFO", "component": "o.e.n.Node", "cluster.name": "k8s-logs", "node.name": "es-cluster-1", "message": "stopped" }
{"type": "server", "timestamp": "2022-08-09T07:04:16,721Z", "level": "INFO", "component": "o.e.n.Node", "cluster.name": "k8s-logs", "node.name": "es-cluster-1", "message": "closing ..." }
{"type": "server", "timestamp": "2022-08-09T07:04:16,735Z", "level": "INFO", "component": "o.e.n.Node", "cluster.name": "k8s-logs", "node.name": "es-cluster-1", "message": "closed" }
{"type": "server", "timestamp": "2022-08-09T07:04:16,737Z", "level": "INFO", "component": "o.e.x.m.p.NativeController", "cluster.name": "k8s-logs", "node.name": "es-cluster-1", "message": "Native controller process has stopped - no new native processes can be started" }


{"type": "server", "timestamp": "2022-08-10T02:11:44,149Z", "level": "INFO", "component": "o.e.b.BootstrapChecks", "cluster.name": "k8s-logs", "node.name": "es-cluster-0", "message": "bound or publishing to a non-loopback address, enforcing bootstrap checks" }
ERROR: [1] bootstrap checks failed. You must address the points described in the following [1] lines before starting Elasticsearch.
bootstrap check failure [1] of [1]: initial heap size [536870912] not equal to maximum heap size [1073741824]; this can cause resize pauses
ERROR: Elasticsearch did not exit normally - check the logs at /usr/share/elasticsearch/logs/k8s-logs.log
{"type": "server", "timestamp": "2022-08-10T02:11:44,158Z", "level": "INFO", "component": "o.e.n.Node", "cluster.name": "k8s-logs", "node.name": "es-cluster-0", "message": "stopping ..." }
{"type": "server", "timestamp": "2022-08-10T02:11:44,174Z", "level": "INFO", "component": "o.e.n.Node", "cluster.name": "k8s-logs", "node.name": "es-cluster-0", "message": "stopped" }
{"type": "server", "timestamp": "2022-08-10T02:11:44,174Z", "level": "INFO", "component": "o.e.n.Node", "cluster.name": "k8s-logs", "node.name": "es-cluster-0", "message": "closing ..." }
{"type": "server", "timestamp": "2022-08-10T02:11:44,187Z", "level": "INFO", "component": "o.e.n.Node", "cluster.name": "k8s-logs", "node.name": "es-cluster-0", "message": "closed" }



##################################
解决方案： 这个是由于initcontainer给的vmt太小，修改对应的大小即可


############################################################################
#######Kibana Pod 错误
{"type":"log","@timestamp":"2022-08-10T04:37:00+00:00","tags":["error","savedobjects-service"],"pid":7,"message":"[.kibana_task_manager] Action failed with 'search_phase_execution_exception: '. Retrying attempt 15 in 64 seconds."}
FATAL  Error: Unable to complete saved object migrations for the [.kibana_task_manager] index: Unable to complete the OUTDATED_DOCUMENTS_SEARCH_OPEN_PIT step after 15 attempts, terminating.
###########################################################
解决方案： 
原因是由于ES集群已经有了Kibana的连接信息，需要Delete,重新启动kibana即可
#登录es-cluster的pod执行：
         curl -X DELETE http://localhost:9200/.kibana*

