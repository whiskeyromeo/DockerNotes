# Notes on Writing Dockerfiles 

## [Best Practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
* Combine RUN commands if possible --> each RUN command adds a new layer
    * from the docs : 
        > "In Docker 1.10 and higher, only RUN, COPY, and ADD instructions create layers." 
* Alphabetize your dependencies
    * In general you should try to do this for all of your files as it greatly simplifies finding
    and changing imports and updating versions.
  
