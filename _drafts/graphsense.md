# Background
Mac catalina, trying to understand graphsense, no experience with java, sbt, etc. As far as I can understand, here are few thing needed for using graphsense(as it is not)
# Graphsense
## Blocksci
[Apache prepare](https://www.datageekinme.com/general/setup/setting-up-my-mac-cassandra/),👈follow guidance.
## Transformation
### Prequisites
#### install sbt
`brew install sbt`
#### install npm
`brew install node`

#### Hadoop
Followed steps here 👉[reference](https://www.cnblogs.com/bigdataZJ/p/hellospark.html)

However, I chose to download 3.2.1 from brew install, which later shows not a good idea.

Some of the error I have seen:
`WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable` 
This means there are something wrong with the enviornment setup. In my case I missed `HADOOP_OPTS`

`WARNING: log4j.properties is not found. HADOOP_CONF_DIR may be incomplete.
ERROR: Invalid HADOOP_COMMON_HOME`

This means I missed `HADOOP_COMMON_HOME` in `hadoop-env.sh`

PS: [use](https://www.jianshu.com/p/34521593f30e) http://localhost:9870 instead of http://localhost:50070/ for hadoop3

I can not access http://localhost:8088, some of the solutions from internet says I can try edit `yarn-site.xml` with:

```
<configuration>
 
<!-- Site specific YARN configuration properties -->

        <property>
                <name>yarn.nodemanager.aux-services</name>
                <value>mapreduce_shuffle</value>
        </property>
        <property>
                <name>yarn.resourcemanager.address</name>
                <value>0.0.0.0:8032</value>
         </property>
         <property>
                <name>yarn.resourcemanager.scheduler.address</name>
                <value>0.0.0.0:8030</value>
         </property>
         <property>
                <name>yarn.resourcemanager.resource-tracker.address</name>
                <value>0.0.0.0:8031</value>
         </property>
         <property>
                <name>yarn.resourcemanager.admin.address</name>
                <value>0.0.0.0:8033</value>
         </property>
         <property>
                <name>yarn.resourcemanager.webapp.address</name>
                <value>0.0.0.0:8088</value>
         </property>
</configuration>

```
But this didn't help with this problem. Then I followed the [official guidance](https://hadoop.apache.org/docs/r3.0.3/hadoop-project-dist/hadoop-common/SingleCluster.html#Execution), still won't work. By this time I found out that `hadoop version` don't give any response in return. However, if `sudo hadoop version` will return some error message, at last I deal with the situation exactly same as [this one](https://stackoverflow.com/questions/51808588/run-hadoop-in-the-mac-os)
``
So there is two problems I am dealing with :

* Hadoop3 is different from hadoop2, which means the tutorial will not be compatable. Besides, hadoop3 is not very compatible with Spark, with will later be used. 
[There is a tutorial to download hadoop3 on mac](https://blog.petehouston.com/complete-guide-to-install-and-configure-apache-hadoop-3-on-macos/)

* I need to use `root` from the start in order to avoid some problems with ssh.

I swithed to hadoop2 with the hope of making things easier.([Here](https://medium.com/@petehouston/complete-apache-hadoop-troubleshooting-660122eac6a5) is one troubleshooting that might be helpful sometime)
#### Apache Spark

#### Apache Cassandra
[Link](https://cassandra.apache.org/download/)

[Unable to find java executable](https://stackoverflow.com/questions/32545746/cassandra-unable-to-find-java-executable)
Change to the directory of the cassandra file and:
`$SPARK_HOME/sbin/start-master.sh`
### Ingest raw block data
## REST
### create python environment
(make sure python version >3.7, and you are in the directory of REST downloaded from github.
error:`Unable to symlink '/usr/local/bin/python3' to '/Users/jiru/venv/bin/python3'`
This error shows up when you are in the wrong directory.)
#### create a python environment
`python3 -m venv venv`
#### activate environment
`. venv/bin/activate`
#### install requirements
`pip3 install -r requirements.txt`
#### export REST interface environment variables
`export FLASK_APP=gsrest`
`export FLASK_ENV=development`
## Dashboard
### development setup
`npm install`
`npm start`
## Clustering
### prerequisites
`sbt about`

`sbt test`
`sbt package`
## sbt learning
`test`

`[error] (Test / compileIncremental) java.io.IOError: java.lang.RuntimeException: /packages cannot be represented as URI`
[sbt](https://www.scala-sbt.org/download.html?_ga=2.163737023.1269010484.1587879876-1173452745.1587879876)
![](https://note.youdao.com/yws/api/personal/file/WEBa46f2946c555c4be516329a2b36fe26b?method=download&shareKey=25fa7a6a4b5e390f6773a4b8365ab7fb)
viewed some of the answers to this problem and suggest change the version of scala.
Re-download sbt:
### Java version
[change JDK version](https://xobo.org/macos-switch-default-java-jdk/)
install `jdk8`
### Install sbt using macport

