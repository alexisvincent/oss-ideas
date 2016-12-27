## Streaming CI Build System
My thoughts on an ideal CI build System.

## Requirements
- Simple
- API based
- Easy to run on a single machine (even within another CI system) and across a cluster
- Interface based design
- Deterministic
- Functional in nature
- Automatic parallelization from specifying tasks and dependencies to form a DAG (Directed Asyclic Graph), <- fancy name for a rooted tree as far as I can see
- tasks as functions on immutable directories (can be layered using a file system like Overlay or AUFS)
- Kick ass UI
- Form tasks into stages
- Tasks are containers
- Programatic API
- Fast
- Stateless

## Core Concepts

### Tasks
Tasks are containers (consider a task being a set of containers with a 'primary' container) and the smallest unit of work, they represent some thing that needs to be done. They can also be thought of as functions.

Tasks yield a directory (its output) of files. Tasks can also specify dependencies on other tasks (function input), the 'output' directories of these dependencies will be mounted in to the container.

Tasks can also affect the outside world (eg. deploy to production or send a notification).

Task outputs are immutable, if multiple tasks depend on a single task they will each get their own copy of the data. Task outputs are also traceable, you can examine the output of a task at any point in the build process. Thus you can examine exactly the output of each task and more easily determine where errors are.

Task outputs are stored as layers. So in the case that task A yields a git repo, and task B downloads dependencies to the repo (task B is effectively a transform of the task A output), the output of task B will be stored as a layer on top of the git repo. This makes storing intermediary assets cheap.

Tasks hint that they are transforms of one of their dependencies by specifying something like 'transform: dep-name'.

Two tasks can transform the same repo, and a third task can be constructed to merge the results of these two tasks (fork and join).

When a tasks dependencies have run, the task will be run parrallel to other tasks that need to be run.

Tasks can also modify a global 'context' that will be passed down to all decendants of a task (so things like env variables can be passed down).

If a task has run previously, it's input and output (from the previous) run, will be mounted in to the container (easy, powerful caching).

Tasks can be marked as 'pure', pure tasks can be memoized (cached) for the same set of inputs (code transforms for instance), meaning they won't be rerun.

Given any build, the system will try reuse as much of the cached tasks as they can.

Tasks can be packaged and shared via a package manager.


#### Sources
A source is a task that can trigger a build, sources can't have dependencies. Sources are things like 'git repos', 'time passing', 'notifications' etc. Essentially event emmiters.

### Stages
A stage is a set of tasks and is meant to help catagorize and control order of execution. Stages also specify dependencies as DAG's. Tasks in a stage will only run if all tasks in any dependency stages has completed.

A 'pipeline' of tasks is thus created and you should be able to inspect it fully. If the pipeline fails at any task, you should be able to rerun it and keep 'good' tasks, just using the cached results.

## Schedulers
The core concepts are a high level abstraction over how to run tasks. And don't dictate the environment that they run on. 

Schedulers are (sharable) plugins that control how tasks are executed and how their dependencies and outputs are distributes. 

For example you could have a 'local' scheduler, which just talks to a docker engine and runs the containers locally and handles volume management by copying directories.
Where as a 'kubernetes' scheduler could hand off task scheduling to the kubernetes native scheduler and run tasks as containers in pods, with a helper container that manages the volumes and distributing the output and input layers around the cluster.

## API Server, UI and CLI
There should be a 'master' process that provides an HTTP API. Thus a UI and CLI, as well as programatic processes, can interact with the system. 

The CLI should be able to kick off individual tasks locally or on a cluster.

**Dashboard** - This is a must have, need some nice visualization that can be displayed on a screen of the build process.

It wont be hard to provide a programatic UI to design your pipeline, which can then just spit out a config file. The UI can also take as input a config and allow you to modify it.

Should also be easy to statically analize the system and provide insight into how the pipeline can be optimized.
