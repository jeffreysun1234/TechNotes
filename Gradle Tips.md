# Gradle Tips

## * Best the globle gradle.properties

```
# The Gradle daemon aims to improve the startup and execution time of Gradle.
# When set to true the Gradle daemon is to run the build.
org.gradle.daemon=true

# Specifies the JVM arguments used for the daemon process.
# The setting is particularly useful for tweaking memory settings.
# Default value: -Xmx1024m -XX:MaxPermSize=256m
org.gradle.jvmargs=-Xms512m -Xmx2112m -XX:MaxPermSize=512m -XX:+HeapDumpOnOutOfMemoryError -Dfile.encoding=UTF-8

# When configured, Gradle will run in incubating parallel mode.
# This option should only be used with decoupled projects. More details, visit
# http://www.gradle.org/docs/current/userguide/multi_project_builds.html#sec:decoupled_projects
org.gradle.parallel=true

# Enables new incubating mode that makes Gradle selective when configuring projects.
# Only relevant projects are configured which results in faster builds for large multi-projects.
# http://www.gradle.org/docs/current/userguide/multi_project_builds.html#sec:configuration_on_demand
org.gradle.configureondemand=true
```

## * Changing the Gradle User Home Directory

Gradle uses the directory .gradle in our home directory as the default Gradle user home directory.
To change the Gradle user home directory we can set the environment variable GRADLE_USER_HOME and point it to another directory.

> $ export GRADLE_USER_HOME=/Users/myName/dev/gradle

Reference: [Gradle Goodness: Changing the Gradle User Home Directory](http://mrhaki.blogspot.ca/2010/09/gradle-goodness-changing-gradle-user.html)

## * Get a parameter from gradle.properties

1. Define a parameter in gradle.properties
> Example: GOOGLE_MAPS_API_KEY=AIzaSyDqvjPj18uoo4DgyFEey0dCMhV2UQhmkAI

2. Map the parameter to a string value in build.gradle
> Example: resValue "string", "google_maps_key", (project.findProperty("GOOGLE_MAPS_API_KEY") ?: "")

3. Use the parameter in AndroidManifest.xml
> \<meta-data android:name="com.google.android.geo.API_KEY"
>            android:value="@string/google_maps_key" />

## * Dependency tree

> gradlew -q dependencies app:dependencies --configuration compile
