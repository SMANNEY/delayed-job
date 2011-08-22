## Background
DJ 2.1 introduced a callback mechanism, allowing jobs to receive notifications at various points in the lifecycle. While useful, this is limited in that the callback methods need to be implemented on a job class, can't be used by PerformableMethod, and have to be implemented/mixed in to each Job class individually.

## Proposal
Global hooks would simplify most callback use cases and allow straightforward integration of third-party tools (Hoptoad, stats tracking, etc) while still providing custom behavior on a per-job basis.

### Configuration
Global hooks are configured at the worker level using a block which receives the job. _(I want them to be defined on Delayed::Job but the backend isn't likely to be set up by the time third-party plugins would need to be hooked up. Anyone have ideas?)_

`Delayed::Worker.hook :name, lambda {|job| ... }`

Multiple blocks can be configured as a chain and will be executed in the order defined.

`Delayed::Worker.hook :name, lambda{|job| puts "First"}`

`Delayed::Worker.hook :name, lambda{|job| puts "Second"}`

Global hooks have the same names as the callback methods on the job class (before, success, after, error, etc).

### Behavior Notes
* job-specific hooks should be invoked before global hooks.

### Questions
* Should global hooks be cancelable by returning false from a job hook?
* Should global hook blocks in the chain be allowed to cancel execution like rails filter chains?

### Comments

(Stephen V.T.) Implement named queues and allow global hooks to be applied to the queue. Then this would allow 3 levels of hooks: Individual job hooks, queue hooks and global hooks (applies to all queues).

(John H.) Want