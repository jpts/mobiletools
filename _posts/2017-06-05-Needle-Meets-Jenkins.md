---
layout: needle-post
title: "Needle meets Jenkins"
description: CI Integration
author: "Marco Lancini"
categories:
  - needle
  - blog
tags:
---

## Needle meets Jenkins: how to include Needle in your CI pipeline

The latest 2 releases of Needle were focused on providing features essential for its integration within a CI pipeline:

* Needle v1.1.0 introduced **automatic issue detection**: modules will now automatically detect and keep track of issues in the target app. All the issues are going to be stored in an SQLite database contained in the chosen output directory (named `issues.db`)
* Needle v1.2.0 introduced **non-interactive mode**: a new command line interface (`needle-cli.py`) will now allow to completely script Needle.

These 2 features made it possible to create the PoC below, where needle has been integrated with Jenkins.



### PoC

* Connect a Jailbroken iDevice to the machine running Jenkins (either via USB or WiFi) and start the needle agent (see the [Quick Start Guide](https://github.com/mwrlabs/needle/wiki/Quick-Start-Guide) for details)

* Create a new Jenkins project:
[![Image: Jenkins Project.](http://mobiletools.mwrinfosecurity.com/images/needle_ci_post/ci_1.png "Jenkins Project.")]((http://mobiletools.mwrinfosecurity.com/images/needle_ci_post/ci_1.png)

* Add an _"Execute Shell"_ step under the Build process:
[![Image: Execute Shell.](http://mobiletools.mwrinfosecurity.com/images/needle_ci_post/ci_2.png "Execute Shell.")](http://mobiletools.mwrinfosecurity.com/images/needle_ci_post/ci_2.png)

First, run needle in non-interactive mode, specifying the output folder, the target app, and all the modules you want to have executed (see [Non-Interactive mode on the Wiki](https://github.com/mwrlabs/needle/wiki/Non-interactive-Mode) for a full list of options):
{% gist 1294d905d17a62409f2d7500cda536bc needle_ci_1.sh %}

As a quick PoC, the “`issues.db`” database could be checked for the presence of vulnerabilities: if so, the build could be marked as a fail. Note that a more complex logic could be used to determine if the build should be failed.
{% gist 1294d905d17a62409f2d7500cda536bc needle_ci_2.sh %}


* When a build is run, the shell script will kick in and run needle against the target app:
[![Image: Run.](http://mobiletools.mwrinfosecurity.com/images/needle_ci_post/ci_3.png "Run.")](http://mobiletools.mwrinfosecurity.com/images/needle_ci_post/ci_3.png)
[![Image: Run.](http://mobiletools.mwrinfosecurity.com/images/needle_ci_post/ci_4.png "Run.")](http://mobiletools.mwrinfosecurity.com/images/needle_ci_post/ci_4.png)
