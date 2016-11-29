---
layout: post
title: 'Jmeter Guide'
tags:
  - jmeter
  - test
category: tool
---
Jmeter is used to do the stress test or load test. Then I will introduce the guide of using jmeter
<!--more-->

## Requirments
JMeter requires affuly compliant JVM 7 or higher
Download the [Apache JMeter](http://jmeter.apache.org/download_jmeter.cgi)。


## Running JMeter
To run JMeter, run the jmeter.bat(for windows) or jmeter(for Linux) file. These files are found in the bin directory. After a short time, the JMeter GUI should apper.There are some additional scripts in the bin directory that you may find useful.

 * jmeter.bat, run JMeter(in GUI mode default)
 * jmeter.cmd, run JMeter without the windows sheel console(in GUI mode by default)
 * jmeter-n.cmd, drop a JMX file on this to run a non-GUI test
 * jmeter-n-r.cmd, drop a JMX file on this to run a non-GUI test remotely
 * jmeter-t.cmd, drop a JMX file on this to load it in GUI mode
 * jmeter-server.bat, start JMeter in server mode
 * mirror-server.cmd, runs the JMeter Mirror Server in non-GUI mode
 * shutdown.cmd, Run the Shutdown client to stop a non-GUI instance gracefully
 * stoptest.cmd, Run the Shutdown client to stop a non-GUI instance abruptly

## Building a Test Plan

### Add a test plan based on template

 * Click File
 * Click templates
 * Select template
 * Click create

### Add a Thread Group
First, add a Thread Group to the Test Plan

 * Right-click on on Test Plan
 * Mouse over Add>
 * Mouse over Threads(Users)>
 * Click on Thread Group

The Thread Group has three particularly important properties influence the load test:

 * Number of Threads (users): The number of users that JMeter will attempt to simulate. Set this to 50
 * Ramp-Up Period (in seconds): The duration of time that JMeter will distribute the start of the threads over. Set this to 10.
 * Loop Count: The number of times to execute the test. Leave this set to 1.

### Add an Http Request Defaults
The HTTP Request Defaults config element is used to set default values for HTTP requests in our test plan, for example domain, port.

 * Select Thread Group, then Right-click it
 * Mouse over Add >
 * Mouse over Config Element >
 * Click on HTTP Request Defaults
 * Set the Server Name or IP and Port

### Add an HTTP Cookie Manager
if your web server uses cookies, you can add support for cookies by adding an HTTP Cookie Manager to the Trhead Group

 * Select Thread Group, then Right-click it
 * Mouse over Add >
 * Mouse over Config Element >
 * Click on HTTP Cookie Manager

### Add an HTTP Request Sampler
Use Http request sample to create stress test case

 * Select Thread Group, then Right-click it
 * Mouse over Add >
 * Mouse over Sampler >
 * Click on HTTP Request
In Http Request Sample, need to set Path  and Method.

### Add a View Results Tree Listener
In JMeter, listeners are used to output the results of a load test. There are a variety of listeners available, and the other listeners can be added by installing plugins. We will use the Tree  

 * because it is easy to get the response.
 * Select Thread Group, then Right-click it
 * Mouse over Add >
 * Mouse over Listener >
 * Click on View Results Tree

## Create the UTC time in BeanShell PreProcessor

    import java.text.SimpleDateFormat;
    import java.util.TimeZone;
    import java.util.Date;

    SimpleDateFormat formatter = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss.S");
    formatter.setTimeZone(TimeZone.getTimeZone("UTC"));
    String utcTime = formatter.format(new Date());

## Create Base64 or SHA256 String

    import javax.crypto.Mac;
    import javax.crypto.spec.SecretKeySpec;
    import java.security.InvalidKeyException;
    import org.apache.commons.codec.binary.Hex;
    import org.apache.commons.codec.binary.Base64;

    String securityKey = "aaa";
    String data ="bbbb";
    Mac mac = Mac.getInstance("HmacSHA256");
    SecretKeySpec secretKeySpec = new SecretKeySpec(securityKey.getBytes(), "HmacSHA256");
    mac.init(secretKeySpec);
    vars.put("base64String", Base64.encodeBase64String(data.getBytes()));
    vars.put("sha256String", Hex.encodeHexString(mac.doFinal(data.getBytes())));

## Using Regular Expression Extrator to get data from response

 * Reference Name: the name is used to store the value
 * Regular Expression: used to match the response, sample: value="(.+?)"
 * Template: $1$
 * Match No: 1
