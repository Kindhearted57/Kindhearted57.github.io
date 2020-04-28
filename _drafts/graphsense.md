# Background
Mac catalina, trying to understand graphsense, no experience with java, sbt, etc. As far as I can understand, here are few thing needed for using graphsense(as it is not)
# graphsense
## transformation
### install sbt
`brew install sbt`
### install npm
`brew install node`
### Apache Spark
[Link](https://www.apache.org/dyn/closer.lua/spark/spark-3.0.0-preview2/spark-3.0.0-preview2-bin-hadoop2.7.tgz)
### Apache Cassandra
[Link](https://cassandra.apache.org/download/)
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

