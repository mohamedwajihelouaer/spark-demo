# Base image: Ubuntu LTS (slim)
FROM ubuntu:22.04

# Avoid interactive prompts during package install
ENV DEBIAN_FRONTEND=noninteractive

# ─── System packages ───────────────────────────────────────────────────────────
RUN apt-get update && apt-get install -y \
    openjdk-8-jdk \
    curl \
    wget \
    git \
    maven \
    procps \
    net-tools \
    && rm -rf /var/lib/apt/lists/*

# ─── Java 8 as default ────────────────────────────────────────────────────────
ENV JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
ENV PATH="${JAVA_HOME}/bin:${PATH}"

# Register Java 8 with update-alternatives, then set it as default
RUN update-alternatives --install /usr/bin/java java ${JAVA_HOME}/bin/java 1 \
    && update-alternatives --install /usr/bin/javac javac ${JAVA_HOME}/bin/javac 1 \
    && update-alternatives --set java ${JAVA_HOME}/bin/java \
    && update-alternatives --set javac ${JAVA_HOME}/bin/javac

# ─── Apache Spark ─────────────────────────────────────────────────────────────
# Change SPARK_VERSION to whichever release your project targets
ARG SPARK_VERSION=3.3.4
ARG HADOOP_VERSION=3

ENV SPARK_HOME=/opt/spark
ENV PATH="${SPARK_HOME}/bin:${SPARK_HOME}/sbin:${PATH}"

RUN wget -q \
    "https://archive.apache.org/dist/spark/spark-${SPARK_VERSION}/spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz" \
    -O /tmp/spark.tgz \
    && mkdir -p ${SPARK_HOME} \
    && tar -xzf /tmp/spark.tgz --strip-components=1 -C ${SPARK_HOME} \
    && rm /tmp/spark.tgz

# ─── Spark configuration ──────────────────────────────────────────────────────
RUN cp ${SPARK_HOME}/conf/spark-defaults.conf.template ${SPARK_HOME}/conf/spark-defaults.conf \
    && echo "spark.driver.memory   2g"   >> ${SPARK_HOME}/conf/spark-defaults.conf \
    && echo "spark.executor.memory 2g"   >> ${SPARK_HOME}/conf/spark-defaults.conf \
    && echo "spark.ui.enabled      true" >> ${SPARK_HOME}/conf/spark-defaults.conf

# ─── Non-root dev user ────────────────────────────────────────────────────────
RUN useradd -ms /bin/bash devuser \
    && chown -R devuser:devuser ${SPARK_HOME}

USER devuser
WORKDIR /workspace

# ─── Ports ────────────────────────────────────────────────────────────────────
# 4040 – Spark UI  |  7077 – Spark Master  |  8080 – Master Web UI
EXPOSE 4040 7077 8080

CMD ["bash"]