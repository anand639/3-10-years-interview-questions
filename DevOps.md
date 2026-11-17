## Basic DevOps Concepts

1. **What is DevOps, and why is it important?**
   - DevOps is a set of practices, cultural principles, and tools that bring software development and IT operations closer together. Its goal is to deliver reliable software more frequently through automation, shared ownership, continuous feedback, and measurable improvement. DevOps is not a single tool or job title; it is a way of improving the entire software delivery lifecycle.

2. **What are the main principles of DevOps?**
   - Common principles include collaboration between development and operations, automation of repeatable work, continuous integration and delivery, infrastructure as code, monitoring and feedback, shared responsibility for reliability, and continuous learning. Teams often use the CALMS model: Culture, Automation, Lean, Measurement, and Sharing.

3. **What is the difference between Agile and DevOps?**
   - Agile focuses on iterative software development, customer feedback, and adapting to change. DevOps extends collaboration and automation across development, testing, release, deployment, and operations. Agile helps teams build the right software incrementally; DevOps helps them deliver and operate it reliably and frequently. They complement rather than replace each other.

4. **What is the difference between Continuous Integration, Continuous Delivery, and Continuous Deployment?**
   - **Continuous Integration (CI)** means developers frequently merge changes into a shared branch and automatically build and test them.
   - **Continuous Delivery** keeps validated changes in a deployable state, with production release typically requiring an approval or business decision.
   - **Continuous Deployment** automatically releases every change that passes the required pipeline checks to production.

5. **What is a DevOps lifecycle?**
   - A typical lifecycle includes plan, code, build, test, release, deploy, operate, and monitor. Feedback from monitoring and users flows back into planning. These stages are not always strictly sequential; modern teams use short feedback loops and automate checks throughout the lifecycle.

6. **What is a CI/CD pipeline?**
   - A CI/CD pipeline is an automated workflow that validates, packages, and delivers software. Typical stages include source checkout, dependency installation, static analysis, unit and integration tests, artifact creation, security scanning, deployment to a test environment, approval where required, and production deployment. A good pipeline is repeatable, observable, secure, and fast enough to encourage frequent use.

7. **What is Infrastructure as Code (IaC)?**
   - Infrastructure as Code means defining and managing infrastructure through version-controlled, machine-readable configuration instead of relying on manual console operations. IaC makes environments repeatable, reviewable, testable, and easier to recreate. Tools include Terraform, OpenTofu, AWS CloudFormation, and Azure Bicep.

8. **What is configuration management, and how is it different from provisioning?**
   - Provisioning creates infrastructure resources such as virtual machines, networks, and databases. Configuration management installs packages and applies desired settings to systems that already exist. Terraform is commonly used for provisioning, while Ansible, Chef, and Puppet are commonly used for configuration management, although tool capabilities can overlap.

9. **What is the difference between DevOps and Site Reliability Engineering (SRE)?**
   - DevOps is a broad set of cultural and engineering practices for improving software delivery and operations. SRE is an engineering discipline that applies software engineering to operational problems, often using service-level indicators, service-level objectives, error budgets, automation, and incident management. SRE is one way to implement DevOps principles.

10. **Which DevOps metrics would you track to understand software delivery performance?**
    - Common metrics include deployment frequency, lead time for changes, change failure rate, and time to restore service. These are the four DORA delivery metrics. Teams should also track build duration, deployment success, availability, latency, error rate, infrastructure cost, and security findings. Metrics should drive improvement rather than become targets that encourage gaming.

## Linux and Shell Scripting

11. **Which Linux commands do you commonly use for DevOps work?**
    - Frequently used commands include `ls`, `cd`, `pwd`, `cat`, `less`, `grep`, `find`, `awk`, `sed`, `cut`, `sort`, `uniq`, `tar`, `curl`, `wget`, `df`, `du`, `free`, `ps`, `top`, `ss`, `systemctl`, `journalctl`, `chmod`, and `chown`. A DevOps engineer should understand command options, exit codes, pipes, redirection, and how to combine tools safely.

12. **How do you check CPU, memory, disk, and process utilization on a Linux server?**
    - Use `top` or `htop` for live process and resource information, `free -h` for memory, `df -h` for filesystem capacity, `du -sh <path>` for directory usage, `uptime` for load averages, and `ps` for process details. For deeper analysis, inspect `/proc`, use `iostat`, `vmstat`, or `sar` when installed, and correlate resource metrics with application logs.

    ```bash
    uptime
    free -h
    df -h
    du -sh /var/log
    ps aux --sort=-%cpu | head
    ```

13. **How do you find and troubleshoot a process consuming high CPU or memory?**
    - Start with `top`, `htop`, or `ps` to identify the process ID and resource usage. Inspect the process command line with `ps -fp <pid>`, review its logs, and check whether load is sustained or temporary. Before terminating a process, understand its role and impact. Use `kill <pid>` for a graceful termination signal and `kill -9 <pid>` only as a last resort because it prevents normal cleanup.

