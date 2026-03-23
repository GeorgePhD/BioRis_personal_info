
## Iaas = Infrastructure as a Service

## Paas = Platform as a Service

## Saas = Software as a Service

- Cluster = a cluster generally refers to a group of interconnected computers (Virtual Machines) that work together as a single system to run applications or process data.

The two most common types of clusters you'll encounter in Google Cloud are:

1. Google Kubernetes Engine (GKE) Cluster
This is the most frequent use of the term "cluster" in GCP. A GKE cluster is a managed environment for deploying, managing, and scaling containerized applications using Kubernetes.

Composition: It consists of at least one control plane (the brains of the operation, managed by Google) and multiple worker machines called nodes (Compute Engine Virtual Machines where your actual applications run).
Use case: Running microservices, web applications, and any software packaged in Docker containers.
2. Dataproc Cluster
Dataproc is Google's managed service for running Apache Hadoop, Spark, Flink, and other open-source data processing frameworks.

Composition: A Dataproc cluster consists of a master node (manages the cluster and jobs) and multiple worker nodes (process the data and store it in HDFS).
Use case: Big data processing, machine learning, and data analytics.
In simple terms: Think of a cluster as a team of computers. Instead of relying on one giant, expensive machine to do a job, you network several smaller, standard machines together so they can share the workload, provide backup if one fails, and easily scale up or down depending on how much computing power you need at that moment.















