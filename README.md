Spring Boot native demo with Gradle
===================================

For some time now I've been excited about the possibilities of [GraalVM](https://www.graalvm.org)
native image compilation and the revolution it might bring to the JVM ecosystem once the technology
matures just a little bit further. I'm especially interested in the possibilities it might bring
to Spring (Boot) applications, since I love and use this framework both for personal as well as
professional projects.

With the release of version ``0.8.3`` of [Spring Native](https://github.com/spring-projects-experimental/spring-native)
it was yet again time to enjoy a little experimentation time and see how far things have come along. 
I was hard pressed to find a working example that uses Gradle (my favourite build tool) and given
that it is much more fun to set up things yourself anyway, I now share the results of my efforts!

Building and running
--------------------

This is just a plain Spring Boot ``2.4.0`` project, therefore everything should feel familiar if
you know Spring Boot.

We will be using a Docker build (using Paketo buildpacks) to perform the native image compilation.
This is easy to set up and doesn't require you to set up GraalVM yourself.

Building the native image is done as follows, assuming you have Docker up and running:

```shell
./gradlew bootBuildImage
```

Now you should have a Docker image and you can run the application as follows:

```shell
docker run --rm -d -p 8080:8080 spring-boot-native-demo:0.1.0-SNAPSHOT
```

Now, if you haven't modified the project (which I encourage you to do!) there should be a simple
HTTP endpoint accessible at ``/hello``:

```shell
curl http://localhost:8080/hello
```

Also be sure to take a look at the logging, and note the startup time. On my machine I see almost a
50 time increase in startup speed compared to running it using plain OpenJDK. After abusing
the REST endpoint for a while, memory usage remains stable slightly below 35 MiB. Running on 
OpenJDK the same application idles at about a 50 MiB heap size. **NOTE**: these measures are
presented only to get you excited about trying things yourself, they are by no means accurate
benchmarks.

Notes
-----

Some pitfalls I ran into when setting up this project:

- Version ``0.8.3`` of ``spring-graalvm-native`` supports **ONLY** version ``2.4.0``. This same
  warning is present in the Spring project's documentation, but still I managed to think at first
  it was possible to use ``2.4.2``. It isn't.
- As you may know the static analysis performed by GraalVM native image compilation consumes quite
  a bit of memory. This build reported a peak memory usage of 5.16 GiB.

Resources
---------

Some additional resources to get you started with this exciting technology:

- https://spring.io/blog/2020/11/23/spring-native-for-graalvm-0-8-3-available-now
- https://repo.spring.io/milestone/org/springframework/experimental/spring-graalvm-native-docs/0.8.3/spring-graalvm-native-docs-0.8.3.zip!/reference/index.html