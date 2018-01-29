---
title: 根据IP获取城市地理位置
date: 2018-01-29 20:54:12
categories: server
tags: [Java] 
---
### 根据IP获取城市地理位置
使用的是百度查询的api，试过到淘宝的API, 但是淘宝做了访问次数限制，如果批量查询的话直接timeout

```java
package com.guangeryi.mall.third.common;

import net.sf.json.JSONObject;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.io.*;
import java.net.URL;
import java.nio.charset.Charset;
import java.util.HashMap;
import java.util.Map;

/**
 * 根据IP获取城市地理位置
 * 调用百度api：http://api.map.baidu.com/location/ip
 */
public class IpUtils {

    private final static Logger logger = LoggerFactory.getLogger(AddressUtils.class);

    /**
     * 根据ip获取城市信息
     * @param ip
     * @return
     */
    public static Map<String, Object> getCityInfoByIp(String ip){
        Map<String, Object> result = new HashMap<>();
        result.put("status","success");
        String jsonInfo = null;
        try {
            jsonInfo = getCityInfoByUrlAPI(ip);
            logger.info("jsonInfo:" + jsonInfo);
        } catch (IOException e) {
            logger.error("调用 api.map.baidu.com/location/ip 获取城市信息异常, ip:" + ip, e);
            result.put("status","failed");
            return  result;
        }
        String province = "";
        String city = "";
        String district ="";
        String street = "";
        try {
            JSONObject jsonObject = JSONObject.fromObject(jsonInfo);
            if (jsonObject != null) {
                if (jsonObject.getJSONObject("content") != null) {
                    JSONObject addressDetail = jsonObject.getJSONObject("content").getJSONObject("address_detail");
                    province = (String)addressDetail.get("province"); // 省
                    city = (String)addressDetail.get("city");         // 市
                    district = (String)addressDetail.get("district"); // 区
                    street = (String)addressDetail.get("street");     // 街道
                }
            }
        } catch (Exception e) {
            logger.error("调用 api.map.baidu.com/location/ip 获取城市信息异常,解析JSON异常 ip:" + ip, e);
            result.put("status","failed");
            return  result;
        }
        result.put("province",province);
        result.put("city",city);
        result.put("district",district);
        result.put("street",street);
        return result;
    }

    /**
     * 调用 api.map.baidu.com/location/ip 获取城市信息
     * @param ip
     * @return
     * @throws IOException
     */
    private static String getCityInfoByUrlAPI(String ip) throws IOException {
        String url = "http://api.map.baidu.com/location/ip?ak=F454f8a5efe5e577997931cc01de3974&ip=" + ip;
        try (InputStream is = new URL(url).openStream()) {
            BufferedReader rd = new BufferedReader(new InputStreamReader(is, Charset.forName("UTF-8")));
            String jsonText = getStrByReader(rd);
            jsonText = decodeUnicode(jsonText);
            return jsonText;
        }
    }
    /**
     * 获取流数据
     * @param rd
     * @return
     * @throws IOException
     */
    private static String getStrByReader(Reader rd) throws IOException {
        StringBuilder sb = new StringBuilder();
        int cp;
        while ((cp = rd.read()) != -1) {
            sb.append((char) cp);
        }
        return sb.toString();
    }
    /**
     * unicode 转换成 中文
     *
     * @author fanhui 2007-3-15
     * @param theString 字符串
     * @return String
     */
    private static String decodeUnicode(String theString) {
        char aChar;
        int len = theString.length();
        StringBuilder outBuilder = new StringBuilder(len);
        for (int x = 0; x < len;) {
            aChar = theString.charAt(x++);
            if (aChar == '\\') {
                aChar = theString.charAt(x++);
                if (aChar == 'u') {
                    int value = 0;
                    for (int i = 0; i < 4; i++) {
                        aChar = theString.charAt(x++);
                        switch (aChar) {
                            case '0':
                            case '1':
                            case '2':
                            case '3':
                            case '4':
                            case '5':
                            case '6':
                            case '7':
                            case '8':
                            case '9':
                                value = (value << 4) + aChar - '0';
                                break;
                            case 'a':
                            case 'b':
                            case 'c':
                            case 'd':
                            case 'e':
                            case 'f':
                                value = (value << 4) + 10 + aChar - 'a';
                                break;
                            case 'A':
                            case 'B':
                            case 'C':
                            case 'D':
                            case 'E':
                            case 'F':
                                value = (value << 4) + 10 + aChar - 'A';
                                break;
                            default:
                                throw new IllegalArgumentException("Malformed      encoding.");
                        }
                    }
                    outBuilder.append((char) value);
                } else {
                    if (aChar == 't') {
                        aChar = '\t';
                    } else if (aChar == 'r') {
                        aChar = '\r';
                    } else if (aChar == 'n') {
                        aChar = '\n';
                    } else if (aChar == 'f') {
                        aChar = '\f';
                    }
                    outBuilder.append(aChar);
                }
            } else {
                outBuilder.append(aChar);
            }
        }
        return outBuilder.toString();
    }
    public static void main(String[] args) {
        System.out.println(getCityInfoByIp("118.212.211.23"));
    }
}

```