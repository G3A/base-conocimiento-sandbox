PS D:\git_public\base-conocimiento-sandbox> make up
docker compose --env-file .env -f compose.yml -f compose.gpu.yml -f compose.docling-gpu.yml up -d --build
#1 [internal] load local bake definitions
#1 reading from stdin 543B 0.0s done
#1 DONE 0.0s

#2 [internal] load build definition from Dockerfile
#2 transferring dockerfile: 3.27kB done
#2 DONE 0.0s

#3 [internal] load metadata for docker.io/library/eclipse-temurin:25-jre-noble
#3 DONE 0.9s

#4 [internal] load metadata for docker.io/library/eclipse-temurin:25-jdk-noble
#4 DONE 0.9s

#5 [internal] load .dockerignore
#5 transferring context: 298B done
#5 DONE 0.0s

#6 [runtime 1/7] FROM docker.io/library/eclipse-temurin:25-jre-noble@sha256:b4c93a50fc67612798db73d68ca3b0ee4ebdd51736e59cca370e689b9797037e
#6 resolve docker.io/library/eclipse-temurin:25-jre-noble@sha256:b4c93a50fc67612798db73d68ca3b0ee4ebdd51736e59cca370e689b9797037e 0.0s done
#6 DONE 0.0s

#7 [deps 1/6] FROM docker.io/library/eclipse-temurin:25-jdk-noble@sha256:534968c051301957beae735e7ba1db54d99ddecf08746d3b9d4f318cc132dbc3
#7 resolve docker.io/library/eclipse-temurin:25-jdk-noble@sha256:534968c051301957beae735e7ba1db54d99ddecf08746d3b9d4f318cc132dbc3 0.0s done
#7 DONE 0.0s

#8 [runtime 3/7] RUN useradd --system --create-home --uid 10001 kb
#8 CACHED

#9 [runtime 2/7] RUN apt-get update  && apt-get install -y --no-install-recommends ripgrep curl  && rm -rf /var/lib/apt/lists/*
#9 CACHED

#10 [runtime 4/7] WORKDIR /app
#10 CACHED

#11 [internal] load build context
#11 transferring context: 14.19kB 0.0s done
#11 DONE 0.0s

#12 [deps 2/6] WORKDIR /build
#12 CACHED

#13 [deps 5/6] RUN chmod +x mvnw
#13 CACHED

#14 [deps 6/6] RUN --mount=type=cache,target=/root/.m2,id=maven-repo     ./mvnw -B -q       -Daether.connector.basic.downstreamThreads=10       -Daether.metadataResolver.threads=10       -Daether.dependencyCollector.bf.threads=10       dependency:go-offline
#14 CACHED

#15 [deps 3/6] COPY .mvn/ .mvn/
#15 CACHED

#16 [deps 4/6] COPY mvnw pom.xml ./
#16 CACHED

#17 [build 1/2] COPY src/ src/
#17 CACHED

#18 [build 2/2] RUN --mount=type=cache,target=/root/.m2,id=maven-repo     ./mvnw -o -B -q clean package -DskipTests
#18 1.444 WARNING: A terminally deprecated method in sun.misc.Unsafe has been called
#18 1.444 WARNING: sun.misc.Unsafe::staticFieldBase has been called by com.google.inject.internal.aop.HiddenClassDefiner (file:/root/.m2/wrapper/dists/apache-maven-3.9.11/a2d47e15/lib/guice-5.1.0-classes.jar)
#18 1.444 WARNING: Please consider reporting this to the maintainers of class com.google.inject.internal.aop.HiddenClassDefiner
#18 1.444 WARNING: sun.misc.Unsafe::staticFieldBase will be removed in a future release
#18 3.210 [ERROR] Failed to execute goal on project base-conocimiento: Could not resolve dependencies for project co.g3a:base-conocimiento:jar:0.1.0-SNAPSHOT
#18 3.210 [ERROR] dependency: com.google.errorprone:error_prone_annotations:jar:2.33.0 (compile)
#18 3.210 [ERROR]       Cannot access central (https://repo.maven.apache.org/maven2) in offline mode and the artifact com.google.errorprone:error_prone_annotations:jar:2.33.0 has not been downloaded from it before.
#18 3.210 [ERROR] -> [Help 1]
#18 3.210 [ERROR]
#18 3.212 [ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
#18 3.212 [ERROR] Re-run Maven using the -X switch to enable full debug logging.
#18 3.212 [ERROR]
#18 3.212 [ERROR] For more information about the errors and possible solutions, please read the following articles:
#18 3.212 [ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/DependencyResolutionException
#18 ERROR: process "/bin/sh -c ./mvnw -o -B -q clean package -DskipTests" did not complete successfully: exit code: 1
------
 > [build 2/2] RUN --mount=type=cache,target=/root/.m2,id=maven-repo     ./mvnw -o -B -q clean package -DskipTests:
