---
layout: post
title: 'SpringBoot(Configuration)'
tags:
  - springboot
  - spring
  - java
category: java
---
Spring boot is an opinionated library that allows to create executable Srping application with a convention over configuration approach.

<!--more-->

The magic behid this framework lies in the `@SpringBootApplication` annotation, which will automatically load all the beans the application requires depending on what Srping boot finds in the classpath.

# Debug Spring Boot Auto-configuration
You can start your application in debug mode with either the `-Ddebug` flag or add the property `debug=true` to `applicatioin.properties`.
The Spring boot auto-configuration tries its best to do the right thing, but sometimes things fail and it can be hard to tell why. There is a really useful `ConditionEvaluationReport` available in any Spring Boot Application Context. You will see it if you start your application in debug mode.

When launched in debug mode, Spring Boot will generate a report that likes this one:

    AbstractThymeleafViewResolverConfiguration#thymeleafViewResolver matched:
      - @ConditionalOnProperty (spring.thymeleaf.enabled) matched (OnPropertyCondition)
      - @ConditionalOnMissingBean (names: thymeleafViewResolver; SearchStrategy: all) did not find any beans (OnBeanCondition)

   AopAutoConfiguration matched:
      - @ConditionalOnClass found required classes 'org.springframework.context.annotation.EnableAspectJAutoProxy', 'org.aspectj.lang.annotation.Aspect', 'org.aspectj.lang.reflect.Advice' (OnClassCondition)
      - @ConditionalOnProperty (spring.aop.auto=true) matched (OnPropertyCondition)

   AopAutoConfiguration.JdkDynamicAutoProxyConfiguration matched:
      - @ConditionalOnProperty (spring.aop.proxy-target-class=false) matched (OnPropertyCondition)

# How does Spring boot auto configure the needed feature
Spring boot will load the beans and execute the bean automatically when it finds the class in the classpatch. So if you want to automatically configure the mysql feature, you just include the jdbc jar to your project classpath.

# @SpringBootApplication
The annountion of `@SpringBootApplication` contains the feature of `@Configuration`, `@EnableAutoConfiguration` and `ComponentScan`.
So there is some advice for scanning your custom pacakge, one is replace `@SpringBootApplication` with the three configuration, then custom your own scan rule.

# The `@Conditional*` annotations
The power of Spring Boot lies in one of the Spring 4 new features: the `@Conditional` annotations, which will enable some configuration only if a specific confition is met.

 * `@ConditionalOnBean`
 * `@ConditionalOnClass`
 * `@ConditionalOnExpression`
 * `@ConditionalOnMissingBean`
 * `@ConditionalOnResource`
 * `@ConditionalOnMissingClass`
 * `@ConditionalOnNotWebApplication`
 * `@ConditionalOnWebApplication`
