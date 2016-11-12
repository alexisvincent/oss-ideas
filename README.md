# OSS Ideas
A catalog of open source ideas I'd like to see implemented (but haven't yet had time to explore myself)

## Build Tools

### Distributed Streaming CI System (a la gulp)
Distributed Streaming CI System, with a focus on the types of tasks normally achieved by config files in systems like Travis.

This comes from a frustration with not easily being able to implement parrallel build dependencies simply across a single machine or cluster.

The idea would be to define a similar API to gulp, with something akin to gulp/vinyl (encapsulating assets running through the build process). The API would allow for parrallel dependent build tasks (single machine or cross cluster, like kubernetes).

### SystemJS Config Generator to allow loading of node_modules
Now that yarn provides deterministic installs, we can leverage this to generate config files for SystemJS (providing an alternative to JSPM). JSPM is an amazing project, but I feel this project could provide a better **onboarding** process then asking developers to introduce a new package manager.

## IDE Tools

### General purpose code analysis / transform system
A general purpose 'IDE engine' that runs as a daemon, exposing a remote API (tcp socket, unix socket) allowing for code analysis, intelli-sense, syntax highlighting, etc. This would allow IDE/editor vendor's to collaborate on IDE tooling, and for IDE's and editors to focus on the rest of the developer facing interface.
