# Scripts to Rule Them All

_You've cloned someone's project code._  Now what?  How do you install the project's dependencies?  How do start up its server?  How do you clean up the project folder and reset it for your next use?

This is a set of _intent-based scripts_ for use as convention by Cisco SEs in our coding projects.

The scripts could be written in any language; so long as, an appropriate _shebang_ line is included at the top of the script and the intended consumers of the project have the necessary interpreters loaded on their machines.

While the patterns demonstrated in this set of scripts can work for any project based on any language, these particular examples are for Python-based projects.

You _do not_ have to implement all of these scripts for your project.  Implement only what your project needs.  The value here is consistency.  If your project needs some "setup" before it can be used, create `script/setup` your project repository and we will know where to find it.

> The following two sections (and portions of the script descriptions), come straight from the [inspiration](#inspiration) for these scripts.

## The Idea

If your scripts are normalized by name across all of your projects, your
contributors only need to know the pattern, not a deep knowledge of the
application. This means they can jump into a project and make contributions
without first learning how to bootstrap the project or how to get its tests to
run.

The intricacies of things like test commands and bootstrapping can be managed by
maintainers, who have a rich understanding of the project's domain. Individual
contributors need only to know the patterns and can simply run the commands and
get what they expect.

## The Scripts

Each of these scripts is responsible for a unit of work. This way they can be
called from other scripts.

This not only cleans up a lot of duplicated effort, it means contributors can do
the things they need to do, without having an extensive fundamental knowledge of
how the project works. Lowering friction like this is key to faster and happier
contributions.

The following is a list of scripts and their primary responsibilities.


## CiscoSE Scripts

### Summary

| Script | Intent |
|:--|:--|
| [`script/bootstrap`](#script-bootstrap) | Verify system dependencies |
| [`script/installdeps`](#script-installdeps) | Install project dependencies |
| [`script/clean`](#script-clean) | Clean-up project artifacts |
| [`script/setup`](#script-setup) | Setup or reset the project to an initial state |
| [`script/update`](#script-update) | Update the project after a fresh pull |
| [`script/test`](#script-test) | Run the project's test suite |
| [`script/build`](#script-build) | Build the project's product(s) |
| [`script/ci`](#script-ci) | Continuous integration script |
| [`script/server`](#script-server) | Control project servers and services |
| [`script/console`](#script-console) | Access the project's console |
| [`script/demo`](#script-demo) | Start running a demo for the project |

### script/bootstrap

[`script/bootstrap`][bootstrap] is used to verify the presence of system dependencies needed to work with this project.

This could be ensuring that a Python interpreter of the correct version is installed, or that the Docker service is running, or that Node is installed and up-to-date.

The goal is to verify that all expected system dependencies are available.

### script/installdeps

[`script/installdeps`][installdeps] is used solely for installing the dependencies of the project.

This can mean Python packages, npm packages, RubyGems, Git submodules, etc.

The goal is to make sure all required dependencies are installed.

### script/clean

[`script/clean`][clean] is used to clean the project directory and clean-up any other system services that have had artifacts created by the project.

The script may implement command line options that clean specific services and/or "deep clean" the system removing all artifacts created by the project.

### script/setup

[`script/setup`][setup] is used to set up a project in an initial state. This is typically run after an initial clone, or, to _reset_ the project back to its _initial state_.

Typically, [`script/clean`][clean] and [`script/installdeps`][installdeps] are run inside this script.

### script/update

[`script/update`][update] is used to update the project after a fresh pull.

If you have not worked on the project for a while, running [`script/update`][update] after a pull will ensure that everything inside the project is up to date and ready to work.

Typically, [`script/installdeps`][installdeps] is run inside this script. This is also a good opportunity to run database migrations or any other things required to get the state of the app into shape for the current version that is checked out.

### script/test

[`script/test`][test] is used to run the test suite of the project.

A good pattern to support is having an optional argument that is a file path. This allows you to support running single tests.

Linting (i.e. flake8, rubocop, jshint, pmd, etc.) can also be considered a form of testing. These tend to run faster than tests, so put them towards the beginning of a [`script/test`][test] so it fails faster if there's a linting problem.

[`script/test`][test] should be called from [`script/ci`](ci), so it should handle setting up the project appropriately based on the environment. For example, if called in a development environment, it should probably call [`script/update`][update] to always ensure that the application is up to date. If called from [`script/ci`][ci], it should probably reset the application to a clean state.

### script/build

[`script/build`][build] builds the project's product(s).

This can be a Python package, Docker image, etc.

If the project has multiple build products, for example a project that builds a package and generates documentation, you may use arguments to control which products are built "by default" and which ones must be selected.

### script/ci

[`script/ci`][ci] is used for your continuous integration server. This script is typically only called from your CI server.

You should set up any specific things for your environment here before your tests are run. Your test are run simply by calling [`script/test`][test].

### script/server

[`script/server`][server] is used to control (start, stop, reset, etc.) any project servers or services.

For a web application, this might start up any extra processes that the 
application requires to run in addition to itself.

[`script/update`][update] should be called ahead of any application booting to ensure that the application is up to date and can run appropriately.

### script/console

[`script/console`][console] is used to open a console for the project.

A good pattern to support is having an optional argument that is an environment
name, so you can connect to that environment's console, if your project supports multiple environments.

You should configure and run anything that needs to happen to open a console for
the requested environment.

### script/demo

[`script/demo`][demo] is used to set up the project and begin running a demo of its capabilities.

This script should call [`script/setup`][setup] or [`script/update`][update] as needed to prepare the project for demonstration; as well as, calling [`script/serve`][serve], [`script/console`][console], and etc. as needed to begin the demonstration.

The goal is to setup the project, initialize and start any needed services, and make everything ready for an individual to demonstrate the capabilities of the project.

A good pattern to follow is to output to the user at the end of the script the links, resources, and instructions they will need to run your demo.

## Inspiration

The GitHub Engineering Team: [Scripts to Rule Them All](https://githubengineering.com/scripts-to-rule-them-all/)

[github/scripts-to-rule-them-all](https://github.com/github/scripts-to-rule-them-all)

[bootstrap]: bootstrap
[installdeps]: installdeps
[clean]: clean
[setup]: setup
[update]: update
[test]: test
[build]: build
[ci]: ci
[server]: server
[console]: console
[demo]: demo
