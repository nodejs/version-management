## Why should version management be in core?

Environment managers like Python's [venv][]/virtualenv, Ruby's [rvm][], and
Node's [nvm][] make it easy for users to install and configure a runtime's
versions and constituent components. Each virtual environment can support a
specific runtime version and configuration and an independent set of global
modules. In fact, some runtimes, such as [.NET Core][], can bundle and refer to
a specific runtime configuration for each app. To summarize, environment
managers enable developers to:

* install the runtime with a specific configuration.
* change runtime configurations for test and other needs.
* manage runtime and component updates.

[venv]: https://docs.python.org/3/library/venv.html
[rvm]: https://rvm.io/
[nvm]: https://github.com/creationix/nvm
[.NET Core]: https://docs.microsoft.com/en-us/dotnet/articles/core/deploying/index#self-contained-deployments-scd

The usefulness of and demand for environment managers for Node.js is clear from the
many managers already available today, including:

* [nvm](https://github.com/creationix/nvm)
* [nave](https://github.com/isaacs/nave)
* [nodist](https://github.com/marcelklehr/nodist)
* [n](https://github.com/tj/n)
* [nvm-windows](https://github.com/coreybutler/nvm-windows)
* [nvs](https://github.com/jasongin/nvs)
* [installer](https://github.com/nodejs/installer)

A full list including different capabilities and semantics is
[here](https://cdn.rawgit.com/jasongin/cf9f64dd2739d78412bb9410701bf166/raw/69d48e0774f43280763eb7bb7175e930ab3e9f33/NodeVersionManagers.html)
- props to [@jasongin](https://github.com/jasongin).

The number of configuration options, operating environments and major
version releases for Node.js continues to multiply and yet there is no official
way to use multiple of these versions at the same time.

Even though there are version managers available, each of them tackles only some of
these options, and only in certain environments. For example, some work on
Windows, some only on Linux. Some use shell script, some use JavaScript, and
some use Go. Some support different environments per directory/project, and some
only support an environment per shell or user. It would be simpler for
developers to have at least one manager which supported all of Node.js's
supported platforms and potential configurations.

In addition, how runtime configuration is specified and implemented requires
close collaboration and feedback between those responsible for the runtime
itself and those responsible for its manager. For example, if a developer wants
to utilize LibreSSL instead of OpenSSL within an app or module, the runtime
itself must accomodate this and provide a way for runtime managers to determine
and set that configuration. Similarly, if developers want ways to specify
alternate JS runtimes, the runtime itself must support this and provide an
external configuration mechanism. It would be easiest for the runtime and its
manager(s) to be developed in close collaboration.

Finally, while having many unofficial tools and options arguably provides
greater flexibility, it also places a significant burden on downstream
developers, who must choose and learn different tools and their semantics for
different use cases. For example, a developer wanting to test an app on Windows
and Linux must use a different env manager for each. If they encounter trouble,
they must determine whether this particular manager modifies the PATH variable,
symlinks into existing directories, or some other mechanism. Developers would be
well served by at least one environment manager which works and behaves the same
way everywhere.

To achieve the above goals of:

  a) a manager which supports all of Node's supported platforms and configurations;
  b) close collaboration between runtime and runtime manager development; and
  c) better useability for downstream developers,

we should bring together maintainers of these disparate projects and try to build
at least one official Node environment manager with the best ideas and
implementations from each project.

To achieve this, let's designate a repo as a workspace and create an informal or
formal working group to pursue the goals described above. Let's invite
maintainers and stakeholders from existing projects and Node.js core to initial
discussions and meetings and therein agree on goals and roadmap. And then let's
collaborate to refine the result and add more options based on our users' needs.

