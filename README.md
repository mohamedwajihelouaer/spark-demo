# Introductory Course To Apache Spark for Java

## Environment Setup


## Build and Run 

### Build

- Inside Terminal of VSCode

```bash

# Install dependencies (download all jars to local .m2 cache)
mvn dependency:resolve

# Compile the source code
mvn compile

# Run tests
mvn test

# Package into a JAR (skipping tests)
mvn package -DskipTests

# Package and run tests
mvn package

# Clean previous builds
mvn clean

# Clean + package (most common full build)
mvn clean package -DskipTests

# Install JAR into local .m2 repo
mvn install

```
### Run

```bash
Spark UI
Once your app is running, open your browser at http://localhost:4040 — the port is already forwarded by the devcontainer.json. You'll see the live Spark jobs, stages, and tasks dashboard there.
http://localhost:7077
http://localhost:8080
```

```bash
# start docker
sudo docker service start

# commands to run the application

# Basic spark-submit (local mode, uses all available cores)
spark-submit \
  --class com.jyx.app.SparkRunner \
  --master local[*] \
  --driver-memory 2g \  # fine tune ram
   --executor-memory 2g \
  target/spark-demo-1.0-SNAPSHOT.jar arg1 arg2 # for arguments
```


## RDD

- 