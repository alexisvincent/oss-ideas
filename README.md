# OSS Ideas
A catalog of open source ideas I'd like to see implemented (but haven't yet had time to explore myself)

## Build Tools

### Distributed Streaming CI System (inspired by gulp)
#### More thoughts [here](./ci.md)
Distributed Streaming CI System, with a focus on the types of tasks normally achieved by config files in systems like Travis.

This comes from a frustration with not easily being able to implement parrallel build dependencies simply across a single machine or cluster.

The idea would be to define a similar API to gulp, with something akin to gulp/vinyl (encapsulating assets running through the build process). The API would allow for parrallel dependent build tasks (single machine or cross cluster, like kubernetes).

### SystemJS Config Generator to allow loading of node_modules (implementation [here](https://github.com/alexisvincent/systemjs-config-builder))
Now that yarn provides deterministic installs, we can leverage this to generate config files for SystemJS (providing an alternative to JSPM). JSPM is an amazing project, but I feel this project could provide a better **onboarding** process then asking developers to introduce a new package manager.

### General purpose pluginable [package manager / build tool / bundler] for the Javascript ecosystem
Similar to Leiningen for Clojure, that would provide mechanisms to:
- initialize a project
- add dependencies
- provide a repl
- build your code
- handle development experience in the browser (ie. hot-reloading, syntax errors, etc.) [similar to lein-figwheel]

Perhaps Yarn JS will fill this gap?

## IDE Tools

### General purpose code analysis / transform system (or even protocol)
This one happens to exist here https://github.com/Microsoft/language-server-protocol.
