Instead of using the current plugin system it has been proposed that we use hooks instead. Hooks can be executed in a few ways:

* in-process hooks written in python
* executables stored on the filesystem, run once per hook fire
* long-running processes than communicate with the daemon