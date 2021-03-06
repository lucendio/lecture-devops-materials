Tips & Guidance
===============

This document gives some advice on how to approach the exercise and how to deal with certain problems. The content
might only be applicable to a certain extent, depending on the approach you chose. It is based on personal experience
and thus somewhat opinionated. You are more than welcome to walk your own path.

*NOTE: the content of this document is not finite but also not updated regularly*



### Approach and workflow

* there is no universal best practise only the most reasonable and suitable solution for your specific use case that you
  are trying to implement 
* infrastructure code is subject to development just as it is with application code
  - you may establish a development life cycle starting with writing code on your local computer, but instead of
    locally building and running your application, you *allocate* and *configure* either on your computer or just
    trigger it locally  
* tear down and recreate your infrastructure on a regular basis
  - shows reproducibility
  - safes some credits
* automate every step of the way right from the beginning
  - not necessarily by starting to write jobs for your automation tool, but rather by scripting those steps  
  - this can speed up your work and may serve as the foundation for the CI/CD integration happening later down the road



### A guided path *(WIP)*

*Please note, that the following outlines do not claim to be exhaustive.*

## (0) Thinking out loud

1. What do you already know about app deployment, automation and infrastructure (from your
   previous courses & projects a/o from you work place)?

2. What is the application, that you are supposed to deploy, made of? What tool chain is
   used to create it? Which dependencies does it have?

3. What would be the simplest architecture (if the software development life cycle would
   be completely automated)?

*Imagine, you follow the app from its written source all the way down to starting it so
that others can access it on the internet.*
```
  Source code            Automation                    Deployment
  -----------    ===>    ------------------    ===>    ----------------
  (app code)             (test + build app)            (host / run app) 
                                                      
                          * Who?                        * Where?
                          * Where?                      * How is it getting there?
                          * Tool chain?               
                          * Dependencies?             
```
Which steps would the app need to go through?

4. Which components (services) would be needed? How to set them up in a reproducible way?
   Which of them will be provisioned by myself and which ones can be managed services?


#### (1) Preparation

