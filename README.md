## Collection of VELD example repos (work in progress!)

On the VELD design, see the living technical concept here:
https://docs.google.com/document/d/1KbHJpWErnhfYhVzBtoimg5R7a_j9T-4RqQynm9To1T4/edit#heading=h.lg83zwmcjpqe

### about

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

  With each increasing example number, the complexity of functionality increases too. Te repos
  simulate a data processing task, where data is first load, then converted, validated, and
  ultimately loaded into database.

### VELD objects

There are three kinds:

- **data**

  simple, static repos providing data, and also VELD metadata indicating its compatibilties with
  other velds.

- **executable**

  a docker compose service, with arbitrary dockerfile / images / code contexts.  The VELD metadata
  describes what kind of data / executable it is compatible with. Note that while an executable is
  by definition something that can be run on itself, it is usually not meant to be run alone Rather
  it would likely need some context, like what data to be fed or where to store output, etc.

- **chain**

  Also a docker service, but one that reuses services from executables and wires it together to data
  sets or other executables. 

Basically, in most cases, a **veld data** repo is combined with a **veld executable**, wired
together in a **veld chain**.

Since all of those VELD objects are git repositories, all their states and execution environments
are persisted and made reproducable.

### VELD metadata (veld.yaml)

A **veld data** and a **veld executable** is defined by one **veld.yaml** file at the root of its
repo. Each repo thus represents precisely one such veld object.

A **veld chain** is defined in most cases by a **veld.yaml** too at the root, but it might also be
defined by any **.yaml** at the root of a repo which contain a chain definition. Because it is
likely that several distinct chains are used throughout a project, a more flexible approach is
required. The detailed reasoning and trade-offs of this decision are discussed in the technical 
concept. It is advised however that whenever possible to also use a single **veld.yaml** for a
single chain of a single repo (as most examples do in this collection).

### how to play around

The repos were implemented with these tools and these versions. Other versions were not tried out.

- git: `2.34.1`
- docker: `24.0.3`
- docker compose: `v2.20.2`

To clone this repo and all its subrepos recursively, do:
```
git clone --recurse-submodules git@github.com:steffres/VELD_example_repos.git
```

Then go into the repos and look around. Each veld repo contains a description of itself.

It is recommended to go through the repos in order by their numbers as they increase with complexity
and are related by forming an overarching ETL task. 

