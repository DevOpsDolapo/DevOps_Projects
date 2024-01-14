
## Implementing a DevOps Tooling Website Solution

As a DevOps Engineer, it's important to know how to implement a tooling website solution that can allow easy and seamless access to DevOps tools within a corporate infrastructure.

For this project, our DevOps tooling website will be running on a 3-tier architecture, which is essentially a client-server architecture that separates the user interface (presentation layer), application processing (application layer), and data management (data layer) into three distinct tiers or layers. 

This DevOps tooling website will consist of the following tools:

1. [Jenkins](https://jenkins.io/) - this is an automation server used to build CI/CD pipelines. It is free and open source.

2. [Kubernetes](https://kubernetes.io/) - used for container orchestration. It is useful for automating computer application deployment, scaling, and management. It is also an open source software.

3. [Jfrog Artifactory](https://jfrog.com/artifactory/) - this is a Universal Repository Manager with support for all major packaging formats, build tools, and CI servers.

4. [Rancher](https://www.rancher.com/) - an open source software platform that allows organizations run and manage [Docker](https://www.docker.com/) and Kubernetes during production.

5. [Grafana](https://grafana.com/) - this is a multi-platform analytics and interactive visualisation web application. It is open source.

6. [Prometheus](https://prometheus.io/) - this is an open-source monitoring system that features a dimensional data model, flexible query language, efficient time-series database, and modern alert systems.

7. [Kibana](https://www.elastic.co/kibana) - this software allows users to visualize Elastic Search data and navigate the Elastic Stack.

The tooling components in this project will be running on the following infrastructure:

1. **Infrastructure**: Oracle VM VirtualBox

2. **Web Servers**: Red Hat Enterprise Linux 8

3. **Database Server**: Ubuntu 20.04 + MySQL

4. **Storage (NFS) Server**: Red Hat Enterprise Linux 8

5. **Programming Language**: PHP

6. **Code Repository**: [GitHub](https://github.com/darey-io/tooling)

The diagram below shows a pictorial representation of the 3-tier architecture setup for this project: