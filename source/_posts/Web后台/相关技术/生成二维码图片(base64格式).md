---
title: 生成二维码图片(base64格式)
date: 2018-01-29 21:00:12
categories: server
tags: [Java] 
---
### 生成二维码图片(base64格式)


<!--more-->
```java
package com.guangeryi.mall.payment.weixin;

import com.google.zxing.BarcodeFormat;
import com.google.zxing.EncodeHintType;
import com.google.zxing.MultiFormatWriter;
import com.google.zxing.client.j2se.MatrixToImageWriter;
import com.google.zxing.common.BitMatrix;
import org.apache.commons.codec.binary.Base64;

import javax.imageio.ImageIO;
import java.awt.image.BufferedImage;
import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.util.Hashtable;
import java.util.Map;

public class QRCodeUtils {

    /**
     * 生成二维码 Base64编码后字符串
     *
     * @param contents 内容
     */
    public static String encodeQRCodeBase64(String contents) {
        return encodeQRCodeToBase64Str(contents);
    }

    /**
     * 生成二维码 Base64编码后字符串 <img src=''> src填入该字符串显示图片
     * （高度:300 , 宽度:300）
     * @param contents 内容
     */
    private static String encodeQRCodeToBase64Str(String contents) {
        int width = WxPcPayConfigImpl.QR_IMG_WIDTH;
        int height = WxPcPayConfigImpl.QR_IMG_HEIGHT;
        Map<EncodeHintType, Object> hints = new Hashtable<>();
        String base64Img = "data:image/png;base64,";
        // 指定编码格式
        hints.put(EncodeHintType.CHARACTER_SET, "UTF-8");
        try {
            // 生成输出流
            BitMatrix bitMatrix1 = new MultiFormatWriter().encode(contents,
                    BarcodeFormat.QR_CODE, width, height, hints);
            BufferedImage image = MatrixToImageWriter.toBufferedImage(bitMatrix1);
            base64Img = base64Img + encodeToString("png", image);
        } catch (Exception e) {
            e.printStackTrace();
        }
        return base64Img;
    }
    /**
     * 将图片转换成base64格式进行存储
     *
     * @param formatName 文件格式
     * @param image      图片流
     * @return base64字符串
     */
    private static String encodeToString(String formatName, BufferedImage image) {
        String imageString = null;
        try (ByteArrayOutputStream bos = new ByteArrayOutputStream()) {
            ImageIO.write(image, formatName, bos);
            byte[] imageBytes = bos.toByteArray();
            imageString = new String(Base64.encodeBase64(imageBytes));
        } catch (IOException e) {
            e.printStackTrace();
        }
        return imageString;
    }

    public static void main(String[] args) {
        // 输出在img标签中img属性中
        System.out.println(encodeQRCodeBase64("你好"));
    }
}
```