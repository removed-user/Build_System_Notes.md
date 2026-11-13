# F TIER
# Reinventing the wheel
meson is not only unnecessary, but also worse than all of its predecessors
## Problems

### Argument parsing

"A" Problem with meson is that it only allows you to specify the build and source dir through positional arguments... 

Which it is... absolutely fucking trash at handling?!
- will literally swap them around, if run through a shell script for no "good" fucking reason.
    - No I didn't write it incorrectly check the positional parameters "${@:2}", "${@:4}", ARGV[x], etc...

Meson fails so hard that Archlinux users wrote a custom exec snippet to fix it, and then meson released a patch - which broke that exec script too. 
Then they released an arch specific tool, with its own issues. 

### Solutions?
Very first thing any dev thinks, idk... use a config file?
Serialize configuration; brilliant - That usually helps

There is a way to read your environment into a sort of config, called a "native file".
Which is...
   - not `json`
   - not `INI`
   - not `toml`
   - not `k-v` pair
   - somewhat `toml-adjacent`
An odd amalgamation of toml and json?
This configuration file is technically experimental?

### Ideologically flawed at its core
Meson disrespects the last 35 years of computer science...
Where we used this common mechanism called your environment... to configure things globally.
That is actually the entire point, purpose, and concept of the environment.

It's a rather advanced computing idea, where you... have any discipline whatsoever?
Meson has zero respect for your environment flags or arguments. 
Which is - intentional, that's considered a benefit.

This is the intellectual equivalent of: 
Sometimes you press the wrong button on your remote.
So you throw away the remote, to prevent that from happening.

The idea being:
    - to limit the ability to break a build
The result being: 
    - a limited ability to configure the build
    - Build still breaks from meson quirks, such as:
     - Failing to find your C compiler
     - Improper assessment of C compilers abilities
     - Inability to use an exec wrapper/proxy as compiler
     - Failing to interpret your config/native-file (because configuration is experimental?)
     - Failing to parse the order of arguments properly if executed through shell-wrapper
     - Changing it's argument structure every time you get used to/create a script to work around it's errors

### Conclusion
Unfortunately many projects and maintainers have moved toward meson for "some reason I don't understand"? 
> It's to conform to Enterprise Build Pipelines

Seemingly a motivated, paid, joint effort from... RHEL, google, Microsoft, Facebook etc.

To replace the "ugly tool", that works beautifully (Auto-tools, Makefiles) 

With a "pretty tool" that can't handle basic positional arguements reliably, and where reading serialized configuration... is somehow an experimental feature?!

(Oh, but at least it has the fancy feature of "wrapping a project" and downloading dependencies automatically... Which is?... 
   - Unnecessary+violates single responsibility or
   - not the responsibility of a build system? 
   - already handled by git
   - bloat

So it can... fail to build those properly too

So, now, if you want to use a good build system, you've got to parse the meson build files, to convert it to something worth using. 

Not don't get me wrong, meson has something going for it... it can... execute your c compiler, the bare minimum requirement of a build system?

So, sometimes it does result in successful builds. 

But in every single case you are relying entirely on the author of the meson.build, to...
- know what their doing?
- Remember to check your CFLAGS, LDFLAGS, MALEFLAGS, etc.
- Actually respect any of those settings.
- link against the libraries you want
- use reasonable flags, and 
- not inject flags that disable warnings

Ultimately meson tries to solve problem of build-configuration... 
By preventing users from configuring the build.

Restricting access to any reasonably meaningful way to configure the build other than keeping your own build file in version control.

Which degrades the (universal, single source of truth), that "unix-like" sysadmins, and developers have relied on for decades.