14. **What are Linux file permissions, and how do `chmod`, `chown`, and `umask` work?**
    - Linux permissions define read, write, and execute access for the owner, group, and others. `chmod` changes permission bits, `chown` changes ownership, and `umask` determines which permission bits are removed from newly created files and directories. Numeric permissions use octal values: read=4, write=2, execute=1.

    ```bash
    chmod 640 app.conf
    chmod 750 deploy.sh
    chown appuser:appgroup app.conf
    umask 027
    ```

15. **What is the difference between a hard link and a symbolic link?**
    - A hard link is another directory entry pointing to the same inode as the original file. It generally cannot cross filesystem boundaries and usually cannot be created for directories by ordinary users. A symbolic link is a separate file containing a path to another file or directory; it can cross filesystems and can become a broken link if its target is removed.

    ```bash
    ln original.txt hardlink.txt
    ln -s /opt/app/current app-link
    ```

16. **How do you investigate Linux system and service logs?**
    - Use `journalctl` for systemd journal entries and inspect application log files under configured locations such as `/var/log`. Useful options include filtering by unit, time, priority, and following new entries. Check log rotation and timestamps, and avoid exposing secrets or personal data in logs.

    ```bash
    journalctl -u nginx --since "1 hour ago"
    journalctl -p err -b
    tail -f /var/log/nginx/error.log
    ```

17. **How do you manage services in a systemd-based Linux distribution?**
    - Use `systemctl` to start, stop, restart, enable, disable, and inspect services. `enable` configures a service to start at boot; it does not necessarily start it immediately. Use `systemctl status` and `journalctl -u` to investigate failures.

    ```bash
    sudo systemctl status nginx
    sudo systemctl restart nginx
    sudo systemctl enable nginx
    sudo systemctl daemon-reload
    ```

18. **What is the difference between a Linux process and a daemon?**
    - A process is a running instance of a program. A daemon is a background service process that is not directly attached to an interactive terminal and commonly provides a system or network service. A daemon may be managed by systemd or another service manager. Not every background process is necessarily a daemon.

19. **How do you schedule recurring tasks in Linux?**
    - `cron` runs commands on a recurring schedule using crontab entries. The five time fields represent minute, hour, day of month, month, and day of week. For one-time scheduled jobs, use `at` where available. Production jobs should log output, handle failures, use absolute paths, and prevent overlapping executions when necessary.

    ```cron
    # Run a backup every day at 2:30 AM
    30 2 * * * /usr/local/bin/backup.sh >> /var/log/backup.log 2>&1
    ```

20. **How do you write a reliable Bash script for automation?**
    - Use a clear shebang, quote variables, validate inputs, check command exit statuses, log meaningful messages, and clean up temporary resources. `set -euo pipefail` can help expose common failures, but its behavior must be understood and critical commands should still be checked explicitly. Avoid embedding credentials in scripts.

    ```bash
    #!/usr/bin/env bash
    set -euo pipefail

    if [[ $# -ne 1 ]]; then
      echo "Usage: $0 <environment>" >&2
      exit 2
    fi

    environment="$1"
    echo "Deploying to ${environment}"
    ```

## Networking and Troubleshooting

21. **Explain the OSI model and its seven layers.**
    - The OSI model is a conceptual framework for understanding network communication: Physical, Data Link, Network, Transport, Session, Presentation, and Application. In troubleshooting, it helps isolate where a problem occurs, from physical connectivity and IP routing through TCP/UDP transport to application protocols such as HTTP. Real-world TCP/IP networking commonly groups these layers differently.

22. **What is the difference between TCP and UDP?**
    - TCP is connection-oriented and provides ordered, reliable byte-stream delivery with retransmission and flow/congestion control. UDP is connectionless and sends independent datagrams without guaranteeing delivery, ordering, or duplicate protection. HTTP/1.1 and HTTP/2 commonly use TCP; DNS often uses UDP, while QUIC/HTTP/3 uses UDP with reliability and security implemented above it.

23. **What is DNS, and how does DNS resolution work?**
    - DNS translates domain names into records such as IP addresses. A resolver checks its cache and, when needed, queries DNS infrastructure through recursive and authoritative servers. Relevant record types include A, AAAA, CNAME, MX, TXT, and NS. DNS caching and TTLs influence how quickly changes become visible.

24. **What is the difference between a public IP and a private IP?**
    - A public IP is globally routable on the Internet, subject to network policy. Private IPv4 ranges are reserved for internal networks and are not globally routed: `10.0.0.0/8`, `172.16.0.0/12`, and `192.168.0.0/16`. Private-addressed workloads usually need NAT, a proxy, VPN, or another gateway to communicate externally.

