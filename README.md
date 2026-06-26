jdeferthrow
===

[![Maven Central](https://img.shields.io/maven-central/v/com.io7m.jdeferthrow/com.io7m.jdeferthrow.svg?style=flat-square)](http://search.maven.org/#search%7Cga%7C1%7Cg%3A%22com.io7m.jdeferthrow%22)
[![Maven Central (snapshot)](https://img.shields.io/maven-metadata/v?metadataUrl=https%3A%2F%2Fcentral.sonatype.com%2Frepository%2Fmaven-snapshots%2Fcom%2Fio7m%2Fjdeferthrow%2Fcom.io7m.jdeferthrow%2Fmaven-metadata.xml&style=flat-square)](https://central.sonatype.com/repository/maven-snapshots/com/io7m/jdeferthrow/)
[![Codecov](https://img.shields.io/codecov/c/github/io7m-com/jdeferthrow.svg?style=flat-square)](https://codecov.io/gh/io7m-com/jdeferthrow)
![Java Version](https://img.shields.io/badge/17-java?label=java&color=e65cc3)

![com.io7m.jdeferthrow](./src/site/resources/jdeferthrow.jpg?raw=true)

| JVM | Platform | Status |
|-----|----------|--------|
| OpenJDK (Temurin) Current | Linux | [![Build (OpenJDK (Temurin) Current, Linux)](https://img.shields.io/github/actions/workflow/status/io7m-com/jdeferthrow/main.linux.temurin.current.yml)](https://www.github.com/io7m-com/jdeferthrow/actions?query=workflow%3Amain.linux.temurin.current)|
| OpenJDK (Temurin) LTS | Linux | [![Build (OpenJDK (Temurin) LTS, Linux)](https://img.shields.io/github/actions/workflow/status/io7m-com/jdeferthrow/main.linux.temurin.lts.yml)](https://www.github.com/io7m-com/jdeferthrow/actions?query=workflow%3Amain.linux.temurin.lts)|
| OpenJDK (Temurin) Current | Windows | [![Build (OpenJDK (Temurin) Current, Windows)](https://img.shields.io/github/actions/workflow/status/io7m-com/jdeferthrow/main.windows.temurin.current.yml)](https://www.github.com/io7m-com/jdeferthrow/actions?query=workflow%3Amain.windows.temurin.current)|
| OpenJDK (Temurin) LTS | Windows | [![Build (OpenJDK (Temurin) LTS, Windows)](https://img.shields.io/github/actions/workflow/status/io7m-com/jdeferthrow/main.windows.temurin.lts.yml)](https://www.github.com/io7m-com/jdeferthrow/actions?query=workflow%3Amain.windows.temurin.lts)|

## Repository Relocation

Development of this project has moved to an
[open-source but not open-contribution](https://sqlite.org/copyright.html#notopencontrib)
model.

Source code and commits will remain publicly available perpetually, but issues
and/or pull requests will be rejected and/or ignored. Additionally, this project
will now only be available via a read-only mirror at:

  https://codeberg.org/io7m-com/jdeferthrow


### Description

The `jdeferthrow` package implements a trivial API for combining multiple
exceptions over a series of statements.

### Usage

```
final var tracker = new ExceptionTracker<IOException>();

try {
  doIO1();
} catch (IOException e1) {
  tracker.addException(e1);
}

try {
  doIO2();
} catch (IOException e2) {
  tracker.addException(e2);
}

try {
  doIO3();
} catch (IOException e3) {
  tracker.addException(e3);
}

tracker.throwIfNecessary();
```

The above code will execute `doIO1`, `doIO2`, and `doIO3`, catching each
exception if any are raised. The `throwIfNecessary` method will throw
whichever of `e1`, `e2`, or `e3` was caught first, with either of the other
two exceptions added to the thrown exception as a _suppressed exception_.

Concretely, if all of `doIO1`, `doIO2`, and `doIO3` throw exceptions, the
`throwIfNecessary` method will throw `e1` with `e2` and `e3` added to `e1`
as suppressed exceptions.

This effectively allows for accumulating exceptions over a range of statements
and then throwing an exception at the end that contains all of the exceptions
that were thrown.

If `addException` was not called, `throwIfNecessary` does nothing.

