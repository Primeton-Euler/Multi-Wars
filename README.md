# Multi-Wars  
  
  
成勘院需求，多个war部署到同一个tomcat下，在一个容器里运行，DevOps平台是支持的，只是需要自定义Dockerfile（用于编译Docker镜像）。  
  
## Binary Package Structure  
  
根目录下必须有Dockerfile，其他war文件的路径可以自定义，war存放路径不同，Dockerfile的写法便不同，该目录结构仅供参考。
  
`# app.war`  
`# |- Dockerfile`  
`# |- app1.war`  
`# |- app2.war`  
`# |- ...`  
  
## Dockerfile
  
使用DevOps平台提供的Web容器基础镜像来编译应用镜像。DevOps平台提供了如下几种基础镜像：  
  
`euler-registry.primeton.com/tomcat6:1.0.0`  
`euler-registry.primeton.com/tomcat7:1.0.0`  
`euler-registry.primeton.com/tomcat8:1.0.0`  
`euler-registry.primeton.com/jetty8:1.0.0`  
`euler-registry.primeton.com/jetty9:1.0.0`  
  
Dockerfile参考示例：  
  
`FROM euler-registry.primeton.com/tomcat8:1.0.0`  
` `  
`# ADD . ${TOMCAT_HOME}/webapps/`  
` `  
`# ADD app1.war ${TOMCAT_HOME}/webapps/`  
`# ADD app2.war ${TOMCAT_HOME}/webapps/`  
` `  
`# if need unpack`  
`ADD . /tmp/`  
`RUN unzip /tmp/app1.war -d ${TOMCAT_HOME}/webapps/app1 \`  
`&& unzip /tmp/app2.war -d ${TOMCAT_HOME}/webapps/app2 \`  
`&& rm -f /tmp/*.war`  
` `  
  
## About  
  
war1，war2是2个J2EE WEB项目，可以根据项目需要新建任意多个war项目，package项目是对需要部署到同一个WEB应用服务器的war进行打包，自定义Dockerfile文件 [package/Dockerfile](https://github.com/Primeton-Euler/Multi-Wars/blob/master/package/Dockerfile)。  
登陆DevOps平台，新建一个产品和一个版本，在该产品版本下，新建一个组件，类型选择`JavaEE应用`类型，提交示例代码，进行编译打包部署等操作。部署完成后，显示的应用访问地址，你需要加上应用名后缀才能访问。  
  
如果是遵循标准一个WAR部署到一个容器，参考示例项目 [https://github.com/Primeton-Euler/DevOps-Examples](https://github.com/Primeton-Euler/DevOps-Examples)  
  
## Binary Package  
  
如果使用上传介质包 的方式进行产品部署，介质包需要遵循规范，根目录下必须有Dockerfile。其中的多个war文件可以随意放置，但是要确保Dockerfile引用这些war文件的路径正确。  
  
`# app.war`  
`# |- Dockerfile`  
`# |- app1.war`  
`# |- app2.war`  
`# |- ...`  