25. **What is a subnet and how does CIDR notation work?**
    - A subnet is a logical division of an IP network. CIDR notation such as `10.0.1.0/24` specifies the network prefix length; `/24` means the first 24 bits identify the network. Subnetting helps allocate address space, isolate workloads, and control routing. Cloud networks also reserve some addresses for platform functions.

26. **What is the difference between a load balancer and a reverse proxy?**
    - A load balancer distributes incoming traffic across multiple backends to improve availability and scalability. A reverse proxy accepts client requests and forwards them to upstream services; it may also handle TLS termination, routing, compression, caching, and security policies. A single product can perform both roles.

27. **How do you troubleshoot a server that is reachable by IP but not by hostname?**
    - Check DNS resolution with `dig`, `nslookup`, or `getent hosts`, verify the configured resolver and search domains, inspect the DNS record and TTL, and compare results from different resolvers. Also check `/etc/hosts`, split-horizon DNS, VPN connectivity, and whether the hostname resolves to the intended address.

    ```bash
    dig app.example.com
    nslookup app.example.com
    getent hosts app.example.com
    ```

28. **How do you troubleshoot a service that is not reachable on a particular port?**
    - Verify that the application is running and listening on the expected interface and port using `ss -lntp`. Test locally with `curl` or `nc`, then check host firewalls, cloud security groups, network ACLs, routes, load balancer health checks, and application binding configuration. A successful local test does not prove that remote routing and firewall rules are correct.

    ```bash
    ss -lntp
    curl -v http://127.0.0.1:8080/health
    nc -vz app.example.com 8080
    ```

## Git and Version Control

29. **What is Git, and how is Git different from GitHub or GitLab?**
    - Git is a distributed version-control system that tracks changes to files and supports branching and merging. GitHub and GitLab are platforms that host Git repositories and add collaboration features such as pull requests or merge requests, code review, issue tracking, and CI/CD automation. Git can be used without either hosting platform.

30. **What is the difference between `git fetch` and `git pull`?**
    - `git fetch` downloads remote commits and references without merging them into the current branch. `git pull` performs a fetch and then integrates the selected remote changes, usually through merge or rebase depending on configuration and options. Fetch first when you want to inspect incoming changes before integrating them.

31. **What is the difference between `git merge` and `git rebase`?**
    - `git merge` combines histories and may create a merge commit, preserving the branch structure. `git rebase` reapplies commits on top of another base, creating new commit IDs and a more linear history. Avoid rebasing commits that others already depend on unless the team explicitly coordinates it.

32. **What is a Git branching strategy?**
    - A branching strategy defines how teams organize changes and releases. Common approaches include trunk-based development, short-lived feature branches, release branches, and GitFlow. Trunk-based development encourages small changes merged frequently behind automated checks or feature flags. Choose a strategy that matches release cadence and team coordination needs.

33. **How do you resolve a Git merge conflict?**
    - Git marks conflicting file sections when it cannot automatically combine changes. Review the base and both versions, edit the file to the intended result, run tests, then stage and complete the merge or rebase. Do not simply accept one side without understanding the changes.

    ```bash
    git status
    # Edit the conflicted files and remove conflict markers.
    git add path/to/file
    git merge --continue   # or git rebase --continue
    ```

34. **What is the purpose of `.gitignore`, and should secrets be stored in Git?**
    - `.gitignore` tells Git which untracked files or patterns to ignore, such as build output, local environment files, and temporary files. It does not remove files already tracked. Never commit passwords, API keys, private keys, or production secrets. If a secret is committed, revoke or rotate it immediately; removing it in a later commit does not erase it from repository history.

## CI/CD and Release Engineering

35. **Which tools are commonly used to build CI/CD pipelines?**
    - Common tools include Jenkins, GitHub Actions, GitLab CI/CD, Azure Pipelines, CircleCI, and cloud-native build services. They automate builds, tests, security checks, artifact publishing, and deployment. Tool choice should consider repository platform, hosting environment, security controls, scale, maintainability, and team expertise.

36. **How does a Jenkins pipeline work?**
    - A Jenkins pipeline defines automation as code, commonly in a `Jenkinsfile`. A pipeline contains stages and steps that execute on agents. Declarative and scripted pipeline syntaxes are available. A typical flow checks out code, builds, tests, archives artifacts, and deploys them. Credentials should be stored in Jenkins credentials facilities rather than hardcoded in the file.

    ```groovy
    pipeline {
      agent any
      stages {
        stage('Build') {
          steps { sh 'make build' }
        }
        stage('Test') {
          steps { sh 'make test' }
        }
      }
    }
    ```

37. **What is a Jenkins agent, and why do teams use multiple agents?**
    - A Jenkins controller coordinates jobs, while agents execute pipeline work. Separate agents can provide isolated build environments, scale parallel workloads, and support different operating systems or toolchains. Use ephemeral agents where possible, limit controller executors for build workloads, and apply least-privilege credentials and network access.

