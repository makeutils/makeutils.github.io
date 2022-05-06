# make: utils

See: [https://github.com/makeutils](https://github.com/makeutils)

The `make: utils` project is intended to be a tool for dynamically managing boilerplate logic dependencies in your Makefile without the need of actually committing that logic and dependency files into your project.

<!-- TOC depthFrom:2 -->

- [Goals](#goals)
- [Work in progress](#work-in-progress)
- [Example from the proof of concept](#example-from-the-proof-of-concept)
  - [The Bootstrapping Process](#the-bootstrapping-process)
  - [.gitignore](#gitignore)
  - [Naming convention](#naming-convention)
  - [Importing modules after manual bootstrapping](#importing-modules-after-manual-bootstrapping)
- [Plans for the future, next steps](#plans-for-the-future-next-steps)
- [Origins and Proof of Concept](#origins-and-proof-of-concept)

<!-- /TOC -->

## Goals

To have a reliable tool you can depend on for including dependencies into your Makefiles.

Implement and create a set of useful and dependable make utilities

## Work in progress

This section is a reminder: This whole project is right now "Work in Progress" that may evolve in the future into a dependable and stable one.

## Example from the proof of concept

### The Bootstrapping Process

- Manually copy paste a small snippet from the [core](https://github.com/makeutils/makeutil-bootstrap) project into you makefile,
- Download basic definitions for streaming resources form network
- Download core utilities using the basic networking abilities
- Offer import utility you can use immediately in your makefile

The anatomy of a make file during the proof of concept will look like this:

```make
#
# Your logic comes at the very top
#

export AWS_PROFILE=customer
export AWS_SDK_LOAD_CONFIG=1

default:
  @echo Success!

report:
  $(TERRAFORM) show > report.txt

#
# The manual copy/pasted bootstrap code goes here
# You will need to trust the magic.
#

_:=$(or $(wildcard makeutils.bootstrap-master), $(shell curl -so makeutils.bootstrap-master https://raw.githubusercontent.com/makeutils/makeutil-bootstrap/master/makeutil-bootstrap.mak))
include makeutils.bootstrap-master

#
# Here you can include your dependencies
#

include $(call .include,terraform,0.0.1)

```

All variables and calls in the example are just representative and not a current working version.

### .gitignore

The most basic idea is to simply ignore anything starting with `makeutils.*`

### Naming convention

- Project names hosted under the `makeutils` organizations should be prefixed with `makeutil-` followed by the name of the module
- Branch names and tags are used for versioning
- The module logic should be stored in a single file located at the root of the project and should be named `makeutil-$NAME.mak`

Example for `terraform` module version `0.0.1`:

```text
https://github.com/makeutils/makeutil-terraform/blob/0.0.1/makeutil-terraform.mak
```

### Importing modules after manual bootstrapping

You need to have the name and version of the module you want to import, in this example it is going to be `terraform` version `0.0.1`

```make
include $(call .include,terraform,0.0.1)
```

## Plans for the future, next steps

- Documentation
  - explain clearly what is this project.
  - explain the bootstrapping process
  - explain how to import a module
  - create an example, getting started

- Open tasks
  - propose gitignore strategy
  - propose naming convention
    - propose local file naming convention
    - propose repositories naming convention
  - auto detect download tool
  - load core dynamically
  - auto import dependencies using a dependency file
  - clean dependencies
  - document gitignore strategy
  - official naming convention
  - strategy and specification for using other dependencies not hosted in makeutils
  - create a fancy Logo

## Origins and Proof of Concept

The basic premise is that you can download and include files using a template similar to this:

```Makefile
_:=$(or $(wildcard $(THE_FILE)), $(shell $(FETCH_FILE_COMMAND)))
include $(THE_FILE)
```

The template above is a [side idea](https://github.com/malcos/makefile-semver/blob/master/support/docs/auto-include.md) from another project.

This project was started during a company wide pet projects day. The results of that day can be seen under the `POC` tag of these projects:

- Original version of this page: [makeutils.github.io](https://github.com/makeutils/makeutils.github.io/tree/POC)
- Bootstrap logic: [makeutil-bootstrap](https://github.com/makeutils/makeutil-bootstrap/tree/POC) (now moved to [core](https://github.com/makeutils/core))
- Demo terraform makeutil module: [makeutil-terraform](https://github.com/makeutils/makeutil-terraform/tree/POC)
