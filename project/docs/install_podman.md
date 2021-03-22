# Installing applications with Podman

## What is Podman
Podman is a _daemonless_, open source tool for running, building and managig __Open Container Initiative (OCI)__ compliant containers similar to _Docker_, but with more security in mind. A container managed by Podman can run _rootless_ instead of _rootfull_ which allows a container to run as any user without root permissions enhancing the security of the system. Podman can also utilize _pods_ in a similar way it is used in _Kuberenetes_ (k8s) which makes complex microservices deployment easier and compatible with _k8s_. Podman has a strong integration to _systemd_ which allows to start and manage containers as _systemd_ services. This is important as _podman-auto-update_ heavily relies on this feature to automatically update containers.

## Before you start
Before you begin, make sure you installed _Podman_ and configure _subgid_ and _subuid_ for your user:

* Install podman with `sudo zypper in podman`,
* Edit 

??? info
    Understanding podman _rootless_ priviliges is fairly simple. 

Include other important information here, such as known issues or bugs.

## Step-by-step guide

### Step 1: Optional: Title for step 
Brief instructions explaining how to interpret the image.

### Step 2: Optional: title for step

### Step 3: Optional: title for step
### Step 4: Optional: title for step

Provide a summary of the steps completed and explain what the user has achieved by following them. You can also include links to related articles that may help the reader reinforce concepts discussed in this How To article.