38. **How do GitHub Actions workflows work?**
    - GitHub Actions workflows are YAML files stored under `.github/workflows`. Events such as pushes, pull requests, schedules, or manual dispatch can trigger jobs. Jobs contain steps and run on hosted or self-hosted runners. Workflows can use actions, shell commands, artifacts, environments, and protected deployment approvals.

    ```yaml
    name: CI
    on: [push, pull_request]
    jobs:
      test:
        runs-on: ubuntu-latest
        steps:
          - uses: actions/checkout@v4
          - uses: actions/setup-python@v5
            with:
              python-version: '3.12'
          - run: python -m pip install -r requirements.txt
          - run: pytest
    ```

39. **What is a build artifact, and why should the same artifact move between environments?**
    - A build artifact is a versioned output of the build process, such as a container image, package, binary, or static asset. Promoting the same immutable artifact from test to staging to production reduces environment-specific differences and makes releases traceable. Rebuilding separately for each environment can introduce untested differences.

40. **How do you manage secrets in a CI/CD pipeline?**
    - Store secrets in a dedicated secrets manager or the CI platform's protected secret store. Restrict access by repository, environment, branch, and workflow; prefer short-lived identity federation over long-lived static keys when supported. Mask values in logs, avoid exposing secrets to untrusted pull-request code, rotate credentials, and audit access.

41. **What is a deployment strategy? Explain rolling, blue-green, and canary deployments.**
    - **Rolling deployment** gradually replaces old instances with new ones.
    - **Blue-green deployment** maintains separate old and new environments and switches traffic after validation.
    - **Canary deployment** sends a small portion of traffic to the new version, then increases it based on health and business metrics.
    - Each strategy has different capacity, cost, rollback, and state-compatibility trade-offs.

42. **What is a rollback, and how do you design a safe rollback process?**
    - A rollback restores a previously known-good application or configuration after a failed release. Keep immutable versioned artifacts, automate deployment history, define health checks and rollback thresholds, and test the process. Database changes require special care: use backward-compatible expand-and-contract migrations because code rollback may not reverse a schema change safely.

43. **What is the difference between an environment variable and a configuration file?**
    - Environment variables provide configuration through the process environment and are convenient for containerized workloads and deployment platforms. Configuration files support structured settings and can be version-controlled when they contain no secrets. Both need validation, clear precedence rules, secure handling of sensitive values, and environment-specific management.

44. **What are quality gates in a CI/CD pipeline?**
    - Quality gates are conditions that must pass before code or an artifact can progress. Examples include unit and integration tests, code coverage thresholds, linting, static analysis, dependency and container vulnerability scans, policy checks, and successful deployment health checks. Gates should be risk-based and actionable rather than creating excessive pipeline delays.

45. **How do you troubleshoot a CI/CD pipeline that suddenly starts failing?**
    - Identify the first failing stage and compare the run with the last successful run. Inspect logs, runner availability, changed dependencies, credentials and permissions, network access, external service status, and recent pipeline configuration changes. Reproduce the failure in a clean environment, isolate flaky tests, and avoid rerunning blindly when the failure may be deterministic or security-related.

## Docker and Containerization

46. **What is Docker, and why is it used?**
    - Docker is a platform and ecosystem for building, distributing, and running container images. Containers package an application and its user-space dependencies into an isolated runtime environment while sharing the host operating-system kernel. They improve consistency across development, testing, and deployment, but they are not full virtual machines.

47. **What is the difference between a Docker image and a container?**
    - An image is an immutable, layered template containing a filesystem and metadata used to create containers. A container is a running or stopped instance of an image with its own writable layer and runtime configuration. Multiple containers can be created from the same image.

48. **What is a Dockerfile, and what are its commonly used instructions?**
    - A Dockerfile describes how to build an image. Common instructions include `FROM`, `WORKDIR`, `COPY`, `ADD`, `RUN`, `ENV`, `ARG`, `EXPOSE`, `USER`, `CMD`, and `ENTRYPOINT`. Prefer deterministic builds, small trusted base images, non-root runtime users, and minimal layers.

    ```dockerfile
    FROM python:3.12-slim
    WORKDIR /app
    COPY requirements.txt .
    RUN pip install --no-cache-dir -r requirements.txt
    COPY . .
    RUN useradd --create-home appuser
    USER appuser
    CMD ["python", "app.py"]
    ```

49. **What is the difference between `CMD` and `ENTRYPOINT` in Docker?**
    - `ENTRYPOINT` defines the main executable that a container runs. `CMD` provides default arguments or a default command, depending on how `ENTRYPOINT` is set. With an exec-form entrypoint, a `CMD` array is commonly used as default arguments that can be overridden at runtime.

50. **What is the difference between `COPY` and `ADD` in a Dockerfile?**
    - `COPY` copies files and directories from the build context or a build stage into the image. `ADD` supports additional behavior, including extracting local tar archives and fetching remote URLs in supported cases. Prefer `COPY` for ordinary file copies because its behavior is simpler and more predictable.

