---
layout: post
title: 'SpringBoot(Thymeleaf)'
tags:
  - springboot
  - thymeleaf
category: java
---
Thymeleaf是一个很好用的模板引擎， Thymeleaf有很多的功能，下面我们只介绍怎么在spring boot中使用 thymeleaf。
<!--more-->

## 依赖配置
Spring boot本身自带有autoconfiguration， 它能够跟据依赖jar包中的类名来启动各个功能的自动配置， 因此在spring boot中使用thymeleaf的第一步就是引入jar包依赖。
下面是在Gradle中引入thymeleaf

    compile group: 'org.thymeleaf', name: 'thymeleaf', version: '3.0.0.RELEASE'
    compile group: 'org.thymeleaf', name: 'thymeleaf-spring4', version: '3.0.0.RELEASE'
    compile group: 'nz.net.ultraq.thymeleaf', name: 'thymeleaf-layout-dialect', version: '2.0.4'

## 如何在HTML中使用
很简单， 只需要在HTML中加入命名空间就可以使用thymeleaf啦

    <html xmlns="http://www.w3.org/1999/xhtml" xmlns:th="http://www/thymeleaf.org">

## Mapping
如何在Controller中通过返回值匹配到模板文件呢？ 很简单， 只需要在`/resources/templates/`文件夹下面建立对应名字的html文件就可以了。例如：

    @RequestMapping("login")
    public String login(HttpServletRequest request, HttpSession session) {
        return "login";
    }

然后创建文件`/resources/templates/login.html`。当你访问login Controller的时候， 系统就会默认根据login.html模板生成HTML文集那然后返回给浏览器。关于Thymeleaf在HTML文件中的语法， 请参考[Thymeleaf](http://www.thymeleaf.org/documentation.html)。

## 配置Thymeleaf的Text模板引擎来生成邮件内容
需要通过声明java类来进行配置。请参考下例。

    @Configuration
    public class ThymeleafMailConfig extends WebMvcConfigurerAdapter{

    public static final String ENCODING = "UTF-8";

    /**
     * Template engine for TEXT email template
     *
     * @return {@link TemplateEngine}
     */
        @Bean
        public TemplateEngine textTemplateEngine() {
            SpringTemplateEngine templeateEngine = new SpringTemplateEngine();

            ClassLoaderTemplateResolver templateResolver = new ClassLoaderTemplateResolver();
            // Set the template path to /resource/mail/
            templateResolver.setPrefix("/mail/");
            // Set the file type to txt
            templateResolver.setSuffix(".txt");
            templateResolver.setTemplateMode(TemplateMode.TEXT);
            templateResolver.setCharacterEncoding(ENCODING);
            templateResolver.setCacheable(false);
            templeateEngine.setTemplateResolver(templateResolver);
            return templeateEngine;
        }
        public String getTextMail(String subject)
                throws MessagingException, IOException {
            // Prepare the evaluation context
            Context thymeleafContext = new Context();
            // Set variable used in the template
            thymeleafContext.setVariable("name", "Henry");
            thymeleafContext.setVariable("subscriptionDate", new Date());
            thymeleafContext.setVariable("hobbies", Arrays.asList("Cinema", "Sports", "Music"));

            // Create the plain TEXT body using Thymeleaf, the first parameter is the template name
            String mailContent = textTemplateEngine.process("<template-name>", thymeleafContext);
            System.out.println(mailContent);

            return mailContent;
        }
    }
