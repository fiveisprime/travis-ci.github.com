---
title: Building a Scala project
layout: en
permalink: scala/
---

### What This Guide Covers

This guide covers build environment and configuration topics specific to Scala projects. Please make sure to read our [Getting Started](/docs/user/getting-started/) and [general build configuration](/docs/user/build-configuration/) guides first.

## Overview

Travis CI environment provides a large set of build tools for JVM languages with [multiple JDKs, Ant, Gradle, Maven](/docs/user/languages/java/#Overview) and [sbt](http://www.scala-sbt.org).

## Projects using sbt

If your project has `project` directory or `build.sbt` file in the repository root, Travis Scala builder will use `sbt` to build it.

Thanks to [paulp/sbt-extras](https://github.com/paulp/sbt-extras) the sbt version of your project is dynamically detected and used.

### Choosing Scala versions to test against

Thanks to sbt ability to perform actions against multiple Scala versions, it is possible to test your projects against different Scala versions. To specify Scala versions you want your project to be tested against, use the `scala` key, for example:

    language: scala
    scala:
       - 2.9.3
       - 2.10.2
       - 2.10.3-RC3

### Default Test Command

By default, Travis will use

    sbt ++$TRAVIS_SCALA_VERSION test

to run your test suite. This can be overridden as described in the [general build configuration](/docs/user/build-configuration/) guide.

### Dependency Management

Because Travis Scala builder assumes sbt dependency management is used by default, it naturally will pull down project dependencies before running tests without any effort on your side.

### Custom sbt Arguments

Most of the time, Travis default [SBT](https://github.com/travis-ci/travis-cookbooks/blob/master/ci_environment/sbt/templates/default/sbtopts.erb) and [JVM](https://github.com/travis-ci/travis-cookbooks/blob/master/ci_environment/sbt/templates/default/jvmopts.erb) options should work fine.

If needed, you can override SBT and JVM options in [many different ways](https://github.com/paulp/sbt-extras#sbt--h) by passing extra arguments to `sbt`.
For example

    script:
      - sbt -jvm-opts travis/jvmopts.compile ... compile
      - sbt -jvm-opts travis/jvmopts.test ... test

will then run `compile` and `test` with different JVM parameters.

With `sbt_args` key in your `.travis.yml`, you also can specify [extra arguments](https://github.com/paulp/sbt-extras#sbt--h) to be passed to the default build script.
For example

    sbt_args: -no-colors -J-Xss2m

will generate

    script: sbt -no-colors -J-Xss2m ++$TRAVIS_SCALA_VERSION test


## Projects Using Gradle, Maven or Ant

If your project is not configured for sbt, the build process behaves like a typical [Java Project](/docs/user/languages/java).

## Testing Against Multiple JDKs

As for any JVM language, it is also possible to [test against multiple JDKs](/docs/user/languages/java/#Testing-Against-Multiple-JDKs).

## Build Matrix

For Scala projects, `env`, `scala`, and `jdk` can be given as arrays
to construct a build matrix.

## Examples

* [twitter/scalding](https://github.com/twitter/scalding/blob/master/.travis.yml)
* [twitter/summingbird](https://github.com/twitter/summingbird/blob/master/.travis.yml)
* [novus/salat](https://github.com/novus/salat/blob/master/.travis.yml)
* [scalaz/scalaz](https://github.com/scalaz/scalaz/blob/scalaz-seven/.travis.yml)
* [spray/spray](https://github.com/spray/spray/blob/master/.travis.yml) (using a custom [`.jvmopts`](https://github.com/spray/spray/blob/master/.jvmopts) to override Travis defaults)

