# Augur

branch | status
   --- | ---
master | [![Build Status](https://travis-ci.org/chaoss/augur.svg?branch=master)](https://travis-ci.org/chaoss/augur)
   dev | [![Build Status](https://travis-ci.org/chaoss/augur.svg?branch=dev)](https://travis-ci.org/chaoss/augur)

## About Augur

Augur is focused on prototyping open source software metrics. 

Functionally, Augur is a prototyped implementation of the Linux Foundation's [CHAOSS Project](http://chaoss.community) on [open source software metrics](https://github.com/chaoss/metrics). Technically, Augur is a [Flask web application](http://augurlabs.io), [Python library](http://augur.augurlabs.io/static/docs/) and [REST server](http://augur.augurlabs.io/static/api_docs/) that presents metrics on open source software development project health and sustainability. 


## Getting Started 
-------------------
### Vagrant
**The quickest way to get started working on Augur is by using [Vagrant](https://www.vagrantup.com/)** to spin up a virtual machine (VM) that comes with Augur already installed. We'll do all the work of setting up and installing dependencies, leaving you free to jump right into making something awesome. 

*Caveat: if you’re a super nerd who likes to have total control over your development environment, there’s a local installation link at the bottom of this page. For the rest of you, Vagrant is the way to go, especially if you've had trouble getting all the dependcies installed locally, are not comfortable installing them yourself, or are using an OS for which we don't currently support local installation. **We currently only support local installation for macOS and most flavors of Linux**.*

Windows installation instructions using Vagrant can be found [here](docs/python/source/windows-install.md).

Dependencies
------------

-   [Git
    client](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
-   [Vagrant](https://www.vagrantup.com/)
-   [Virtualbox](https://www.virtualbox.org/)
-   [GitHub Access Token](https://github.com/settings/tokens) (no write
    access required)

## Developer - Instructions

The directions found in this link are for installation on mac OS and amazon ec2 instance.

[https://github.com/computationalmystic/augur-group26/blob/master/Sprint%204/developer-instructions.md](https://github.com/computationalmystic/augur-group26/blob/master/Sprint%204/developer-instructions.md)

## Current State of Group 26 Use Cases and Issues

The following link is to inform users of the current progress of group 26's use cases and issues.

[https://github.com/computationalmystic/augur-group26/blob/master/Sprint%204/Current_State.md](https://github.com/computationalmystic/augur-group26/blob/master/Sprint%204/Current_State.md)

## Expected User Deliverable Product

[http://ec2-13-58-174-130.us-east-2.compute.amazonaws.com:3333/single/github.com%2Ftwitter%2Ftwemoji/commits#](http://ec2-13-58-174-130.us-east-2.compute.amazonaws.com:3333/single/github.com%2Ftwitter%2Ftwemoji/commits#)

Group 26 added a new tab to the original Augur program called Commits. In lines you will find two new and functional graphs. The first graph, is a tick graph. It shows the number of commit by the 10 authors as Percentage in year month or continuous. The second graph, is a zoomable graph. It shows the commits of code added by the top 10 authors. Its zoom function works by hovering over the graph and scrolling up or down. Giving the user a way to find exact values in the graph that spans a large difference in values.

## Guidelines
To contribute to Augur, please check out our [development guide](http://augur.augurlabs.io/static/docs/dev-guide/1-overview.html) and [notes on making contributions](CONTRIBUTING.md). Also, please note our [code of conduct](CODE_OF_CONDUCT.md). We want Augur to be a welcoming development community that is open to everyone. 

## Roadmap
Our technical, outreach, and academic goals [roadmap](https://github.com/chaoss/augur/wiki/Release-Schedule).

## License and Copyright
Copyright © 2018 University of Nebraska at Omaha, University of Missouri and CHAOSS Project at the Linux Foundation

Augur is free software: you can redistribute it and/or modify it under the terms of the MIT License as published by the Open Source Initiative. See the file [LICENSE](LICENSE) for more details.

(This work has been funded through the Alfred P. Sloan Foundation)
