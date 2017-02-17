---
layout: post
title: 'Spring Security'
tags:
  - security
category: spring
---

# Http Security
HttpSecurity is used to do the access controll for Web Application, it can helps to prevent information leaks

<!--more-->
## 什么是spring security
Spring security能够为企业级JavaEE软件系统提供全面的安全服务。

## spring security能够干什么

 * 认证（authentication），认证是用来识别用户的级别
 * 授权（authorization），授权是用来判断用户能干什么，也叫访问控制（access controll）

## 认证实现方法
主要通过继承AuthenticationProvider这个类来实现认证， 可以用第三方认证也可以拥有自己的认证逻辑

    package com.config;

    import java.util.ArrayList;
    import org.apache.commons.lang.StringUtils;
    import org.springframework.security.authentication.AuthenticationProvider;
    import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
    import org.springframework.security.core.Authentication;
    import org.springframework.security.core.AuthenticationException;
    import org.springframework.stereotype.Component;

    @Component
    public class CustomAuthenticationProvider implements AuthenticationProvider {

        @Override
        public Authentication authenticate(Authentication authentication) throws AuthenticationException {
            String userName = authentication.getName();
            String password = String.valueOf(authentication.getCredentials().toString());

            // TODO use the credential and authenticate against the third-party system
            if(...) {
                return new UsernamePasswordAuthenticationToken(userName, password, new ArrayList<>());
            }
            return null;
        }

        @Override
        public boolean supports(Class<?> authentication) {
            return UsernamePasswordAuthenticationToken.class.isAssignableFrom(authentication);
        }
    }

将CustomAuthenticationProvider添加到WebSecurityConfigurerAdapter

    package com.config;

    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.context.annotation.Bean;
    import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
    import org.springframework.security.config.annotation.web.builders.HttpSecurity;
    import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
    import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
    import org.springframework.session.web.http.CookieSerializer;
    import org.springframework.session.web.http.DefaultCookieSerializer;

    @EnableWebSecurity
    public class SecurityConfiguration extends WebSecurityConfigurerAdapter {

        @Autowired
        private CustomAuthenticationProvider customAuthenticationProvider;

        @Override
        protected void configure(HttpSecurity http) throws Exception {
        }

        @Override
        protected void configure(AuthenticationManagerBuilder auth) throws Exception {
            auth.authenticationProvider(customAuthenticationProvider);
        }
    }

## 如何实现访问控制（授权）
通过继承WebSecurityConfigurerAdapter来实现自定义的访问控制规则

    package com.config;

    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.context.annotation.Bean;
    import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
    import org.springframework.security.config.annotation.web.builders.HttpSecurity;
    import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
    import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
    import org.springframework.session.web.http.CookieSerializer;
    import org.springframework.session.web.http.DefaultCookieSerializer;

    @EnableWebSecurity
    public class SecurityConfiguration extends WebSecurityConfigurerAdapter {

        @Override
        protected void configure(HttpSecurity http) throws Exception {
            // configure access-control
            http.authorizeRequests()
                    // add matcher to permit all
                    .antMatchers("/css/**.css",
                            "/js/**.js").permitAll()
                    .anyRequest().authenticated()
                .and().formLogin()
                    .loginProcessingUrl("/login")
                    .loginPage("/login")
                    .defaultSuccessUrl("/success")
                    .failureUrl("/login?loginFailure")
                .and().sessionManagement()
                    .maximumSessions(1);
        }
    }


## Authorize Requests
We can specify custom reuqirments for our URL by adding multiple children to http.authorizeRequests()

## Core components
SecurityContextHolder, SecurityContext, Authentication. The most fundamental object is SecurityContextHolder.
This is where we store details of the present security context of the application, which includes details of the principal currently using the
application. The detail information can be get by the following method

    ((UserDetails)SecurityContextHolder.getContext().getAuthentication().getPrincipal()).getUsername();

## Sumarry
* SecurityContextHolder, to provide access to the SecurityContext.
* SecurityContext, to hold the Authentication and possibly request-specific security information.
* Authentication, to represent the principal in a Spring Security-specific manner.
* GrantedAuthority, to reflect the application-wide permissions granted to a principal.
* UserDetails, to provide the necessary information to build an Authentication object from your application’s DAOs or other source of security data.
* UserDetailsService, to create a UserDetails when passed in a String-based username (or certificate ID or the like).
