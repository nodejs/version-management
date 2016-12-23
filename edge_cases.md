#Use Cases

If you created or are maintaining a version manager, please help identify edge/use cases you've encountered.

## Redeployment 
(by @coreybutler)

In some environments (government, defense, some corporations), the developer environment is locked down. 
To facilitate this, many organizations utilize Active Directory as a source of supervised software installations.

- Difficult/impossible to track usage.
- Silent installers are necessary (must be modifiable by the organization)
- Alternative feed of version availability is necessary to prevent confusion.

## Offline Access Use Case 
(by @coreybutler)

A state education agency managed technical infrastructure for all school districts, many of which are in rural/remote areas. 
Some of them have no internet access, and many have very strained bandwidth, yet they want/need to use newer versions of Node. 
They need a local mirror of Node executables, as well as npm mirrors (optional, but mostly impractical to not have this).

- Difficult/impossible to track usage.
- Completely different update method (manually copy resources to a mirror from a USB drive or other physically transportable media)
- A feed of available versions needs to be accessible in a LAN.

## Roaming Profiles & npm Bloat 
(by @coreybutler)

A company uses roaming profiles,  expecting their node environments to roam with them. Global `node_modules` are a part of this. 
Due to the number of dependencies, the `node_modules` directory was enormous, and “almost redundant”, meaning many versions of
the same module with minor variations. This caused the user a 30 minute bootup time while waiting for ALL node environments to copy
from the server to a new desktop.

- Horrible user experience

## Architecture Awareness 
(by @coreybutler)

Some native dependencies require different versions of libraries depending on the architecture (32 vs 64 bit). Users need a
`node_modules` directory specific to a version of node and it’s architecture.

- 2 global node_modules directories per Node.js version if supporting both 32/64-bit (double the already large footprint).

## Yarn: Missing Information 
(by @coreybutler)

When yarn was released, it was looking for a specific Windows registry key to determine whether Node was installed on the
system. Different version managers approach the registry differently. Since there is no spec answering “how to determine
if Node is installed?”, version managers have provided alternative solutions.

- Lacking an “authoritative” spec stifles integration.

## Confused Community 
(by @coreybutler)

Many people do not understand the differences between version managers. The yarn team [didn’t realize](https://github.com/yarnpkg/yarn/issues/626#issuecomment-252993114)
people were using certain version managers at all. At NINA'16, @williamkapke mentioned that he just assumed nvm and n were the same thing. 
Many people confuse nvm and nvm-windows as being the same functional thing for different operating systems.

- Insufficient and/or unclear public messaging.

_NOTE:_ @marcelklehr put together a concise [list of usage types](https://github.com/nodejs/version-management/issues/4#issuecomment-258567834) that would
make a good starting point to educate the community about the options (or at least make them aware they have a choice).

## Permission Problems (Shims) 
(by @coreybutler)

Using a shim changes the execution context of the node process (I troubleshoot this all the time in [node-windows](https://github.com/coreybutler/node-windows)).
A shim may run under a specific user (root, elevated admin), but it treats node as a child process, which (unless specified) runs 
under the default user context. In other words, it masks the user context.

- Symlinks execute node “directly”, guaranteeing the user context.
- Shims can potentially change the value of `__dirname, process.cwd()`, and user.
- Symlinks on Windows can be hard or soft. This choice effects whether it will work across multiple hard drives on a computer.
