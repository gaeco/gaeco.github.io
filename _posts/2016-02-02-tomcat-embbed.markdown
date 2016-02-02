---
layout: post
comments: true
title:  "Ebedded Tomcat on Intellij C"
date:   2016-02-02 23:45:09 +0900
categories: jekyll update
---




* all codes : https://github.com/gaeco/tomcatEmbedded

* gradle.build
{% highlight groovy %}
dependencies {
    def tomcatVersion = '8.0.21'

    compile "org.apache.tomcat:tomcat-catalina:${tomcatVersion}"
    compile "org.apache.tomcat:tomcat-coyote:${tomcatVersion}"
    compile "org.apache.tomcat:tomcat-jasper:${tomcatVersion}"

    testCompile group: 'junit', name: 'junit', version: '4.11'
}
{% endhighlight %}


* TomcatServer.java
{% highlight java %}

    private void init(){
        tomcat = new Tomcat();
        tomcat.setPort(port);
        if(contextPath == null){
            contextPath = "";
        }
        if(docBase == null){
            File base = new File(System.getProperty("java.io.tmpdir"));
            docBase = base.getAbsolutePath();
        }
        rootContext = tomcat.addContext(contextPath, docBase);
    }

    public void addServlet(String servletName, String uri, HttpServlet servlet){
        Tomcat.addServlet(this.rootContext, servletName, servlet);
        rootContext.addServletMapping(uri, servletName);
    }

    public void startServer() throws LifecycleException {
        tomcat.start();
        tomcat.getServer().await();
    }

{% endhighlight %}
