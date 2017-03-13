# What's mdub?
mdub (`mw` on the command line) is a `mvn` / `mvnw` wrapper. Not to be
confused with the [Maven][] [Wrapper][], `mw` uses the Maven Wrapper on
projects where one is configured, and falls back to use the `mvn` from the
`$PATH` if a wrapper is not available.

[Maven]: http://maven.apache.org/
[Wrapper]: https://github.com/takari/maven-wrapper

# Installation

## Installation with `brew`

TODO

## Installing mdub from source
You will probably want to [install Maven][] first. While this is not
technically necessary if all your projects are using a Maven Wrapper, it is a
good idea to have the latest version of `mvn` available system-wide because
some handy Maven features are available outside the context of an existing
project.

[install Maven]: http://maven.apache.org/install.html

Check out a copy of the mdub repository. Then, either add the mdub `bin`
directory to your `$PATH`, or run the provided `install` command with the
location to the prefix in which you want to install mdub. The default prefix is
`/usr/local`.

For example, to install mdub into `/usr/local/bin`:

```bash
git clone https://github.com/dansomething/mdub.git
cd mdub
./install
```

Note: you may need to run `./install` with `sudo` if you do not have
permission to write to the installation prefix.

## Aliasing the `mvn` command
For maximum fidelity add a `mvn` alias to `mw` to your shell's configuration
file.

Example *bash*:

```bash
echo "alias mvn=mw" >> ~/.bashrc
source ~/.bashrc
```

From now on you can just type `mvn ...` from wherever you are and `mw` takes
care of the rest. Happiness ensues!

# Why mdub?

## The problems with `mvn` and `mvnw`
mdub is a convenience for developers running local Maven commands and addresses
a few minor shortcomings of `mvn` and `mvnw`'s commandline behaviour.
These are known issues, and they are set to be addressed in future versions of
Maven. If you are interested in the discussions surrounding them, check out:


Here are the issues I feel are most important, and the ones mdub attempts to
address:

### You have to provide a relative path to `pom.xml`
If you are using the `mvn` command, and you are not in the same directory as
the `pom.xml` file you want to run, you have to provide `mvn` the path.
Depending on where you happen to be, this can be somewhat cumbersome:

```bash
$ pwd
~/myProject/src/main/java/org/project
$ mvn -f ../../../../../pom.xml compile
```

With `mw`, this becomes:

```bash
$ mw build
```

### You have to provide a relative path to `mvnw`
If you are using `mvnw` and you want to run your build, you need to do
something similiar and provide the relative path to the `mvnw` script:

```bash
$ pwd
~/myProject/src/main/java/org/project/stuff
$ ../../../../../../mvnw compile
```

Again, with `mw` this becomes:

```bash
$ mw build
```

### You have a combination of the above problems
I don't even want to type out an example of this, let alone do it on a
day-to-day basis. Use your imagination.

### Typing `./mvnw` to run the maven wrapper is kind of inconvenient
Even with tab completion and sitting at the root of your project, you have to
type at least `./mvn<tab>`. It gets a bit worse if you happen to have a
`maven.properties` file, and with the Maven wrapper, you have a `mvn`
directory to contend with as well. A simple alias would solve this problem, but
you still have the other (more annoying) issues to contend with.

### You meant to use the project's `mvnw`, but typed `mvn` instead
This can be a problem if the project you are building has customizations to the
Maven wrapper or for some reason is only compatible with a certain version of
Maven that is configured in the wrapper. If you know the project uses Maven,
you may be tempted to just use your own system's Maven binary. This might be
ok, or it might cause the build to break, but if a project has a `mvnw`, it
is a pretty safe bet you should use it, and not whatever Maven distribution you
happen to have installed on your system.

# The `mw` payoff
Anywhere you happen to be on your project, you can run the Maven tasks of your
project by typing `mw <tasks>`, regardless of whether you use the Maven Wrapper
in your project or not.

`mw` works by looking upwards from your current directory and will run the
nearest `pom.xml` file with the nearest `mvnw`. If a `mvnw` cannot
be found, it will run the nearest `pom.xml` with your system's Maven. This
is probably always what you want to do if you are running Maven from within a
project's tree that uses the Maven build system.
