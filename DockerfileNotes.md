# Notes on Writing Dockerfiles 

## [Best Practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
* Combine RUN commands if possible --> each RUN command adds a new layer
    * from the docs : 
        > "In Docker 1.10 and higher, only RUN, COPY, and ADD instructions create layers." 
* Alphabetize your dependencies and multi line arguments
    * In general you should try to do this for all of your files as it greatly simplifies finding
    and changing imports and updating versions.
* Utilize labels to help with organization/automation/licensing.
    ```
      LABEL com.example.version="1.0.0-alpha"
      LABEL vendor="Occam's Razor Co."
      LABEL com.example.release-date="2018-02-03"
      LABEL com.example.version.is-production="False"
    ```
* Always combine `apt-get -y update && apt-get -y install` into a single RUN command
  
