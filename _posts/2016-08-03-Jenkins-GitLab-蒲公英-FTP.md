---
layout: post
title: "iOS持续集成 Jenkins+GitLab+蒲公英+FTP"
date: 2016-08-03
author: "Alpaca"
subtitle: "学习笔记"
catalog: true
categories: ios
tags:
   - Jenkins
   - iOS
   - 开发

---

### 什么是持续集成

> 
> 持续集成是一种软件开发实践，即团队开发成员经常集成它们的工作，通过每个成员每天至少集成一次，也就意味着每天可能会发生多次集成。每次集成都通过自动化的构建（包括编译，发布，自动化测试）来验证，从而尽早地发现集成错误。
> 


### 为什么使用持续集成

> 
> 1.减少风险
> 2.减少重复过程
> 3.任何时间、任何地点生成可部署的软件
> 4.增强项目的可见性
> 


### 常用的持续集成工具

---

- [Jenkins CI](http://jenkins-ci.org)
- [Travis CI](https://travis-ci.com/)
- [Hudson CI](http://hudson-ci.org/)
- [Circle CI](https://circleci.com/)


市面上的持续集成工具有很多，考虑到Jenkins的稳定性，我们还是选择以Jenkins来开始iOS的持续集成。

好吧，接下来就正式开始搭建iOS持续集成平台了。

### Jenkins的安装

---

在Mac环境下，我们需要先安装JDK，然后在Jenkins的[官网](http://jenkins-ci.org)下载最新的war包。
下载完成后，打开终端，进入到war包所在目录，执行以下命令：

`java -jar jenkins.war --httpPort=8888`

httpPort指的就是Jenkins所使用的http端口，这里指定8888，可根据具体情况来修改。待Jenkins启动后，在浏览器页面输入以下地址:

`http://localhost:8888`

这样就打开Jenkins管理页面了。

### Jenkins的配置

---

- 
**安装GitLab插件**
因为我们用的是GitLab来管理源代码，Jenkins本身并没有自带GitLab插件，所以我们需要依次选择**系统管理**->**管理插件**，在“**可选插件**”中选中“**GitLab Plugin**”和“**Gitlab Hook Plugin**”这两项，然后安装。

- 
**安装Xcode插件**
同安装GitLab插件的步骤一样，我们依次选择**系统管理**->**管理插件**，在“**可选插件**”中选中“**Xcode integration**”安装。

- 
**安装签名证书管理插件**
iOS打包内测版时，需要发布证书及相关签名文件，因此这两个插件对于管理iOS证书非常方便。还是在**系统管理**->**管理插件**，在“**可选插件**”中选中“**Credentials Plugin**”和“**Keychains and Provisioning Profiles Management**”安装。

- 
**安装FTP插件**
在**系统管理**->**管理插件**，在“**可选插件**”中选中“**Publish over FTP**”安装。

- 
**安装脚本插件**
这个插件的功能主要是用于在build后执行相关脚本。在**系统管理**->**管理插件**，在“**可选插件**”中选中“**Post-Build Script Plug-in**”安装。



好了，插件安装完，可以正式开始自动化构建了！！！

### 自动化构建

---

在Jenkins中，所有的任务都是以“**item**”为单位的。接下来我们就新建一个iOS的项目来开始自动化构建。点击“**新建**”，输入item的名称，选择“**构建一个自由风格的软件项目**”，然后点击“**OK**”。

<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-08-03_15:24:41.jpg" alt="" class="shadow"/>


然后按下图设置构建信息，

<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-08-03_15:25:22.jpg" alt="" class="shadow"/>


**源码管理：**
这里用到的是GitLab，先需要配置SSH，我们可以在Jenkins的证书管理中添加SSH。在Jenkins管理页面，选择“**Credentials**”，然后选择“**Global credentials (unrestricted)**”，点击“**Add Credentials**”，如下图所示，我们填写自己的SSH信息，然后点击“**Save**”，这样就把SSH添加到Jenkins的全局域中去了。

<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-08-03_15:25:41.jpg" alt="" class="shadow"/>


接下来，我们再回到刚刚新建的任务中，在**源码管理**中，选择Git，按下图填好相关信息。PS：**Credentials不需要选择。**

<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-08-03_15:25:59.jpg" alt="" class="shadow"/>


**构建触发器设置**
因为此教程不涉及自动测试这块的流程，所以不需要设置触发器。(以后会有另外的自动测试教程^_^)

**构建环境设置**
iOS打包需要签名文件和证书，所以这部分我们勾选“**Keychains and Code Signing Identities**”和“**Mobile Provisioning Profiles**”。

这里我们又需要用到Jenkins的插件，在系统管理页面，选择“**Keychains and Provisioning Profiles Management**”。

<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-08-03_15:26:27.jpg" alt="" class="shadow"/>


进入**Keychains and Provisioning Profiles Management**页面，点击“**浏览**”按钮，分别上传自己的keychain和证书。
上传成功后，我们再为keychain指明签名文件的名称。点击“**Add Code Signing Identity**”，最后添加成功后如下图所示：


<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-08-03_15:26:49.jpg" alt="" class="shadow"/>

这样我们的Adhoc证书和签名文件就已经在Jenkins中配置好了，接下来我们只需要在item设置中指定相关文件即可。

回到我们新建的item，找到**构建环境**，按下图选好自己的相关证书和签名文件。


<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-08-03_15:27:06.jpg" alt="" class="shadow"/>

**Xcode配置**
点击“**增加构建步骤**”，选择“**Xcode**”。
依次按下图填写项目信息：


<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-08-03_15:27:22.jpg" alt="" class="shadow"/>

<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-08-03_15:28:25.jpg" alt="" class="shadow"/>

<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-08-03_15:28:35.jpg" alt="" class="shadow"/>

**脚本设置**
我们没有勾选“**Pack application and build.ipa**”的原因是，Jenkins的Xcode插件不支持Mac10.10以上的打包了。所以，我们需要用脚本来自己实现iOS打包。
仍然是点击“**增加构建步骤**”，选择“**Execute Shell**”。
输入下列脚本：
`if [ -d "${WORKSPACE}/builds" ]; then rm -rf ${WORKSPACE}/builds; fi;
mkdir ${WORKSPACE}/builds;
if [ -d "${WORKSPACE}/builds/${BUILD_NUMBER}" ]; then rm -rf ${WORKSPACE}/builds/${BUILD_NUMBER}; fi;
mkdir ${WORKSPACE}/builds/${BUILD_NUMBER};
xcodebuild -project ${WORKSPACE}/testForiOS/testForiOS.xcodeproj -scheme "testForiOS" -sdk iphoneos archive -archivePath ${WORKSPACE}/builds/${BUILD_NUMBER}/archive CODE_SIGN_IDENTITY="iPhone Distribution: xxxxxxx"
xcodebuild -exportArchive -exportFormat IPA -archivePath ${WORKSPACE}/builds/${BUILD_NUMBER}/archive.xcarchive -exportPath ${WORKSPACE}/builds/${BUILD_NUMBER}/${JOB_NAME}_${BUILD_NUMBER}.ipa -exportProvisioningProfile "XC Ad Hoc: com.xxxxx.xxx"`

这样我们就简单的实现了自动打包的所有配置了。

不过，当iOS应用打包好后，我们还想发给其他相关人员安装，包括公司内部的，外网的，都需要。这时我们还需配置OTA服务和内网FTP。

外网安装App我们需要用到现在市面上比较流行的免费平台，**[蒲公英](http://www.pgyer.com/)**。在上面蒲公英官网设置相关信息后，我们可以写一个简单的脚本，来实现App打包后，上传到蒲公英和公司内网以及邮件提醒相关人员这一系列操作。

我们先用Jenkins的插件配置FTP信息。进入系统管理页面，选择**系统设置**，找到“**Publish over FTP**”，按下图填好相关信息：


<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-08-03_15:28:51.jpg" alt="" class="shadow"/>

回到任务配置页面，点击“**增加构建后操作步骤**”，然后选择“**Send build artifacts over FTP**”,填写：


<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-08-03_15:29:04.jpg" alt="" class="shadow"/>

这样FTP服务就配置好了。

接下来我们再点击“**增加构建后操作步骤**”，选择“**Execute a set of scripts**”，如下图所示：


<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-08-03_15:29:18.jpg" alt="" class="shadow"/>

所以配置都已经设置好，点击“**保存**”，好了，我们可以试试点击“**立即构建**”按钮了。

**SUCCESS！！！**

以上就是iOS持续集成的简单内容，教程中暂未涉及到自动测试，以后会推出自动测试的内容，未完待续。。。。

    附(脚本内容)：
    #coding=utf-8
    import time
    import urllib2
    import time
    import json
    import mimetypes
    import os
    import smtplib
    from email.MIMEText import MIMEText
    from email.MIMEMultipart import MIMEMultipart
    import json
    
    #蒲公英应用上传地址
    url = 'http://www.pgyer.com/apiv1/app/upload'
    #蒲公英提供的 用户Key
    uKey = 'xxxx'
    #上传文件的文件名（这个可随便取，但一定要以 ipa 结尾）
    file_name = 'xxxx.ipa'
    #蒲公英提供的 API Key
    _api_key = 'xxxxx'
    #安装应用时需要输入的密码，这个可不填
    installPassword = '123456'
    
    # 运行时环境变量字典
    environsDict = os.environ
    #此次 jenkins 构建版本号
    jenkins_build_number = environsDict['BUILD_NUMBER']
    
    #项目名称，用在拼接 tomcat 文件地址
    project_name = 'xxxx'
    #ipa 文件在 tomcat 服务器上的地址
    ipa_file_tomcat_http_url = 'ftp://192.168.10.4/jenkins/iOS/' + jenkins_build_number + '/' + project_name +'_' + jenkins_build_number + '.ipa'
    
    #获取 ipa 文件路径
    def get_ipa_file_path():
    #工作目录下面的 ipa 文件
    ipa_file_workspace_path = '/Users/Shared/Jenkins/Home/jobs/' + project_name + '/workspace/builds/' + jenkins_build_number + '/' + project_name + '_' + jenkins_build_number + '.ipa'
    #tomcat 上的 ipa 文件
    ipa_file_tomcat_path = '/usr/local/tomcat/webapps/' + project_name + '/static/' + jenkins_build_number + '/' + jenkins_build_number + '.ipa'
    
    if os.path.exists(ipa_file_workspace_path):
        return ipa_file_workspace_path
    elif os.path.exists(ipa_file_tomcat_path):
        return ipa_file_tomcat_path
    
    #ipa 文件路径
    ipa_file_path = get_ipa_file_path()
    print ipa_file_path
    #请求字典编码
    def _encode_multipart(params_dict):
    boundary = '----------%s' % hex(int(time.time() * 1000))
    data = []
    for k, v in params_dict.items():
        data.append('--%s' % boundary)
        if hasattr(v, 'read'):
            filename = getattr(v, 'name', '')
            content = v.read()
            decoded_content = content.decode('ISO-8859-1')
            data.append('Content-Disposition: form-data; name="%s"; filename="SASDKDemo.ipa"' % k)
            data.append('Content-Type: application/octet-stream\r\n')
            data.append(decoded_content)
        else:
            data.append('Content-Disposition: form-data; name="%s"\r\n' % k)
            data.append(v if isinstance(v, str) else v.decode('utf-8'))
    data.append('--%s--\r\n' % boundary)
    return '\r\n'.join(data), boundary
    
    #处理蒲公英上传结果
    def handle_resule(result):
    json_result = json.loads(result)
    if json_result['code'] is 0:
        send_Email(json_result)
    
    #发送邮件
    def send_Email(json_result):
    appName = json_result['data']['appName']
    appKey = json_result['data']['appKey']
    appVersion = json_result['data']['appVersion']
    appBuildVersion = json_result['data']['appBuildVersion']
    appShortcutUrl = json_result['data']['appShortcutUrl']
    #邮件接受者
    mail_receivers = ['xxx@xxx.com', 'xxx@xxx.com']
    #根据不同邮箱配置 host，user，和pwd
    mail_host = 'smtp.xxx.com'
    mail_user = 'xxx@xxx.com'
    mail_pwd = '123'
    mail_to = ','.join(mail_receivers)
    msg = MIMEMultipart()
    
    environsString = '<h3>移动端iOS安装包</h3><p>'
    environsString += '<p>内网ipa包下载地址 : ' + ipa_file_tomcat_http_url + '<p>'
    environsString += '<p>外网在线安装 : ' + 'http://www.pgyer.com/' + str(appShortcutUrl) + '<p>'
    environsString += '<li><a href="itms-services://?action=download-manifest&url=https://ssl.pgyer.com/app/plist/' + str(appKey) + '">手机直接安装</a></li>'
    message = environsString
    body = MIMEText(message, _subtype='html', _charset='utf-8')
    msg.attach(body)
    msg['To'] = mail_to
    msg['from'] = mail_user
    msg['subject'] = 'iOSxxx版本最新打包文件'
    
    try:
        s = smtplib.SMTP()
        s.connect(mail_host)
        s.login(mail_user, mail_pwd)
    
        s.sendmail(mail_user, mail_receivers, msg.as_string())
        s.close()
    
        print 'success'
    except Exception, e:
        print e
    #############################################################
    
    #请求参数字典
    params = {
    'uKey': uKey,
    '_api_key': _api_key,
    'file': open(ipa_file_path, 'rb'),
    'publishRange': '2',
    }
    coded_params, boundary = _encode_multipart(params)
    req = urllib2.Request(url, coded_params.encode('ISO-8859-1'))
    req.add_header('Content-Type', 'multipart/form-data; boundary=%s' % boundary)
    try:
    resp = urllib2.urlopen(req)
    body = resp.read().decode('utf-8')
    handle_resule(body)
    except urllib2.HTTPError as e:
    print(e.fp.read())

> 转自: http://www.jianshu.com/p/c69deb29720d  