51. **What are Docker volumes and bind mounts?**
    - Both persist or share data outside a container's writable layer. A Docker-managed volume is managed by Docker and is commonly used for persistent application data. A bind mount maps a specific host path into a container and is useful for development or controlled host-file access. Protect host paths and avoid giving containers unnecessary write access.

52. **What is Docker networking, and what network drivers are commonly used?**
    - Docker networking allows containers to communicate with each other and external networks. Common drivers include `bridge` for networks on one host, `host` for sharing the host network namespace, `none` for disabling networking, and `overlay` for multi-host networking in supported setups. User-defined bridge networks provide DNS-based container-name discovery.

53. **What is Docker Compose?**
    - Docker Compose defines and runs multi-container applications using a YAML file. It can describe services, networks, volumes, environment settings, health checks, and dependencies. It is useful for local development, testing, and some single-host deployments; it is not a replacement for a full production orchestration platform in every scenario.

    ```yaml
    services:
      web:
        build: .
        ports:
          - "8080:8080"
        depends_on:
          - db
      db:
        image: postgres:16
        environment:
          POSTGRES_PASSWORD: localdev
        volumes:
          - db-data:/var/lib/postgresql/data
    volumes:
      db-data:
    ```

54. **How do you reduce Docker image size and improve build efficiency?**
    - Use an appropriate minimal base image, multi-stage builds, `.dockerignore`, cached dependency layers, and only required runtime files. Avoid installing compilers and build tools in the final image when they are not needed. Pin image versions or digests according to the release policy, scan images, and do not optimize size at the expense of security or maintainability.

55. **What is a multi-stage Docker build?**
    - A multi-stage build uses multiple `FROM` stages in one Dockerfile. One stage can compile or build the application, and a later stage copies only the required output into a smaller runtime image. This reduces the final image footprint and excludes build-time tools and files that are not needed at runtime.

    ```dockerfile
    FROM golang:1.23 AS build
    WORKDIR /src
    COPY . .
    RUN go build -o /out/app ./...

    FROM debian:bookworm-slim
    COPY --from=build /out/app /usr/local/bin/app
    ENTRYPOINT ["/usr/local/bin/app"]
    ```

56. **How do you troubleshoot a Docker container that exits immediately or keeps restarting?**
    - Inspect `docker ps -a`, `docker logs`, and `docker inspect`. Check the container's exit code, configured command, missing files or environment variables, permissions, health checks, resource limits, and dependencies. Run an interactive shell or override the entrypoint when appropriate. A container should generally run a foreground process; backgrounding the main process can cause the container to exit.

    ```bash
    docker ps -a
    docker logs --tail 100 <container>
    docker inspect <container>
    docker exec -it <running-container> sh
    ```

## Kubernetes and Container Orchestration

57. **What is Kubernetes, and why is it used?**
    - Kubernetes is an open-source container orchestration platform. It automates deployment, scheduling, scaling, service discovery, and lifecycle management for containerized workloads. It provides desired-state reconciliation, but operating a cluster still requires careful capacity planning, security, networking, observability, and application design.

58. **Explain the Kubernetes architecture.**
    - A Kubernetes cluster has a control plane and worker nodes. The control plane commonly includes the API server, scheduler, controller manager, and a cluster data store such as etcd. Worker nodes run a kubelet, a container runtime, and networking components. Managed Kubernetes services may operate some or most control-plane components for the customer.

59. **What is a Pod in Kubernetes?**
    - A Pod is the smallest deployable unit in Kubernetes and can contain one or more tightly coupled containers. Containers in the same Pod share networking and can share volumes. Pods are designed to be replaceable; controllers such as Deployments create and manage them rather than requiring operators to maintain individual Pods manually.

60. **What is the difference between a Deployment, ReplicaSet, and StatefulSet?**
    - A Deployment manages stateless application rollouts and typically creates ReplicaSets to maintain the desired number of Pods. A ReplicaSet ensures that a specified number of matching Pods are running. A StatefulSet manages workloads that need stable identities and often persistent storage, such as databases or clustered systems.

61. **What is a Kubernetes Service, and what types are available?**
    - A Service provides a stable network endpoint for a changing set of Pods selected by labels. Common types are `ClusterIP` for internal cluster access, `NodePort` for exposing a port on nodes, and `LoadBalancer` for requesting an external load balancer from a supporting environment. Headless Services provide DNS-based discovery without a virtual cluster IP.

62. **What is the difference between Ingress and a LoadBalancer Service?**
    - A `LoadBalancer` Service exposes a service through a cloud or infrastructure load balancer. Ingress defines HTTP/HTTPS routing rules, such as host- and path-based routing, and requires an Ingress controller to implement them. Gateway API is a newer, more expressive Kubernetes networking API in supported environments. Neither Ingress nor Gateway API creates a working data plane without an implementation.

