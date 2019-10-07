# Repo to separately compile jh61b-1.0.jar

This repo pulls out the code for the jh61b-1.0 library
so that it can be separately compiled into a jar that
can then be pulled into other Maven or Ant based Java
project for Gradescope autograding.

See intlist in this same organization for an example of how to use it.

To build the jar, run `mvn package` then find the jar in the `target` directory.
