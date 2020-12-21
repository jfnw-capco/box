<p align="center">
  <img alt="delineate.io" src="https://github.com/delineateio/.github/blob/master/assets/logo.png?raw=true" height="75" />
  <h2 align="center">delineate.io</h2>
  <p align="center">portray or describe (something) precisely.</p>
</p>

# Box

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-ff69b4.svg)](https://github.com/delineateio/box/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22+)

## Purpose

The purpose of **box** is to provide a general purpose development `vagrant` box with a pre-installed set of useful tools.  Over time the installed tools is expected to be expanded.

## Usage

### Mandatory Requirements

The solution primarily uses [Hashicorp Vagrant](https://www.vagrantup.com/) and [Redhat Ansible](https://www.ansible.com/) to provision and configure a development Ubuntu 18.04 machine.  The following commands will install the mandatory requirements on macOS.

```shell
# Installs Ansible, VirtualBox & Vagrant
brew install ansible
brew install --cask virtualbox vagrant
```

> For additional information including alternative installation methods please review the appropriate official documentation

* [Redhat Ansible](https://docs.ansible.com/)
* [Oracle VirtualBox docs](https://www.virtualbox.org/wiki/Documentation)
* [Hashicorp Vagrant docs](https://www.vagrantup.com/docs)

### Optional Requirements

Optionally Vagrant Manager can be installed on MacOS to provide access from the menu bar.  For more information on Vagrant Manager see [here](https://www.vagrantmanager.com/).

```shell
# optionally install Vagrant Manager
brew install --cask vagrant-manager
```

![vagrant manager](./assets/manager.png)

### Project Configuration

The VM box created by this project is hosted on Hashicorp Vagrant Cloud [here](https://app.vagrantup.com/delineateio/boxes/box).  Consult the `vagrant` documentation for more details, below shows the minimum configuration required in the `vagrantfile` to use the box.

```ruby
Vagrant.configure("2") do |config|

  config.vm.box = "delineateio/box"
  config.vm.box_version = "1.2.0"

  # access to postgres on default port
  config.vm.network "forwarded_port", guest: 5432, host: 5432, protocol: "tcp"

end
```

In the directory where the `vagrantfile` is located the command to `vagrant up --provider virtualbox` can be run to create the VM.

> Note that specific port forwarding should be configured to access tools from the host (e.g. `postgres`, `octant`).  It maybe necessary to explicitly set different host ports to get fixed port numbers.

## Pre-configured Tools

The following tools and languages are automatically installed using `ansible` as part of the provisioning and configuration process

> There are some further details provided below on elements of the configuration

### General Tools

* [bat](https://github.com/sharkdp/bat) - A `cat` clone with syntax highlighting and Git integration
* [circleci](https://github.com/CircleCI-Public/circleci-cli) - CLI for working with [CircleCI](https://circleci.com/)
* [gh](https://cli.github.com/) - Work with issues, pull requests, checks, releases from the terminal
* [pre-commit](https://pre-commit.com/) - A framework for managing and maintaining multi-language pre-commit hooks
* [shellcheck](https://github.com/koalaman/shellcheck) - Shell script static analysis tool for instant feedback

### Security Tools

* [detect-secrets](https://github.com/Yelp/detect-secrets) - Detecting secrets within enterprise code bases before commit
* [mkcert](https://github.com/FiloSottile/mkcert) - Simple tool for making locally-trusted development certificates
* [snyk](https://github.com/snyk/snyk) - Find, fix and monitor known vulnerabilities
* [trivy](https://github.com/aquasecurity/trivy) - Simple and comprehensive vulnerability scanner for containers and other artifacts

### APIs

* [hey](https://github.com/rakyll/hey) - HTTP load testing with concurrency level and printing stats
* [httpie](https://httpie.io/) - User-friendly command line HTTP client for the API era
* [jq](https://stedolan.github.io/jq/) - Slice, filter, map and transform JSON data

### Containers & k8s

* [docker](https://www.docker.com/) - Solution for defining and using containers
* [kubectl](https://kubernetes.io/docs/reference/kubectl/kubectl/) - Controls the Kubernetes cluster manager
* [octant](https://octant.dev/) - Developer-centric web interface for Kubernetes
* [skaffold](https://skaffold.dev/) - Workflow for building, pushing and deploying your k8s applications
* [st](https://github.com/GoogleContainerTools/container-structure-test) - Powerful framework to validate the structure of a container images

### Databases

* [psql](http://postgresguide.com/utilities/psql.html) - Interactive terminal for working with Postgres

### Infrastructure Tools

* [inspec](https://community.chef.io/tools/chef-inspec/) - Turn compliance, security, and other policies into automated tests
* [osquery](https://osquery.io/) - SQL powered OS instrumentation, monitoring, and analytics framework
* [packer](https://www.packer.io/) - Automates the creation of any type of machine image
* [terraform](https://www.terraform.io/) - Infrastructure as code tool that provides a consistent workflow to manage cloud services

### Network Tools

* [iotop](https://github.com/analogue/iotop) - shows process I/O usage and issues
* [mtr](https://github.com/traviscross/mtr/) - Investigates the network between the host and destination host
* [nmap](https://nmap.org/docs.html) - Utility for network discovery and security auditing

> These tools are in addition to ths standard tools that are available in the base VM (`ping`, `netstat`, `nslookup` etc)

### Inspection Tools

* [neofetch](https://github.com/dylanaraps/neofetch) - Displays information about your operating system, software and hardware

## Installed Languages

* [clojure](https://clojure.org/) - Robust, practical, and fast programming language with a set of useful features
* [go](https://golang.org/) - Language that makes it easy to build simple, reliable, and efficient software
* [node](https://nodejs.org/en/) - As an asynchronous event-driven JavaScript runtime designed to build scalable network applications
* [ruby](https://www.ruby-lang.org/en/) - A dynamic programming language with a focus on simplicity and productivity
* [rust](https://www.rust-lang.org/) - Language empowering everyone to build reliable and efficient software
* [scala](https://www.scala-lang.org/) - Object-oriented and functional programming in one concise, high-level language

### Convenience Shell Aliases

#### Profile Shell Alias

Using `profile` will display the current contents of `~/.bash_profile` using `bat`.
![profile](./assets/profile.png)

#### Clear Shell Alias

Overrides the standard command and writes out some header information.  By using the `clear` command the following screen will be displayed.

#### Home Shell Alias

Changes the working directory to `$HOME` and runs `clear`.  Ultimate the view is the same as clear but the `pwd` will be be `$HOME`.

## Key Configurations

### Starship

The `starship` prompt has been installed and configured with simplified configuration from the defaults.  To review the configuration run `bat $HOME/.config/starship.toml`.

### Git

#### Overview

The following configuration is pre-configured for `git`.

It is still necessary to configure the user in the project `vagrantfile`.

#### Git Aliases

A number of convenient `git` aliases are provided:

* `git pretty` provides a concise log of the commits
* `git initial` enables rebasing to the root to help get a clean initial commit
* `git last` provides a detailed view of the last commit

#### GPG Commit Signing

`git` is pre-configured by default to use `gpg` signing so commits are verified.  However the key needs setting from within the project.

To learn more read the `github` documentation on [signing commits](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/signing-commits).

![git config](./assets/git.png)

### Postgres

A `docker` container for `postgres:11.6` is deployed as this is the most common database platform used for development.  The container exposes postgres on the standard `5432` port.  To simplify connectivity a `.pgpass` file has been configured to ease connectivity when using `psql`.

```shell
psql -h localhost -U postgres
```

> To access the `postgres` container from the host when developing using an IDE on the host port forwarding must be enabled.  It maybe necessary to use a non-standard port to avoid port clashes and provide a predictable port.

```ruby
# access to postgres on default port
config.vm.network "forwarded_port", guest: 5432, host: 5432, protocol: "tcp"
```

### Docker Housekeeping

In addition to help with housekeeping a `docker prune -f` job is scheduled using `cron`.

### Nginx

A proxy has been setup and configured using `mkcert` to issue self sign certificates.

> The public certificate must be installed on the host to be able to interact with the services.  See the instructions on this page to configure the host.

The purpose of this proxy is to provide single access point to services hosted in the VM.

If the configuration is required to be updated then the file at `/etc/nginx/sites-enabled/nginx.conf` can be replaced and the service restarted `sudo systemctl restart nginx.service`.

## Contributing

Contributions to this project are welcome!

* [Contribution Guidelines](https://github.com/delineateio/.github/blob/master/CONTRIBUTING.md)
* [Code of Conduct](https://github.com/delineateio/.github/blob/master/CODE_OF_CONDUCT.md)
