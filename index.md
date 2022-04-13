# make: utils

This project was kicked off during a company wide pet projects day.

The `make: utils` project is intended to be a proof of concept about dynamically managing boilerplate logic dependencies in your Makefile without the need of actually committing that logic and dependency files into your project.

The project takes inspiration on tools like `gradle`, `maven`, `cargo`, etc.

Goals:

- To have a reliable tool you can depend on for including dependencies into your Makefiles.
- Implement and create a set of useful and dependable make utilities

## Proof of concept

This section is a reminder: This is s a proof of concept that may evolve in the future into a dependable project.

## The Bootstrapping Process

- Manually copy paste a small snippet from the [makeutil-bootstrap](https://github.com/makeutils/makeutil-bootstrap) project into you makefile,
- Download basic definitions for streaming resources form network
- Download core utilities using the basic networking abilities
- Offer import utility you can use immediately in your makefile

The anatomy of a make file during the proof of concept will look like this:

```make
# You logic comes at the very top

COMPILER_VERSION:=3.2.42

goal1:
  compile-file.sh

goal2: goal1
  package-file-sh

# the manual bootstrap goes here
BOOTSTRAP := $(trust the magic)

# including dependencies
include $(call .include,terraform,0.2.1)
```

All variables and calls in the example are just representative and not a current working version.

## .gitignore

The most basic idea is to simply ignore anything starting with `makeutils.*`

## Naming convention

- Project names hosted under the `makeutils` organizations should be prefixed with `makeutil-` followed by the name of the module
- Branch names and tags are used for versioning
- The module logic should be stored in a single file located at the root of the project and should be named `makeutil-$NAME.mak`

Example for `terraform` module version `0.0.1`:

```text
https://github.com/makeutils/makeutil-terraform/blob/0.0.1/makeutil-terraform.mak
```

## Importing modules after manual bootstrapping

You need to have the name and version of the module you want to import, in this example it is going to be `terraform` version `0.0.1`

```make
include $(call .include,terraform,0.0.1)
```

## The TODO's for the proof of concept

- Pages
  - ~~explain what is this project.~~
  - ~~explain the bootstrapping process~~
- Proof of concept
  - ~~create the initial bootstrap project, intended to remain there~~
  - ~~implement manual bootstrap using curl~~
  - ~~propose gitignore strategy~~
  - ~~propose naming convention~~
  - ~~explain how to import a module~~

## Plans for the future, next steps

- auto detect download tool
- load core dynamically
- auto import dependencies using a dependency file
- clean dependencies
- document gitignore strategy
- official naming convention
- create a fancy Logo

## Scraps and notes

Excuse me, simply ignore this

```make
# $(call __MAKEUTIL_FILENAME,$(1),$(2))
# $(call __MAKEUTIL_URL,$(1),$(2))
include $(call .include,bootstrap,master)
```
