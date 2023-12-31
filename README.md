## Collection of VELD example repos (work in progress!)

On the VELD design, see the living technical concept here:
https://docs.google.com/document/d/1KbHJpWErnhfYhVzBtoimg5R7a_j9T-4RqQynm9To1T4/edit#heading=h.lg83zwmcjpqe

### About

This here is a collection of example repos implementing the VELD design, to showcase and play
around.

Real-life usages of VELD can be found here: https://github.com/steffres/VELD_production_repos

The examples simulate several atomic steps, forming together an overarching ETL pipeline, where data
is fetched from an external resource, processed, validated, and loaded into a database. 

They are designed primarily to:

- be minimalistic
  
  For each kind of functionality a minimalistic set of logic and data shape was chosen to focus on
  the VELD design.
  
- represent real-life requirements

  Allthough the data and logic is simple, the underlying requirements and concepts should be
  somewhat representative of regular data processing. 

- grow gradually in complexity

  With each increasing example number, the complexity of VELD functionality increases too. 

### VELD objects

There are three kinds:

- **data**

  simple, static repos providing data, and also VELD metadata indicating its compatibilties with
  other velds.

- **executable**

  a docker compose service, with arbitrary dockerfile / images / code contexts.  The VELD metadata
  describes what kind of data / executable it is compatible with. Note that while an executable is
  by definition something that can be run on itself, it is usually not meant to be run alone. Rather
  it would likely need some context, e.g. a **data veld** mounted as input. The purpose of an
  **executable veld** is to provide itself as an easily reusable tool.

- **chain**

  Also a docker service, but one that reuses services from **executable velds**, and wires them
  together to **data velds** or even further **executable velds**. The purpose of a **chain veld**
  is to fullfill a concrete task in a reproducible implementation.

Basically, in most cases, a **data veld** veld repo is combined with an **executable veld**, wired
together in a **chain veld**.

### VELD metadata (veld.yaml)

A **data veld** and an **executable veld** is defined by one **veld.yaml** file at the root of its
repo. Each repo thus represents precisely one such veld object.

A **veld chain** is defined in most cases by a **veld.yaml** too at the root, but it might also be
defined by any **.yaml** at the root of a repo which contains a chain definition. Because it is
likely that several distinct chains are used throughout a single project, a more flexible approach
is required. The detailed reasoning and trade-offs of this decision are discussed in the technical
concept.

The metadata for a veld are described in a veld.yaml file, like so:
```
x-veld:
  executable:
    ...
```

And if it's an **executable** or a **chain**, then its service is described as regular docker 
compose service, like so:
```
services:
  veld:
    ...
```

Basically a VELD yaml file:

- **must** contain a section `x-veld`.

  This section informs about the VELD interoperability and communicates its purpose.
  
- **may** contain a section `services`.

  If it can be executed, this makes a VELD yaml file an enriched docker compose file,
  which VELD will always fully adhere to.

### How to play around

The repos were implemented with the following tools and versions. Other versions were not tried out, 
but unless they are ancient, older versions should work nicely too.

- git: `2.34.1`
- docker: `24.0.3`
- docker compose: `v2.20.2`

To clone this repo and all its subrepos recursively, do:
```
git clone --recurse-submodules https://github.com/steffres/VELD_example_repos.git
```

Then go into the repos and look around. Each veld repo contains a description of itself.

If a VELD repo contains a **chain** (or an **executable**), it can be launched as a docker 
service. For this, first build it:

(Note: If you have an older version of docker compose, you might need to call `docker-compose`
instead of `docker compose`)
```
docker compose -f veld.yaml build
```
then run it:
```
docker compose -f veld.yaml up
```
If you use Bash, you might also create a function for both of these commands together for convenience 
(this way whatever you run will always be guaranteed to be built too in one quick command):
```
veld() { docker compose -f $1 build && docker compose -f $1 up; }
```
Then to build and run a veld, you would only need to:
```
veld veld.yaml
```

It is recommended to go through the repos in order by their numbers as they increase with complexity
and are related by forming an overarching ETL task. 

### Write your own velds

#TODO
- desribe mapping issue when host side does not exist
- recommend docker user creation inside container to avoid root execution
