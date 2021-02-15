# kubernetes-fluentbit-multiline-log

It's a multiline log parser fluent-bit config that can be used to parse log4j logs. It addresses the challenges of parsing log4j logs that often contain exception stack traces in them.  

## How does the kubernetes generated application log files look like?
### Application generated logs:
```
2021-02-14 11:31:57.847 DEBUG microservice-deployment-779cd79bd5-tnzn7 - [io-20200-exec-8] ( ) o.s.s.w.a.ExceptionTranslationFilter               : Access is denied (user is anonymous); redirecting to authentication entry point
    org.springframework.security.access.vote.AffirmativeBased.decide(AffirmativeBased.java:84)
    org.springframework.security.access.intercept.AbstractSecurityInterceptor.beforeInvocation(AbstractSecurityInterceptor.java:233)
    org.springframework.security.web.access.intercept.FilterSecurityInterceptor.invoke(FilterSecurityInterceptor.java:123)
    org.springframework.security.web.access.intercept.FilterSecurityInterceptor.doFilter(FilterSecurityInterceptor.java:90)
2021-02-14 11:31:57.855 DEBUG microservice-deployment-779cd79bd5-tnzn7 - [io-20200-exec-8] ( ) o.s.s.w.a.ExceptionTranslationFilter               : Calling Authentication entry point.
```
### How kubernetes converts the logs to json?
```
{"log":"2021-02-14 11:31:57.847 DEBUG microservice-deployment-779cd79bd5-tnzn7 - [io-20200-exec-8] ( ) o.s.s.w.a.ExceptionTranslationFilter               : Access is denied (user is anonymous); redirecting to authentication entry point\n","stream":"stdout","time":"2021-02-14T16:31:57.849388706Z"}
{"log":"org.springframework.security.access.AccessDeniedException: Access is denied\n","stream":"stdout","time":"2021-02-14T16:31:57.849427075Z"}
{"log":"\u0009at org.springframework.security.access.vote.AffirmativeBased.decide(AffirmativeBased.java:84)\n","stream":"stdout","time":"2021-02-14T16:31:57.849432912Z"}
{"log":"\u0009at org.springframework.security.access.intercept.AbstractSecurityInterceptor.beforeInvocation(AbstractSecurityInterceptor.java:233)\n","stream":"stdout","time":"2021-02-14T16:31:57.849438447Z"}
{"log":"\u0009at org.springframework.security.web.access.intercept.FilterSecurityInterceptor.invoke(FilterSecurityInterceptor.java:123)\n","stream":"stdout","time":"2021-02-14T16:31:57.849443899Z"}
{"log":"2021-02-14 11:31:57.855 DEBUG microservice-deployment-779cd79bd5-tnzn7 - [io-20200-exec-8] ( ) o.s.s.w.a.ExceptionTranslationFilter               : Calling Authentication entry point.\n","stream":"stdout","time":"2021-02-14T16:31:57.855373788Z"}
```

## How to use?
Download the application logs that were converted by the kubernetes clusters as json file, place it in the input folder, and update the multi-line.cfg file with the location of the json file. and run the following command:

```
./fluent-bit -c multi-line.cfg
```
