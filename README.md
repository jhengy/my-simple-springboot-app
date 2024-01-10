# Context

- Three demo endpoints are created to observe whether tracing information such as traceId is propagated from controller
  to service layer in SL4J logs
- All logs are formatted to include traceId in [logback-spring.xml](./src/main/resources/logback-spring.xml)

# How to run

- run `ContextPropagationApiTest`
- observe the application logs and the test outcomes

# Observation

- To preserve tracing information in Reactive chain, it requires calling `Hooks.enableAutomaticContextPropagation()`:
  see https://github.com/jhengy/my-simple-springboot-app/commit/92c2925957b7212b166f8f764b641f4df686121a
- If reactive chain uses reactive web client and the chain is invoked using `.subscribe`, it requires adding `
  .contextCapture()` to the reactive
  chain: https://github.com/jhengy/my-simple-springboot-app/commit/b6eae55ec8b310281a222228a175235f7d1dc8c7
- `.contextCapture()` does not work for downstream logs:
  see https://github.com/jhengy/my-simple-springboot-app/commit/b6eae55ec8b310281a222228a175235f7d1dc8c7#diff-1c9e8faf8b007829d6c11cad02e87721b6ecbdeb417584e81000c2ae7fdc55a9R49

# References

- https://github.com/micrometer-metrics/tracing/wiki/Spring-Cloud-Sleuth-3.1-Migration-Guide