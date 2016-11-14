---
layout: post
title: Learning Spring
---

Create a scheduled task
@Scheduled(fixedRate = 5000)
	the scheduled annotation defines when a particular method runs. NOTE: THis example uses fixedRate, which specifies the interval
between method invocations measured from the start time of each invocation. There are other options, like fixedDelay, which specifies
the interval betwween invocations measured from the comletion of the task. You can also use @Scheduled(cron=". . .") expressions from
more sophisticated task scheduling

<!--more-->

Detect the device type
	The way to detect the device type is to including the Spring Mobile dependency, by including the dependency, spring boot configur
-es a DeviceResolverHandlerInterceptor and DevicehandlerMethodArgumentResolver automatically. DeviceResolverHandlerInterceptor examin
-es the User_Agent header in the incoming request, and based on the header value, determines whether the request is coming from a nor
-mal (destop) browser, or a table browser, the DeviceHandlerMethodArgumentResolver allows Spring MVC to use the resolved Device objec
-t in a controller method.