3.210 [ERROR] Failed to execute goal on project base-conocimiento: Could not resolve dependencies for project co.g3a:base-conocimiento:jar:0.1.0-SNAPSHOT
3.210 [ERROR] dependency: com.google.errorprone:error_prone_annotations:jar:2.33.0 (compile)
3.210 [ERROR]   Cannot access central (https://repo.maven.apache.org/maven2) in offline mode and the artifact com.google.errorprone:error_prone_annotations:jar:2.33.0 has not been downloaded from it before.
3.210 [ERROR] -> [Help 1]
3.210 [ERROR]
3.212 [ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
3.212 [ERROR] Re-run Maven using the -X switch to enable full debug logging.
3.212 [ERROR]
3.212 [ERROR] For more information about the errors and possible solutions, please read the following articles:
3.212 [ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/DependencyResolutionException
------
[+] up 0/1
 - Image base-conocimiento-api Building                                                                             4.8s
Dockerfile:36

--------------------

  35 |     # `go-offline` sola no evitaba.

  36 | >>> RUN --mount=type=cache,target=/root/.m2,id=maven-repo \

  37 | >>>     ./mvnw -o -B -q clean package -DskipTests

  38 |

--------------------

failed to solve: process "/bin/sh -c ./mvnw -o -B -q clean package -DskipTests" did not complete successfully: exit code: 1


What's next:
    Debug this Compose error with Gordon → docker ai "help me fix this compose error"
make: *** [Makefile:293: up] Error 1
PS D:\git_public\base-conocimiento-sandbox> make down
docker compose --env-file .env -f compose.yml -f compose.gpu.yml -f compose.docling-gpu.yml down --remove-orphans
PS D:\git_public\base-conocimiento-sandbox> make up-ministral
docker compose --env-file .env -f compose.yml -f compose.gpu.yml -f compose.ministral.yml up -d --build
[+] up 14/14
 ✔ Image quay.io/docling-project/docling-serve-cpu:latest Pulled                                                  124.1s
#1 [internal] load local bake definitions
#1 reading from stdin 543B 0.0s done
#1 DONE 0.0s

#2 [internal] load build definition from Dockerfile
#2 transferring dockerfile: 3.27kB done
#2 DONE 0.0s

#3 [internal] load metadata for docker.io/library/eclipse-temurin:25-jre-noble
#3 DONE 0.5s

#4 [internal] load metadata for docker.io/library/eclipse-temurin:25-jdk-noble
#4 DONE 0.5s

#5 [internal] load .dockerignore
#5 transferring context: 298B done
#5 DONE 0.0s

#6 [runtime 1/7] FROM docker.io/library/eclipse-temurin:25-jre-noble@sha256:b4c93a50fc67612798db73d68ca3b0ee4ebdd51736e59cca370e689b9797037e
#6 resolve docker.io/library/eclipse-temurin:25-jre-noble@sha256:b4c93a50fc67612798db73d68ca3b0ee4ebdd51736e59cca370e689b9797037e 0.0s done
#6 DONE 0.0s

#7 [deps 1/6] FROM docker.io/library/eclipse-temurin:25-jdk-noble@sha256:534968c051301957beae735e7ba1db54d99ddecf08746d3b9d4f318cc132dbc3
#7 resolve docker.io/library/eclipse-temurin:25-jdk-noble@sha256:534968c051301957beae735e7ba1db54d99ddecf08746d3b9d4f318cc132dbc3 0.0s done
#7 DONE 0.0s

#8 [runtime 2/7] RUN apt-get update  && apt-get install -y --no-install-recommends ripgrep curl  && rm -rf /var/lib/apt/lists/*
#8 CACHED

#9 [runtime 3/7] RUN useradd --system --create-home --uid 10001 kb
#9 CACHED

#10 [runtime 4/7] WORKDIR /app
#10 CACHED

#11 [internal] load build context
#11 transferring context: 14.19kB 0.0s done
#11 DONE 0.0s

#12 [deps 3/6] COPY .mvn/ .mvn/
#12 CACHED

#13 [deps 4/6] COPY mvnw pom.xml ./
#13 CACHED

#14 [deps 5/6] RUN chmod +x mvnw
#14 CACHED

#15 [deps 6/6] RUN --mount=type=cache,target=/root/.m2,id=maven-repo     ./mvnw -B -q       -Daether.connector.basic.downstreamThreads=10       -Daether.metadataResolver.threads=10       -Daether.dependencyCollector.bf.threads=10       dependency:go-offline
#15 CACHED

#16 [deps 2/6] WORKDIR /build
#16 CACHED

#17 [build 1/2] COPY src/ src/
#17 CACHED

#18 [build 2/2] RUN --mount=type=cache,target=/root/.m2,id=maven-repo     ./mvnw -o -B -q clean package -DskipTests
#18 1.021 WARNING: A terminally deprecated method in sun.misc.Unsafe has been called
#18 1.021 WARNING: sun.misc.Unsafe::staticFieldBase has been called by com.google.inject.internal.aop.HiddenClassDefiner (file:/root/.m2/wrapper/dists/apache-maven-3.9.11/a2d47e15/lib/guice-5.1.0-classes.jar)
#18 1.021 WARNING: Please consider reporting this to the maintainers of class com.google.inject.internal.aop.HiddenClassDefiner
#18 1.021 WARNING: sun.misc.Unsafe::staticFieldBase will be removed in a future release
#18 2.041 [ERROR] Failed to execute goal on project base-conocimiento: Could not resolve dependencies for project co.g3a:base-conocimiento:jar:0.1.0-SNAPSHOT
#18 2.041 [ERROR] dependency: com.google.errorprone:error_prone_annotations:jar:2.33.0 (compile)
#18 2.041 [ERROR]       Cannot access central (https://repo.maven.apache.org/maven2) in offline mode and the artifact com.google.errorprone:error_prone_annotations:jar:2.33.0 has not been downloaded from it before.
#18 2.042 [ERROR] -> [Help 1]
#18 2.042 [ERROR]
#18 2.044 [ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
#18 2.044 [ERROR] Re-run Maven using the -X switch to enable full debug logging.
#18 2.044 [ERROR]
#18 2.044 [ERROR] For more information about the errors and possible solutions, please read the following articles:
#18 2.045 [ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/DependencyResolutionException
#18 ERROR: process "/bin/sh -c ./mvnw -o -B -q clean package -DskipTests" did not complete successfully: exit code: 1
------
 > [build 2/2] RUN --mount=type=cache,target=/root/.m2,id=maven-repo     ./mvnw -o -B -q clean package -DskipTests:
2.041 [ERROR] Failed to execute goal on project base-conocimiento: Could not resolve dependencies for project co.g3a:base-conocimiento:jar:0.1.0-SNAPSHOT
2.041 [ERROR] dependency: com.google.errorprone:error_prone_annotations:jar:2.33.0 (compile)
2.041 [ERROR]   Cannot access central (https://repo.maven.apache.org/maven2) in offline mode and the artifact com.google.errorprone:error_prone_annotations:jar:2.33.0 has not been downloaded from it before.
2.042 [ERROR] -> [Help 1]
2.042 [ERROR]
2.044 [ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
2.044 [ERROR] Re-run Maven using the -X switch to enable full debug logging.
2.044 [ERROR]
2.044 [ERROR] For more information about the errors and possible solutions, please read the following articles:
[+] up 14/15] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/DependencyResolutionException
 ✔ Image quay.io/docling-project/docling-serve-cpu:latest Pulled                                                  124.1s
 - Image base-conocimiento-api                            Building                                                  3.3s
Dockerfile:36

--------------------

  35 |     # `go-offline` sola no evitaba.

  36 | >>> RUN --mount=type=cache,target=/root/.m2,id=maven-repo \

  37 | >>>     ./mvnw -o -B -q clean package -DskipTests

  38 |

--------------------

failed to solve: process "/bin/sh -c ./mvnw -o -B -q clean package -DskipTests" did not complete successfully: exit code: 1


What's next:
    Debug this Compose error with Gordon → docker ai "help me fix this compose error"
make: *** [Makefile:379: up-ministral] Error 1
