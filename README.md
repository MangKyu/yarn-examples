yarn-beginners-examples
=======================

## Build ##
```
mvn clean install
cp target/yarn-examples-1.0-SNAPSHOT.jar  $HADOOP_HOME/share/hadoop/yarn/lib/
```

## Usage ##
```
cd $HADOOP_HOME
hadoop jar share/hadoop/yarn/lib/yarn-examples-1.0-SNAPSHOT.jar com.wikibooks.hadoop.yarn.examples.MyClient

usage: Client
 -appname <arg>                                 Application Name. Default
                                                value - HelloYarn
 -container_memory <arg>                        Amount of memory in MB to
                                                be requested to run the
                                                shell command
 -container_vcores <arg>                        Amount of virtual cores to
                                                be requested to run the
                                                shell command
 -help                                          Print usage
 -jar <arg>                                     Jar file containing the
                                                application master
 -master_memory <arg>                           Amount of memory in MB to
                                                be requested to run the
                                                application master
 -master_vcores <arg>                           Amount of virtual cores to
                                                be requested to run the
                                                application master
 -num_containers <arg>                          No. of containers on which
                                                the shell command needs to
                                                be executed
 -priority <arg>                                Application Priority.
                                                Default 0
 -queue <arg>                                   RM Queue in which this
                                                application is to be
                                                submitted
 -timeout <arg>                                 Application timeout in
                                                milliseconds
```

## Example ##
```
cd $HADOOP_HOME
$HADOOP_HOME/bin/yarn jar share/hadoop/yarn/lib/yarn-examples-1.0-SNAPSHOT.jar com.wikibooks.hadoop.yarn.examples.MyClient -jar share/hadoop/yarn/lib/yarn-examples-1.0-SNAPSHOT.jar -num_containers=1
```

## Etc ##

If you run the MyClient, you will find a error log in container stderr file as follow

```
INFO impl.AMRMClientAsyncImpl: Interrupted while waiting for queue
java.lang.InterruptedException
	at java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject.reportInterruptAfterWait(AbstractQueuedSynchronizer.java:1961)
	at java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject.await(AbstractQueuedSynchronizer.java:1996)
	at java.util.concurrent.LinkedBlockingQueue.take(LinkedBlockingQueue.java:399)
	at org.apache.hadoop.yarn.client.api.async.impl.AMRMClientAsyncImpl$CallbackHandlerThread.run(AMRMClientAsyncImpl.java:275)
```

Don't worry about it, above log is a unnecessary log which is a bug for hadoop.
See YARN-1022: https://issues.apache.org/jira/browse/YARN-1022

## Task Flow ##
command: hadoop jar yarn-examples.jar com.wikibooks.hadoop.yarn.examples.MyClient -jar yarn-examples.jar -num_containers=1

1. MyClient는 YarnClient를 통해 Container들을 실행시키기 위한 Application의 제출을 요청한다. 이를 위해서는 YARN의 Resource Manager와 Connect 해야한다.
2. 해당 Application을 받은 ResourceManager는 ApplicationId를 반환한다.
3. Client는 Resource Manager로 Application을 제출한다.
4. 이후에 Application Master를 실행하고, 컨테이너들을 생성한다.
5. 생성된 컨테이너들을 구동한다.
