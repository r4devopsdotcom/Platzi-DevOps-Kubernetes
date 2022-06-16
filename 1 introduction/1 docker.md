# Overview
Containers = namespaces + cgroups + chroot

- namespaces: Views to the OS resources. Isolate the processes in the sandbox
    types:
    - mount: limit the processes access to the directories
    - network: entity where the containers runs. Provide containers with isolated network resources
    - process: manage the process in a container
    - chroot: grant access to the dependencies needed for the container
- cgroups (control groups): limit OS resources
- chroot:

# Architecture
App1        App2        App3
bin/libs    bin/libs    bin/libs
        Hypervisor
        Host OS
        Infrastructure