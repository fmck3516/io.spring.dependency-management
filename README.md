# io.spring.dependency-management
Sample project that demonstrates the impact on `runtimeClasspath` depending on how a version is provided.

- `build.gradle` provides the version explicitly. `commons-logging` is excluded from `runtimeClasspath`
- `build-constraint.gradle` provides the version via constraint. `commons-logging` is _NOT_ excluded from `runtimeClasspath`


`build.gradle`:
```
dependencies {
    implementation 'com.optimizely.ab:core-httpclient-impl:3.10.2'
}
```


`build-constraint.gradle`:
```
dependencies {
    constraints {
        implementation 'com.optimizely.ab:core-httpclient-impl:3.10.2'
    }
    implementation 'com.optimizely.ab:core-httpclient-impl'
}
```

`./gradlew dependencies --configuration runtimeClasspath --b build.gradle`:
```
...
     \--- org.apache.httpcomponents:httpclient:4.5.13
          +--- org.apache.httpcomponents:httpcore:4.4.13 -> 4.4.15
          \--- commons-codec:commons-codec:1.11 -> 1.15
...

```

`./gradlew dependencies --configuration runtimeClasspath --b build-constraint.gradle`:
```
+--- com.optimizely.ab:core-httpclient-impl -> 3.10.2
...
|    \--- org.apache.httpcomponents:httpclient:4.5.13
|         +--- org.apache.httpcomponents:httpcore:4.4.13 -> 4.4.15
|         +--- commons-logging:commons-logging:1.2
|         \--- commons-codec:commons-codec:1.11 -> 1.15
...

```
