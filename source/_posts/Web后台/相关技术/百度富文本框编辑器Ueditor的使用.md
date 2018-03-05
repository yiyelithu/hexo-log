---
title: 百度富文本框编辑器Ueditor的使用
date: 2018-03-05 22:54:12
categories: server
tags: [Java] 
---
### 百度富文本框编辑器：
官网： http://ueditor.baidu.com/website/ 

官网演示地址：http://ueditor.baidu.com/website/onlinedemo.html

UEditor是由百度web前端研发部开发所见即所得富文本web编辑器，具有轻量，可定制，注重用户体验等特点，开源基于MIT协议，允许自由使用和修改代码...
<!--more-->
#### 开始使用：
参考:
http://fex.baidu.com/ueditor/
##### 1. 入门部署和体验

##### 1.1下载编辑器

到官网下载 UEditor 最新版：[官网地址]

##### 1.2创建demo文件

解压下载的包，在解压后的目录创建 demo.html 文件，填入下面的html代码

```html
<!DOCTYPE HTML>
<html lang="en-US">

<head>
    <meta charset="UTF-8">
    <title>ueditor demo</title>
</head>

<body>
    <!-- 加载编辑器的容器 -->
    <script id="container" name="content" type="text/plain">
        这里写你的初始化内容
    </script>
    <!-- 配置文件 -->
    <script type="text/javascript" src="ueditor.config.js"></script>
    <!-- 编辑器源码文件 -->
    <script type="text/javascript" src="ueditor.all.js"></script>
    <!-- 实例化编辑器 -->
    <script type="text/javascript">
        var ue = UE.getEditor('container');
    </script>
</body>

</html>
```
##### 1.3 在浏览器打开demo.html

如果看到了编辑器，恭喜你，初次部署成功！

##### 2. 整合jsp后端配置

##### 2.1 下载 jsp 版本完整包

下载地址: http://ueditor.baidu.com/website/download.html 

选择 [1.4.3.3 Jsp 版本]

##### 2.2 下载之后会得到如下文件

按照官网上的做法是把文件copy到webapp跟目录下 , 但我们是集成ueditor, 肯定不是放在根目录下, 所以我们把文件都复制到 webapp/static/plugins/ueditor 下, 方便管理

![logo](/images/server/相关技术/ueditor目录结构图.png) 

##### 2.3 前台代码集成
2.3.1 在需要集成ueditor的页面添加如下代码, 如果能看到编辑器则说明配置成功
```html
<!DOCTYPE HTML>
<html lang="en-US">

<head>
    <meta charset="UTF-8">
    <title>ueditor demo</title>
    <link rel="stylesheet" href="${ctx}static/plugins/ueditor/lang/zh-cn/zh-cn.js" media="all" />
</head>

<body>
    <!-- 加载编辑器的容器 -->
    <script id="container" name="content" type="text/plain">
        这里写你的初始化内容
    </script>
    <!-- 配置文件 -->
    <script type="text/javascript" src="${ctx}static/plugins/ueditor/ueditor.config.js"></script>
    <!-- 编辑器源码文件 -->
    <script type="text/javascript" src="${ctx}static/plugins/ueditor/ueditor.all.js"></script>
    <!-- 实例化编辑器 -->
    <script type="text/javascript">
        var ueditor = UE.getEditor('container');
    </script>
</body>

</html>
```
获取编辑器内容
```html
var ueditor = UE.getEditor('container');

var content = ueditor.getContent(content);

```

设置编辑器内容
```html
 // 等UEditor创建完成就使用UEditor的setContent函数
 var ueditor = UE.getEditor('container');
 ueditor.ready(function() {
      ueditor.setContent(content);
 });
```

有了这些你可以处理一些普通文字, 但如果是要文件上传,图片上传,视频上传这些功能你就要进行一些后台代码的配置

##### 2.4 后台代码集成

后台环境： Spring + Spring Mvc + Mybatis + Maven

##### 2.4.1 配置 ueditor.config.js

原配置:
```javascript
    var URL = window.UEDITOR_HOME_URL || getUEBasePath();

    /**
     * 配置项主体。注意，此处所有涉及到路径的配置别遗漏URL变量。
     */
    window.UEDITOR_CONFIG = {

        //为编辑器实例添加一个路径，这个不能被注释
        UEDITOR_HOME_URL: URL

        // 服务器统一请求接口路径
        , serverUrl: URL + "jsp/controller.jsp"

        //工具栏上的所有的功能按钮和下拉框，可以在new编辑器的实例时选择自己需要的重新定义
```
修改后的配置：
```javascript
    window.UEDITOR_HOME_URL = "/static/plugins/ueditor/";

    var URL = window.UEDITOR_HOME_URL || getUEBasePath();

    /**
     * 配置项主体。注意，此处所有涉及到路径的配置别遗漏URL变量。
     */
    window.UEDITOR_CONFIG = {

        //为编辑器实例添加一个路径，这个不能被注释
        UEDITOR_HOME_URL: URL

        // 服务器统一请求接口路径
        , serverUrl: "/ueditor/ueditorAction"

```

