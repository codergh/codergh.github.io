---
layout: post
title: 'Spring Security'
tags:
  - security
category: spring
---

<h2>1. Http Security</h2>
<p>HttpSecurity is used to do the access controll for Web Application, it can helps to prevent information leaks</p>

<!--more-->

<h3>1.1 Authorize Requests</h3>
<p>We can specify custom reuqirments for our URL by adding multiple children to http.authorizeRequests()</p>

<h2>Core components</h2>
<p>SecurityContextHolder, SecurityContext, Authentication. The most fundamental object is SecurityContextHolder. This is where we store details of the
present security context of the application, which includes details of the principal currently using the
application. The detail information can be get by the following method</br>
((UserDetails)SecurityContextHolder.getContext().getAuthentication().getPrincipal()).getUsername();</p>
<h3>Sumarry</h3>
• SecurityContextHolder, to provide access to the SecurityContext.
• SecurityContext, to hold the Authentication and possibly request-specific security
information.
• Authentication, to represent the principal in a Spring Security-specific manner.
• GrantedAuthority, to reflect the application-wide permissions granted to a principal.
• UserDetails, to provide the necessary information to build an Authentication object from your
application’s DAOs or other source of security data.
• UserDetailsService, to create a UserDetails when passed in a String-based username (or
certificate ID or the like).