63. **What are ConfigMaps and Secrets in Kubernetes?**
    - ConfigMaps store non-sensitive configuration, while Secrets are intended for sensitive values such as credentials and tokens. Kubernetes Secrets are not automatically a complete secret-management solution: base64 encoding is not encryption. Enable encryption at rest where appropriate, apply strict RBAC, restrict exposure to Pods, and consider an external secrets manager.

64. **What are liveness, readiness, and startup probes?**
    - A liveness probe checks whether a container should be restarted. A readiness probe determines whether it should receive traffic. A startup probe gives slow-starting applications time to initialize before liveness checks take effect. Probe endpoints should reflect the intended condition; overly aggressive probes can create restart loops or remove healthy workloads from service.

65. **What are Kubernetes requests and limits?**
    - Resource requests are used by the scheduler to place Pods and represent the resources a workload is expected to need. Limits cap resource usage where supported. CPU limits can throttle a container; memory-limit breaches can lead to an out-of-memory kill. Set values using observed usage and load testing rather than arbitrary guesses.

66. **How does Kubernetes Horizontal Pod Autoscaler (HPA) work?**
    - HPA adjusts the replica count of a scalable workload based on observed metrics such as CPU, memory, or custom/external metrics. It compares current measurements with configured targets and changes the desired replica count within defined bounds. Correct resource requests, metrics availability, startup time, and application capacity are important for effective scaling.

67. **What is the difference between a rolling update and a rollback in Kubernetes?**
    - A rolling update gradually replaces existing Pods with a new version according to Deployment strategy settings. A rollback returns the Deployment to an earlier revision, provided the required ReplicaSet history remains available. Rollbacks do not automatically undo external side effects or database schema changes, so application and migration design must support safe recovery.

    ```bash
    kubectl rollout status deployment/my-app
    kubectl rollout history deployment/my-app
    kubectl rollout undo deployment/my-app
    ```

68. **How do you troubleshoot a Pod stuck in `Pending`, `CrashLoopBackOff`, or `ImagePullBackOff`?**
    - Start with `kubectl get pods -o wide` and `kubectl describe pod`. For `Pending`, inspect scheduling events, resource requests, node capacity, affinity, taints, and persistent-volume claims. For `CrashLoopBackOff`, inspect previous container logs, exit codes, probes, and configuration. For `ImagePullBackOff`, verify the image name/tag, registry access, image pull secrets, and network connectivity.

    ```bash
    kubectl get pods -A
    kubectl describe pod <pod-name>
    kubectl logs <pod-name> --previous
    kubectl get events --sort-by=.metadata.creationTimestamp
    ```

69. **What are taints, tolerations, node selectors, and affinity in Kubernetes?**
    - Node selectors and node affinity constrain which nodes can run a Pod. Taints repel Pods from nodes unless the Pod has a matching toleration. Affinity and anti-affinity can express preferred or required placement based on node or Pod labels. These features support workload isolation, dedicated node pools, and spreading replicas for availability.

70. **What is Helm, and why is it used with Kubernetes?**
    - Helm is a package manager for Kubernetes. A Helm chart packages Kubernetes manifests with templates and configurable values; a release is an installed instance of a chart. Helm supports repeatable installation, upgrades, rollback, and reuse of application deployment definitions. Review rendered manifests and chart versions before deploying to production.

## Infrastructure as Code and Configuration Management

71. **What is Terraform, and how does it work?**
    - Terraform is an infrastructure-as-code tool that uses declarative configuration to describe desired resources. Providers interact with cloud platforms and other APIs. Terraform compares configuration with its state and observed infrastructure, proposes a plan, and applies approved changes. Review plans carefully because resource replacement or deletion can affect production.

72. **What is Terraform state, and why is it important?**
    - Terraform state maps configuration resources to real infrastructure objects and records attributes needed to plan future changes. It can contain sensitive values, so protect it with access controls, encryption, and secure storage. In team environments, use a remote backend with locking where supported to reduce conflicting operations.

73. **What is the difference between `terraform plan` and `terraform apply`?**
    - `terraform plan` calculates and displays proposed infrastructure changes without applying them. `terraform apply` executes a plan, creating, updating, or deleting resources. In controlled environments, teams often save and review a plan artifact before applying it, and ensure the approval is tied to the exact plan and code revision.

    ```bash
    terraform init
    terraform fmt -check
    terraform validate
    terraform plan -out=tfplan
    terraform apply tfplan
    ```

74. **What are Terraform modules?**
    - A Terraform module is a collection of configuration files managed together. The root module is the working directory where Terraform runs; child modules can be called to package reusable infrastructure patterns. Well-designed modules expose clear inputs and outputs, document assumptions, and avoid excessive abstraction or hidden dependencies.

75. **What is Terraform drift, and how do you detect it?**
    - Drift occurs when real infrastructure differs from the configuration or state Terraform expects, often because someone made a manual change or an external system modified a resource. Run a refresh-aware plan to identify differences and decide whether to update configuration, reconcile the resource, or deliberately accept a change. Prevent unreviewed console edits through policy and access controls.