* if you have very little experience on UNIX basics a/o the terminal a/o, take a look at the 
  [tutorial](./links.md#unixlinux-basics) and spent some time and make yourself familiar with these technologies

* check out the application and get it to work locally

* think about where you want to host your infrastructure (and thus the application) 
  * is one of the [available cloud providers](./links.md#cloud-providers) an option?
  * does your own computer have enough resources (to host one or more virtual machines)?
  * if non of these options work for you, please get in touch
  * (also, if you are uncertain about this whole part)

* take a look at some of the core technologies you might be going to use
  * runtime:
    * hypervisors: VirtualBox, QEMU, VMware 
    * container engines: Podman, Docker, LXC 
  * automation: 
    * allocate:
      * Terraform, if you want to deploy to the cloud
      * Vagrant, if you want to run stuff locally (verify that the available resources on your computer is sufficient)
    * configure: Ansible, Chef, Salt
  * CI/CD: Jenkins, Concourse, Gitlab, Github Actions
  * Monitoring: Prometheus + Grafana, EFK Stack

* all the fundamental technologies should be taught before the initial [concept](./deliverables/exercise_concept.md)
  is due


#### (2) Writing the concept

* necessary pieces: VCS, CI/CD, environments, monitory

* make yourself familiar with e few *Internet standards & distributed concepts:* IP, DNS, HTTP(S), load balancing, proxying
  
* think about how the deployment process of the *"workload"* should look like
* think about how you want to create the infrastructure and automate the process (entirely described in code) 

* create a new Git repository and put a `concept.md`

* visualize the overall setup that you have planned - maybe [draw a nice diagram](https://app.diagrams.net)

* by the time you hand in the first version of your concept, you should have started to write some infrastructure code,
  even it is just a few lines; maybe start with setting up an environment; usually, one needs a computer (e.g.
  virtual machine) first to then configure the environment and deploy a service in it


#### (3) Implementation

* use the repository that already contains your concept and add all the infrastructure code that does not need to
  sit next to the source code of the [application](https://github.com/lucendio/lecture-devops-app) you want to deploy  

* (fork the [application](https://github.com/lucendio/lecture-devops-app) repository if you need to introduce changes
  in order to make it work with your implementation


#### (0) Timeline

*__DISCLAIMER__: This is no one-size-fits-all but merely an incomplete suggestion. It may or may not be helpful 
on your journey through the course, and it's probably not (fully) in sync with the course road map.*

Based on an average of 20 weeks (starting with *onboarding* and concluding with *review*)

01  Go through the provided materials and note down questions
02  Familiarize yourself with the [scope](./links.md#devops) of this course and its topics 
03  Refresh [linux](./links.md#unixlinux-basics) and [git](./links.md#git) skills
04  Fork the [app](https://github.com/lucendio/lecture-devops-app), that will be used as *deployable workload*
    and get it to run locally
05  Revisit the 12-Factor-App; install a container runtime on your system start some containers and try to build
    container images as well 
06  Install a hypervisor locally; start a virtual machine; connect to a shell inside the machine; find out which
    process manager is being used and try to stop and start some of the processes there
07  Revisit the agile-based software development life cycle; start to note down your first thoughts about
    your implementation; allocate some cloud resources (e.g. virtual machines, networks, storage, managed services)
08  Automate the resource allocation so that it becomes reproducible; destroy and re-create your setup 
09  Use a Configuration Management Tool to install and configure some software/services on one or more machines
10  Learn about CI/CD and think about its role in your architecture; write a pipeline and configure a trigger
11  Finish the initial version of your concept; add a diagram (or some other visualization) to show off your
    architecture design
12  Transform the application source code into a deployable artifact; integrate the process into the CI/CD platform
    (that's the CI part) and set up a trigger; this requires a CI/CD platform to begin with, so make sure you have
    one - either deploy (automate!) one yourself (remember poll vs. push) or use a managed one (SaaS)  
13  Set up an environment founded on the knowledge and technologies you gained over the past weeks; deploy the
    application and automate the process; implement some deployment strategy to prevent any down time 
14  Evaluate Container Orchestration to see whether it would be useful for your setup 
15  Integrate the application deployment into your CI/CD platform (that's the CD part) to reflect a complete 
    software development life cycle 
16  Define and provision a second target environment; configure another trigger; ensure that the application
    successfully deploys to that environment, too
17  Evaluate monitoring solutions and go through their documentation to understand how it's being deployed and
    configured; maybe do a test deployment - by hand - for easier debugging; take notes
18  use the notes to automate the provisioning of the monitoring; integrate and document
19  Ensure complete reproducibility by destroying the whole setup and re-create it from scratch; make sure 
    everything still works
20  Double-check the grading criteria and see if they are met; plan and trial run the review presentation 



### Technical Tips

* a change in VCS can be, for example, a modification of the code or a new tag
* FQDNs are typically provided through DNS
    * simulated locally: (a) by editing the `/etc/hosts` file or (b) by running a local DNS service (e.g. `dnsmasq`)
    * public: third-party DNS provider that runs *name servers* (requires to own a *domain*)
    * service: [nip.io](https://nip.io/) (and alike) provide *wildcard DNS for any IP address*
* to access the same application deployed in different environments, they typically get assign distinct FQDNs, instead
  of, for example, different ports (e.g. `dev.myapp.tld`, `prod.myapp.tld`)
* even though the application requires a persistence layer, it can be considered *ephemeral*
* to prevent side effects and increase reproducibility (necessary for fair evaluation/grading), it is recommended to not
  install any of the services to the local host system (e.g. your computer)
* in order to distribute incoming traffic across multiple instances of an application a load balancer can be
  placed *in front* of them
* to implement TLS for local (non-public) services, self-signed certificates are a valid solution (keep in mind, that
  other services might need to have the certificate installed, when communication between each other)
* [Let's Encrypt](https://letsencrypt.org/docs/) issues certificates free of charge for public domains (e.g. used by
  [*cert-manager*](https://github.com/jetstack/cert-manager) on K8s)
* on public clouds, being able to *allocate* and *configure* your infrastructure from scratch in an automated fashion on
  demand, can help saving some credits (e.g. tear down everything after a day of development work and next day bringing
  everything back up)
* the [application repository](https://github.com/lucendio/lecture-devops-app) defines all its dependencies. For more
  details, please refer to the [README](https://github.com/lucendio/lecture-devops-app/blob/master/app/README.md)
* infrastructure provisioning can be triggered manually and doesn't have to be integrated into the automation service



### Example stacks

The subsequent collection of examples describes, in a very vague manner, possible technology stacks and even relations
between some of them. It is just meant as an inspiration and starting point for research when designing and developing
you individual exercise solution.

*Please note, that non of the examples are mutual exclusive. Certain items across these examples may yield reasonable
and even more suitable combination(s).*

__*Setup categories:*__
a) *local* (your own computer)
b) *remote* (any another computer; aka 'cloud')
c) *hybrid* (local + remote)


##### 1a – virtual machines *[local]*

* Virtualbox (+Vagrant) -> *application*    *[runtime environment]*
* Packer (+Ansible)                         *[automation]*
* Gitea                                     *[VCS]*
* Jenkins                                   *[CI/CD]*
* HAProxy                                   *[proxy/vhost/load-balancer]*
* ELK                                       *[logging]*


##### 1b – Containers *[local]*

* (Virtualbox + Vagrant)    
* Ansible                   *[automation]*
* Docker -> *application*   *[runtime environment]*
* Gitea                     *[VCS]*
* Concourse                 *[CI/CD]*
* Nginx                     *[proxy/vhost/load-balancer]*
* Monit                     *[metrics]*


##### 1c – Orchestration *[local]*

* (Virtualbox + Vagrant)
* Ansible (+Kubespray)                      *[automation]*
* Kubernetes (+Docker) -> *application*     *[runtime environment]*
* Helm                                      *[automation]*
* Gitlab                                    *[VCS,CI/CD]*
* Prometheus + Grafana                      *[metrics]*


##### 2a.1 – Cloud services + Monitoring VM *[remote]*

* Bitbucket                             *[VCS]*
* Heroku (+Terraform) -> *application*  *[CI/CD,runtime environment,automation,proxy,load balancer]*


##### 2a.2 – Cloud services + Monitoring VM *[hybrid]*

* Heroku (+Terraform) -> *application*     *[VCS,CI/CD,runtime environment,automation,proxy,load balancer]*
* EFK                                       *[logging]*
  + DigitalOcean            
  + Terraform
  * Ansible


##### 2b – Multi-Cloud: managed *[remote]*

* Github                                *[VCS]*
* TravisCI (or Github actions)          *[CI/CD]*
* AWS (+Terraform) -> *application*     *[runtime environment,automation,proxy,load balancer]*
* Splunk                                *[logging]*


##### 2c – Local + Cloud *[hybrid]*

* Github                            *[VCS]*
* Jenkins                           *[CI/CD]*
  + Virtualbox + Vagrant or Docker
  + Ansible
* DigitalOcean                      *[runtime environment]*
    * *application*
    * HAProxy                       *[proxy/load-balancer]*
    * ELK                           *[logging]*
+ Terraform                         *[automation]*
* Ansible                           *[automation]*


##### 3a - Cloud Orchestration: managed *[remote]*

* Bitbucket                 *[VCS]*
* Terraform                 *[automation]*
* GKE -> *application*      *[runtime environment,proxy,load balancer]*
* Helm                      *[automation]*
* Jenkins + K8s Plugin      *[CI/CD]*
* Stackdriver               *[logging]*


##### 3b - Cloud Orchestration: DIY *[remote]*

* Github                            *[VCS]*
* Terraform                         *[automation]*
* DigitalOcean
    * Ansible (+Kubespray)          *[automation]*
* Kubernetes -> *application*       *[runtime environment,proxy,load balancer]*
* Helm                              *[automation]*
* Jenkins + K8s Plugin              *[CI/CD]*
* EFK                               *[logging]*


##### 4 – All in One *[local or remote]*

* OpenShift:
    * HAProxy                   *[proxy,load balancer]*
    * Container Registry
    * Kubernetes                *[runtime environment]*
      + Gitea                   *[VCS]*
    * Jenkins                   *[CI/CD]*
    * EFK                       *[logging]*
    * Prometheus + Grafana      *[metrics]*


##### 5 – Jenkins X 

* Github                                *[VCS]*
* AWS -> Jenkins X -> *application*     *[CI/CD,runtime environment,proxy,load balancer]*