1. 主要是 `window.UEDITOR_HOME_URL` 这个参数赋值成自己的ueditor的文件路径
2. 修改 服务器统一请求接口路径 `/ueditor/ueditorAction` , 这样Ueditor后台服务接口就会请求到这个接口中来

##### 2.4.2 新增后台服务接口

第一步： 导入jar包, 我是只添加了最后俩个包，其他的包可以通过maven的形式导入，copy 这俩个包放到WEBINF/lib目录下, 然后配置Maven依赖
![logo](/images/server/相关技术/jar.png) 

注：使用maven构建项目的时候需要进行如下配置, 这样maven构建的时候才不会报找不到lib目录下jar包的错误
```sql
    <dependency>
      <groupId>json</groupId>
      <artifactId>json</artifactId>
      <version>1.0</version>
      <scope>system</scope>
      <systemPath>${project.basedir}/src/main/webapp/WEB-INF/lib/json.jar</systemPath>
    </dependency>
```

第二步：新建 后台统一服务接口

```java
/**
 * Ueditor 后台统一服务接口
 * @author songshuiyang
 * @date 2018/3/4 18:11
 */
@Controller
@RequestMapping("/ueditor")
public class UEditorController extends BaseController {
    
    private HttpServletRequest request = null;
    
    private String actionType = null;

    private ConfigManager configManager = null;


    @RequestMapping(value = "ueditorAction", method = {RequestMethod.GET,RequestMethod.POST})
    @ResponseBody
    public JSONObject exec (@RequestParam String action, HttpServletRequest request) {
        String result;
        this.actionType = action;
        this.request = request;
        String rootPath =  request.getSession().getServletContext().getRealPath("/");
        String contextPath = request.getContextPath();
        this.configManager = ConfigManager.getInstance( rootPath, contextPath,"/static/plugins/ueditor/jsp/controller.jsp");

        String callbackName = this.request.getParameter("callback");

        if ( callbackName != null ) {
            result =  !validCallbackName( callbackName ) ? new BaseState( false, AppInfo.ILLEGAL ).toJSONString() : callbackName+"("+this.invoke()+");";
        } else {
            result = this.invoke();
        }
        return JSONObject.fromObject(result);

    }

    public String invoke() {

        if ( actionType == null || !ActionMap.mapping.containsKey( actionType ) ) {
            return new BaseState( false, AppInfo.INVALID_ACTION ).toJSONString();
        }

        if ( this.configManager == null || !this.configManager.valid() ) {
            return new BaseState( false, AppInfo.CONFIG_ERROR ).toJSONString();
        }

        State state = null;

        int actionCode = ActionMap.getType( this.actionType );

        Map<String, Object> conf;

        switch ( actionCode ) {

            case ActionMap.CONFIG:
                return this.configManager.getAllConfig().toString();

            case ActionMap.UPLOAD_IMAGE:
            case ActionMap.UPLOAD_SCRAWL:
            case ActionMap.UPLOAD_VIDEO:
            case ActionMap.UPLOAD_FILE:
                conf = this.configManager.getConfig( actionCode );
                state = new Uploader( request, conf ).doExec();
                break;

            case ActionMap.CATCH_IMAGE:
                conf = configManager.getConfig( actionCode );
                String[] list = this.request.getParameterValues( (String)conf.get( "fieldName" ) );
                state = new ImageHunter( conf ).capture( list );
                break;

            case ActionMap.LIST_IMAGE:
            case ActionMap.LIST_FILE:
                conf = configManager.getConfig( actionCode );
                int start = this.getStartIndex();
                state = new FileManager( conf ).listFile( start );
                break;

        }

        assert state != null;
        return state.toJSONString();

    }

    private int getStartIndex () {

        String start = this.request.getParameter( "start" );

        try {
            return Integer.parseInt( start );
        } catch ( Exception e ) {
            return 0;
        }

    }

    /**
     * callback参数验证
     * @param name 名字
     * @return boolean
     */
    private boolean validCallbackName ( String name ) {
        return name.matches( "^[a-zA-Z_]+[\\w0-9_]*$" );
    }
}
```

一： 初始化ueditor的时候, ueditor会访问该接口, 此时`action` 参数是 `config` , 该接口会返回其`/static/plugins/ueditor/jsp/config.json` 配置的json参数，这些参数配置了上传功能的一些参数, 通过这些配置你可以DIY上传功能, ueditor获取到这些参数之后就可以使用上传功能了,否则你上传文件会提示： 后端配置项没有正常加载，上传插件不能正常使用！

配置主要包括： 上传图片配置项 涂鸦图片上传配置项 截图工具上传 抓取远程图片配置 上传视频配置 上传文件配置

二： 如要上传图片, ueditor会访问该接口, 此时`action` 参数是 `uploadimage` ，则会执行上传图片操作, 上传成功后会返回
```sql
{
    "state": "SUCCESS",
    "url": "upload/demo.jpg",
    "title": "demo.jpg",
    "original": "demo.jpg"
}
```
三：由于系统文件上传使用的是阿里云的OSS所以需要将文件上传转到OSS处理上

