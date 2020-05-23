# Debugging OpenShift 4.x installation


To be able to successfully debug installation OpenShift 4.x you must:

- Understand how ssh works using private keys
- Understand DNS processing and potentially DHCP
- Understand the OpenShift initialization and bootstrap process

The following describes the OpenShift installation process:

1. Installation preparation - these must be checked prior to installation when you are performing custom User Provisioned Infrastructure. If you are using Installation Provisioned Infrastructure or automated using terraform, a lot of this tasks are automated hence you do not need to check.

    - DNS resolution
    - Load Balancer
    - Installation code and client code
    - Storage for ignition file (object storage, Web server, FTP server)
    - Create ignition files

    (Preparation check)[preparation.md]

2. Cluster initialization

    1. Bootstrap node startup

        - Getting ignition file and boot coreOS
        - Loading release image
        - Initialize bootkube service
        - Loop - waiting for etcd cluster
        - Loop - waiting for critical Kubernetes pods in masters
        - Complete bootkube service

        (Bootstrap check)[bootstrap.md]

    2. Master nodes startup

        - Getting ignition file (from bootstrap) and boot coreOS
        - Startup etcd service
        - Startup kubelet and processing etcd content
        - Initialize machine configuration operator
        - Create cluster operators and initialize cluster

        (Master check)[master.md]

    3. Worker nodes startup

        - Getting ignition file (from master machine config operator) and boot coreOS
        - Startup kubelet and processing etcd content
        - Set up ingress controller and router
        - Finish processing of cluster operators

        (Worker check)[worker.md]
