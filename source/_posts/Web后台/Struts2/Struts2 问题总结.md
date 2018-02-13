---
title: Struts2 问题总结
date: 2017-09-19 16:12:44
categories: 服务器
tags: [java,struts2]
---
## 上传图片出现错误
但是，明明上传的文件格式是正确，还是出现：

    Content-Type not allowed: file "09poC_wallpapers.jpg" "upload_1ea6fe4e_13611ac7d7c__8000_00000012.tmp" image/pjpeg 


firefox 和 ie 的文件类型区别
    Firefox：
    	
    
    image/jpeg, image/bmp, image/gif, image/png
    
    ie 6：
    	
    
     image/pjpeg ,image/bmp, image/gif, image/x-png
    
    ie 7：
    	
    
    image/pjpeg, image/bmp, image/gif, image/x-png
    
    ie 8：
    	
    
    image/pjpeg, image/bmp, image/gif, image/x-png
    
    Ie 9： 
    	
    
    image/jpeg, image/bmp, image/gif, image/png 
解决方法

    <param name="allowedTypes">
      image/bmp,image/png,image/gif,image/jpeg,image/jpg,
      image/pjpeg ,image/bmp, image/gif, image/x-png,
    </param>