76. **What is the difference between Terraform and Ansible?**
    - Terraform is primarily used to provision and manage infrastructure resources through declarative desired-state configuration and state tracking. Ansible is commonly used for configuration management, orchestration, and application deployment through agentless remote execution. They can complement each other: Terraform provisions infrastructure, and Ansible configures hosts or coordinates operational tasks.

77. **What is Ansible, and what are inventories, playbooks, and roles?**
    - Ansible automates configuration and orchestration, commonly over SSH for Linux hosts. An inventory identifies managed hosts and groups; a playbook defines tasks to run; and a role organizes reusable tasks, handlers, variables, templates, and files. Modules perform operations, while handlers run when notified by changed tasks.

    ```yaml
    - name: Install and start Nginx
      hosts: web
      become: true
      tasks:
        - name: Install Nginx
          ansible.builtin.package:
            name: nginx
            state: present
        - name: Enable and start Nginx
          ansible.builtin.service:
            name: nginx
            state: started
            enabled: true
    ```

78. **What is idempotency, and why is it important in DevOps automation?**
    - An operation is idempotent when applying it repeatedly produces the same intended result as applying it once. Idempotent automation can be safely retried and reduces unexpected changes. Configuration-management tasks should describe the desired state, such as ensuring a package is installed, instead of blindly repeating commands that may append duplicate configuration or cause side effects.

79. **What is the difference between declarative and imperative automation?**
    - Declarative automation describes the desired end state and lets a tool determine the actions needed to reach it. Imperative automation specifies the sequence of commands or steps to execute. Terraform and Kubernetes manifests are largely declarative; shell scripts are commonly imperative. Declarative approaches improve reconciliation, while imperative steps remain useful for procedural tasks.

80. **How do you manage infrastructure changes safely across development, staging, and production?**
    - Use reusable modules or templates with separate environment-specific inputs and isolated state. Apply changes through reviewed pull requests and automated plans, require approvals for sensitive environments, and restrict production credentials. Test changes in lower environments, maintain audit trails, and design changes to be reversible or compatible with rollback. Avoid sharing state or secrets across environments without a deliberate reason.

## Cloud and Deployment Architecture

81. **What are the main benefits of cloud computing for DevOps teams?**
    - Cloud platforms provide on-demand infrastructure, managed services, elastic capacity, global regions, automation APIs, and usage-based pricing. These capabilities reduce the need to operate all underlying hardware and can speed up environment creation. Cloud adoption still requires cost management, security configuration, resilience planning, governance, and awareness of provider-specific limits.

82. **What is the difference between IaaS, PaaS, and SaaS?**
    - **Infrastructure as a Service (IaaS)** provides foundational compute, storage, and networking resources that customers configure and manage.
    - **Platform as a Service (PaaS)** provides a managed application platform, reducing infrastructure administration.
    - **Software as a Service (SaaS)** provides a complete application to end users.
    - The responsibility split varies by service and provider.

83. **What is a Virtual Private Cloud (VPC) or Virtual Network?**
    - A VPC or virtual network is a logically isolated network in a cloud environment. It contains subnets, routing configuration, gateways, and network security controls. Public and private subnet designs, route tables, NAT, private endpoints, and segmentation determine how workloads communicate with each other and with external networks.

84. **What is the difference between a security group and a network ACL in AWS?**
    - In AWS, a security group is a stateful virtual firewall attached to network interfaces or resources such as instances. Network ACLs are stateless filters applied at the subnet boundary, so inbound and outbound rules must be considered separately. Security groups are typically the primary control for instance-level traffic; use both according to a clear network policy.

85. **What are high availability and fault tolerance?**
    - High availability aims to keep a service operational despite some failures, often using redundant instances and failover. Fault tolerance aims to continue operating through specified failures with little or no interruption. Both require eliminating single points of failure, health-based routing, redundancy across appropriate failure domains, and tested recovery procedures. They do not guarantee protection from every failure.

86. **What is auto scaling, and what factors should be considered when configuring it?**
    - Auto scaling adjusts the number or size of resources in response to demand or policy. Consider scaling metrics, minimum and maximum capacity, cooldown or stabilization behavior, startup time, health checks, dependencies, quotas, and cost. Scaling out application instances may not help if the database or another downstream service is already the bottleneck.

87. **What is a CDN, and how does it improve application performance?**
    - A Content Delivery Network serves cached content from geographically distributed edge locations closer to users. It can reduce latency and origin load for static assets and suitable cacheable responses, and may provide TLS termination and traffic protection. Cache-control headers, invalidation, personalization, and content freshness must be designed carefully to avoid serving stale or private data.

## Monitoring, Logging, SRE, and Incident Management

