# Deployment Environments

The notification-delivery system is deployed on Kubernetes in multiple environments and maintained on GitHub. The table below describes a list of possible tiers -

**Local** - Developer's desktop/workstation where changes are worked on and tried out. <br/>
**Development (dev)** - Development servers acting as a sandbox where unit testing may be performed by the developer. The environment may contain development tools, different or additional versions of libraries and support software, etc., which are not present in the production environment. <br/>
**Staging** - Mirror of production environment. <br/>
**Production** - The environment that users directly interact with.

For this projects the microservices are deployed in 2 environments namely -
1. Development (or dev) Environment
2. Production Environment

At the source code level these environments are distinguished using Git branches. 
The `development` and `production` git branchs of the respective repositories features the source code for the Development Environment and the Production Environment respectively.

![Showing all branches in the Notification Service git repository](https://github.com/adisakshya/continuous-improvement/blob/master/assets/utils/microservice-branches.png?raw=true)

The development branch is created from the master branch. An appropriate branch can be created from the development branch for any feature updates, patches etc and then it can be merged back to the development branch. Say when a feature currently in the development branch is complete to be shiped then the development branch can be merged to the master branch. The master branch reflects the code for the staging envionment through which all kinds of updates have to pass through. The production branch reflects the deployed code. A newer version of a microservice can be deployed by merging the master branch into the production branch. If you need to know what code is in production, you can check out the production branch.