前台配置：
```html
<script type="text/javascript">
    // 当action是如下时，访问自己定义的服务接口
    UE.Editor.prototype._bkGetActionUrl=UE.Editor.prototype.getActionUrl;
    UE.Editor.prototype.getActionUrl=function(action){
        // 上传图片, 文件, 视频
        if (action == 'uploadimage' || action == 'uploadfile'  || action == 'uploadvideo') {
            return '/file/uploadLocal';
        }  else if( action== 'uploadscrawl'){ // 上传涂鸦，涂鸦请求是Base64字符需要请求另外的接口
            return '/file/uploadScrawl';
        }   else if(action == 'listimage'){
            return this._bkGetActionUrl.call(this, action);
        } else{
            return this._bkGetActionUrl.call(this, action);
        }
    }
    var ueditor = UE.getEditor('ueditorContainer');
</script>
```

后台接口：
```java
package com.ecut.admin.controller;

import com.aliyun.oss.ClientException;
import com.aliyun.oss.OSSException;
import com.ecut.admin.entity.OssFile;
import com.ecut.admin.entity.UeditorState;
import com.ecut.admin.service.impl.FileServiceImpl;
import com.ecut.core.base.BaseController;
import com.ecut.core.utils.Base64Utils;
import com.ecut.core.utils.MessageUtils;
import com.google.common.collect.Lists;
import com.google.common.collect.Maps;
import org.apache.commons.lang3.StringUtils;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.MediaType;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.multipart.MultipartFile;
import org.springframework.web.multipart.MultipartHttpServletRequest;

import java.io.ByteArrayInputStream;
import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.util.HashMap;
import java.util.Iterator;
import java.util.List;
import java.util.Map;

import static com.ecut.core.utils.MessageUtils.success;

/**
 * 阿里云OSS文件上传控制器
 * @author songshuiyang
 * @date 2018/2/11 20:22
 */
@Controller
@RequestMapping("/file")
public class FileController extends BaseController {

    private final Logger logger = LoggerFactory.getLogger(getClass());

    @Autowired
    private FileServiceImpl fileServiceImpl;

    /**
     * 文件上传
     * produces="application/json;charset=UTF-8 解决服务器返回406问题
     * @param file
     * @return
     * @throws OSSException
     * @throws ClientException
     * @throws IOException
     */
    @RequestMapping(value = "/uploadLocal", method = RequestMethod.POST, produces="application/json;charset=UTF-8")
    @ResponseBody
    public UeditorState uploadLocalFile(@RequestParam(value = "upfile",required = false) MultipartFile file) throws OSSException, ClientException, IOException {
        Map<String, Object> resultMap = new HashMap<>();
        OssFile file1 = fileServiceImpl.uploadFileByMultipartFile(file);
        UeditorState ueditorState = new UeditorState("SUCCESS",file1.getFileSrc(),file1.getFileName(),file1.getFileName());
        return ueditorState;
    }
    /**
     * 上传涂鸦照片
     * @param upfile
     * @return
     * @throws Exception
     */
    @RequestMapping(value = "/uploadScrawl", method = RequestMethod.POST, produces="application/json;charset=UTF-8")
    @ResponseBody
    public UeditorState uploadscrawl(String upfile) throws Exception {
        byte [] bytes= Base64Utils.decode(upfile);
        InputStream inputStream = new ByteArrayInputStream(bytes);
        String fileType = "image/png";
        Long fileSize = new Long((long)bytes.length);
        String fileName = "scrawl" + System.currentTimeMillis() + ".png";
        String extensionName = "png";
        OssFile file1 = fileServiceImpl.uploadFileByInputStream(inputStream, fileType,fileSize,fileName,extensionName);
        UeditorState ueditorState = new UeditorState("SUCCESS",file1.getFileSrc(),file1.getFileName(),file1.getFileName());
        return ueditorState;
    }
}
```
##### 2.4.3 问题集合

###### 解决百度ueditor富文本编辑器不能插入视频的问题/src掉链/src清空，不能显示视频
转载：http://blog.csdn.net/qq_34787830/article/details/75092347

> 直接下载到的百度富文本编辑器当插入视频的时候会自动清掉src，不显示视频造成这样的原因是:百度富文本编辑器的过滤器xssFilter导致插入视频异常，编辑器在切换源码的过程中过滤掉img的_url属性（用来存储视频url）

解决办法:

1.在配置文件ueditor.config.js中，定位 //xss过滤白名单，即,whitList:{ }，对 img: 增加 “_url” 属性： 
2. 在下面的 video 标签后面新增3给标签，使Ueditor分别能支持embed标签和iframe标签：
```java
 source: ['src', 'type'],

 embed: ['type', 'class', 'pluginspage', 'src', 'width', 'height', 'align', 'style', 'wmode', 'play',  

      +  'autoplay','loop', 'menu', 'allowscriptaccess', 'allowfullscreen', 'controls', 'preload'],

 iframe: ['src', 'class', 'height', 'width', 'max-width', 'max-height', 'align', 'frameborder', 'allowfullscreen']
```