88. **What is the difference between monitoring and observability?**
    - Monitoring tracks known signals and alerts on predefined conditions. Observability is the ability to infer a system's internal state from its outputs, commonly using metrics, logs, and traces. Monitoring is one part of observability. Good observability helps teams investigate unfamiliar failures, not just detect known ones.

89. **What are metrics, logs, and distributed traces?**
    - Metrics are numerical time-series measurements such as request rate, latency, error rate, and CPU utilization. Logs are timestamped event records that provide detailed context. Distributed traces follow a request across multiple services and show spans, timing, and dependencies. Correlating all three helps diagnose performance and reliability issues.

90. **What is Prometheus, and how does it collect metrics?**
    - Prometheus is an open-source monitoring and alerting system. It commonly scrapes metrics from HTTP endpoints at configured intervals, stores time-series data with labels, and provides PromQL for querying. Exporters expose metrics from systems that do not natively provide Prometheus-format endpoints. Label cardinality should be controlled because unbounded labels can increase storage and query costs.

91. **What is Grafana, and how is it used with Prometheus?**
    - Grafana is an observability and analytics platform used to query and visualize data sources such as Prometheus, Loki, and cloud monitoring services. Teams create dashboards for service health, infrastructure, and business indicators, and configure alerts. Dashboards should focus on actionable signals and clear units, thresholds, and ownership.

92. **What is the ELK or Elastic Stack?**
    - The Elastic Stack commonly refers to Elasticsearch, Logstash, and Kibana, often with Beats or Elastic Agent for collection. It is used to ingest, process, index, search, and visualize logs and other data. Production deployments need retention policies, access control, careful field design, and capacity planning because indexing and storage can be expensive.

93. **What are SLIs, SLOs, and SLAs?**
    - An **SLI** (Service-Level Indicator) is a measured reliability signal, such as request success rate or latency.
    - An **SLO** (Service-Level Objective) is the target for an SLI over a defined window, such as 99.9% successful requests over 30 days.
    - An **SLA** (Service-Level Agreement) is a formal commitment to customers and may include remedies if targets are missed.
    - SLOs should reflect user experience and business priorities.

94. **What is an error budget, and how does it influence release decisions?**
    - An error budget is the amount of unreliability allowed by an SLO over its measurement window. For an availability SLO of 99.9%, the budget represents the remaining 0.1% of eligible events that may fail. Teams use burn rate and budget consumption to balance release velocity against reliability work. A rapidly exhausted budget may justify slowing risky changes and prioritizing stability.

95. **How do you handle a production incident and conduct a postmortem?**
    - First assess impact, declare the incident, assign an incident lead and clear roles, communicate status, and prioritize mitigation or rollback. Preserve evidence and maintain a timeline. After recovery, hold a blameless review that identifies contributing technical and process factors, evaluates detection and response, and assigns measurable follow-up actions with owners and due dates. The goal is learning and prevention, not individual blame.

## Security and Advanced DevOps Practices

96. **What is DevSecOps?**
    - DevSecOps integrates security practices throughout the software delivery lifecycle rather than treating security as a final approval step. It includes secure coding, threat modeling, dependency and container scanning, infrastructure policy checks, secrets detection, access controls, and continuous monitoring. Automation should provide actionable feedback early while keeping risk-based human review for critical decisions.

97. **What is the principle of least privilege?**
    - Least privilege means giving a user, service, process, or workload only the permissions required to perform its job, for only as long as needed. Apply it to cloud IAM roles, CI/CD runners, Kubernetes RBAC, database accounts, and secrets access. Prefer scoped, short-lived credentials and periodically review permissions.

98. **What are SAST, DAST, and software composition analysis (SCA)?**
    - **SAST** analyzes source code or compiled code for potential vulnerabilities without running the application.
    - **DAST** tests a running application from the outside to identify security weaknesses.
    - **SCA** identifies third-party and open-source dependencies, their versions, licenses, and known vulnerabilities.
    - These techniques complement one another and should be selected according to application risk and pipeline constraints.

99. **What is GitOps, and how does it work with Kubernetes?**
    - GitOps uses Git as the version-controlled source of truth for declarative infrastructure and application configuration. An agent or controller continuously compares the live environment with the desired state in Git and reconciles differences. Tools such as Argo CD and Flux support this model. Changes are reviewed through Git workflows, and drift can be detected and corrected through reconciliation policies.

100. **How would you design a secure, reliable, and scalable DevOps platform for a production application?**
    - Start with clear availability, recovery, security, performance, and cost requirements. Use version-controlled application and infrastructure definitions, automated CI/CD with quality and security gates, immutable artifacts, and isolated environments. Deploy workloads on appropriately managed compute or Kubernetes, distribute traffic across failure domains, use managed data services where suitable, and secure identities and secrets with least privilege. Add metrics, logs, traces, SLO-based alerting, tested backups, disaster-recovery procedures, and incident runbooks. Validate the design through load tests, failure exercises, security reviews, and ongoing cost and reliability reviews.
