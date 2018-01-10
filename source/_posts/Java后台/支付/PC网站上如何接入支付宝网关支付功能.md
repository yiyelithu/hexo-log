---
title: PC网站上如何接入支付宝网关支付功能
date: 2018-01-10 10:24:12
categories: server
tags: 支付
---
### 一: 应用场景
1. 主要应用于一些交易平台商品订单支付，账户充值，线上收费这些有支付需求的交易

2. 用户通过支付宝PC收银台完成支付，交易款项即时给到商户支付宝账户

![logo](/images/server/支付/taobao.png)

![logo](/images/server/支付/12306.png) 


### 二: 准备条件
1. 一个公司, 不是公司的话是不能接入商户支付宝网关支付的, 当然支付宝是分个人用户和商户用户的, 如果是个人网站的话可以贴个自己收款二维码上去进行收款, 如果是正在运营的商户企业收取费用的话是要接入支付宝网关支付功能进行收费, 这样的话可以看起来bigger更高

2. 企业或个体工商户，具有真实有效的营业执照，且支付宝账户名称需与营业执照主体一致

3. 网站通过ICP备案，能正常访问，页面显示完整，有明确的运营内容与完整的商品信息。

### 三：接入支付宝支付功能步骤

#### 第一步：创建应用
要在应用中使用支付宝开放产品的接口能力：

1. 需要先去蚂蚁金服开放平台，在开发者中心创建登记您的应用，此时将获得应用唯一标识（APPID）
2. 请在【功能信息】中点击【添加功能】，选择【电脑网站支付】
3. 提交审核（需要上传公司营业执照,填写法人身份信息等等），等待审核通过，该应用正式可以使用

> TIPS：电脑网站支付接口需签约后才能调用

#### 第二步：配置密钥

开发者调用接口前需要先生成RSA密钥，RSA密钥包含应用私钥(APP_PRIVATE_KEY)、应用公钥(APP_PUBLIC_KEY）。生成密钥后在开放平台管理中心进行密钥配置，配置完成后可以获取支付宝公钥(ALIPAY_PUBLIC_KEY)。


**用途：**
支付宝发送信息给商户系统时，使用支付宝私钥对数据进行加签，商户获取到支付宝加签的信息后使用支付宝公钥对数据进行验签，得到正确的数据。
商户系统给支付宝发送信息时，使用商户自己的私钥对数据加签，支付宝获取到数据后使用商家上传的公钥进行验签。


**加签步骤:**

1.筛选

获取所有请求参数，不包括字节类型参数，如文件、字节流，剔除sign与sign_type参数。

2.排序

将筛选的参数按照第一个字符的键值ASCII码递增排序（字母升序排序），如果遇到相同字符则按照第二个字符的键值ASCII码递增排序，以此类推。
拼接将排序后的参数与其对应值，组合成“参数=参数值”的格式，并且把这些参数用&字符连接起来，此时生成的字符串为待签名字符串。商户将待签名字符串和商户私钥带入加签算法中得出sign。然后将sign值加入到请求参数中，发送给支付宝

3.拼接

将排序后的参数与其对应值，组合成“参数=参数值”的格式，并且把这些参数用&字符连接起来，此时生成的字符串为待签名字符串。

4.加签

商户将待签名字符串和商户私钥带入加签算法中得出sign。然后将sign值加入到请求参数中，发送给支付宝


**验签步骤：**

与加签步骤一致，只不过是延签是使用公钥算出sign值，两方算出的sign值都一致的话则延签成功

#### 第三步：搭建和配置开发环境
1. 需要到支付宝开发平台下载服务端SDK,打包即用, 十分方便

2. 配置参数
```java
AlipayClient alipayClient = new DefaultAlipayClient(URL,APP_ID,APP_PRIVATE_KEY,FORMAT,CHARSET,ALIPAY_PUBLIC_KEY,SIGN_TYPE);

// URL:支付宝网关（固定） https://openapi.alipay.com/gateway.do, 如果是沙箱环境的话: https://openapi.alipaydev.com/gateway.do
// APP_ID:创建应用时获取, 支付宝提供
// APP_PRIVATE_KEY: 应用私钥, 运用支付宝提供的工具进行生成
// FORMAT: json（固定）
// CHARSET: 编码格式
// ALIPAY_PUBLIC_KEY: 支付宝公钥, 由支付宝提供
// SIGN_TYPE： 加签类型，商户生成签名字符串所使用的签名算法类型，目前支持RSA2和RSA，推荐使用RSA2
```
> 配置完参数之后就可以调用支付宝的支付接口了, 十分方便, 阿里阿里 !!!
3. 配置完参数之后先来看一下支付的调用流程：
![logo](/images/server/支付/step.png) 

4. 接下来就是发起支付请求了
```java
import com.alipay.api.*;
import com.alipay.api.request.*; 
public void doPost(HttpServletRequest httpRequest, HttpServletResponse httpResponse) throws ServletException,IOException { 
//获得初始化的AlipayClient 
AlipayClient alipayClient = new DefaultAlipayClient("https://openapi.alipay.com/gateway.do", APP_ID, APP_PRIVATE_KEY, FORMAT, CHARSET, ALIPAY_PUBLIC_KEY, SIGN_TYPE);
//创建支付请求的对应request
AlipayTradePagePayRequest alipayRequest = new AlipayTradePagePayRequest();
//设置请求参数及回跳地址和通知地址
 alipayRequest.setBizContent(
"{" + " \"out_trade_no\":\"20150320010101001\",
" + " \"total_amount":88.88,
" + " \"subject\":title\",
" + " \"body\":\"Iphone6 16G\",
" + }");
//跳转地址就是支付完成之后，支付宝自动执行页面重定向,就是跳转到我们设置的页面
alipayRequest.setReturnUrl("http://www.songshuiyang.site/return_url"); 
//通知地址就是支付宝会根据API中商户传入的notify_url，通过POST请求的形式将支付结果作为参数通知到商户系统。
alipayRequest.setNotifyUrl("http://www.songshuiyang.site/notify_url");
String form="";
try { 
 //调用SDK生成html表单
  form = alipayClient.pageExecute(alipayRequest).getBody(); 
} catch (AlipayApiException e) { 
  e.printStackTrace(); 
} 
//直接将完整的表单html输出到页面
httpResponse.setContentType("text/html;charset=" + CHARSET); httpResponse.getWriter().write(form); httpResponse.getWriter().flush(); httpResponse.getWriter().close(); 
} 
```

**支付接口生成的html代码**
```html
<form name="punchout_form" method="post" action="https://openapi.alipay.com/gateway.do?charset=utf-8&method=alipay.trade.page.pay&sign=jsgXRru7b%2FHLO76SMPoj6lIuCnKJ9lkLo%2BTPIKfetqMOd8kyp2zYBZ456Dvf0eb4SyYgUrOjAgTkNW2AkgJh%2BbLJDu3eAtQVAUEEzFGy2Ix3uE3j3lPLHZDs1cF7g8vw7hwfmEqe8CE8OCJ%2B79J0Hp6YFOH8vnJEDUPvjla2AsCO0mhAsnYxm30rmqgDqJPfZLytOvRD5FF%2BoBd4UPH%2Budk7vCn9lEX%2BkEe7YBa3E7l6vWxXz%2BJDKGL9ZMHNtUzYUaid%2F%2BIugVLqtECybldd8YDZUFnz92Iq%2BOwIL09MzNtb6iC9AypfQxlTseFezDihBn%2Fey5itIovqntbLLdxt2g%3D%3D&return_url=http%3A%2F%2Fwww.songshuiyang.com%2Fbuyer2%2Fpayment%2Findex&notify_url=http%3A%2F%2Fwww.songshuiyang.com%2Fbuyer2%2Fpayment2%2Falipay_notify&version=1.0&app_id=123456789101554&sign_type=RSA2&timestamp=2018-01-01+14%3A27%3A50&alipay_sdk=alipay-sdk-java-dynamicVersionNo&format=json">
   <input type="hidden" name="biz_content" value="{&quot;out_trade_no&quot;:&quot;160b06765224a9aee66a6654541b947f&quot;,&quot;total_amount&quot;:&quot;0.01&quot;,&quot;subject&quot;:&quot;江西广而易科技有限公司  账户充值&quot;,&quot;body&quot;:&quot;充值金额: 0.01&quot;,&quot;product_code&quot;:&quot;FAST_INSTANT_TRADE_PAY&quot;}">
     <input type="submit" value="立即支付" style="display:none" >
</form>
<script>document.forms[0].submit();</script>
```

**注意**
1. action 链接后面的sign值就是签名字符串, 用于校验数据的来源还有数据有没有被修改
2. biz_content 是业务参数
3. html输出到页面后会跳转到支付的支付页面

#### 第四步：扫码支付进行的步骤
1.支付
 ![logo](/images/server/支付/pagePay.jpg) 

2.支付成功会自动跳转到商户页面(同步通知) 

> 就是前面设置的 alipayRequest.setReturnUrl("http://www.songshuiyang.site/return_url");,这部是支付完成之后支付宝的处理程序进行了页面重定向, 不是支付宝主动触发的。
 ![logo](/images/server/支付/paysuccess.png) 

3.系统后台收到异步通知

> 对于PC网站支付的交易，在用户支付完成之后，支付宝会根据API中商户传入的alipayRequest.setNotifyUrl("http://www.songshuiyang.site/notify_url");，通过POST请求的形式将支付结果作为参数通知到商户系统，该方式的作用是页面跳转同步通知没有处理订单更新，需要通过异步通知的方式去通知系统后台更新流水

4.进行异步通知处理

> 程序执行完后必须打印输出“success”。如果商户反馈给支付宝的字符不是success这7个字符，支付宝服务器会不断重发通知，直到超过24小时22分钟。一般情况下，25小时以内完成8次通知（通知的间隔频率一般是：4m,10m,10m,1h,2h,6h,15h）；

处理代码:
```java
//将异步通知中收到的所有参数都存放到map中
Map<String, String> paramsMap = ...;
//调用SDK验证签名
boolean signVerified = AlipaySignature.rsaCheckV1(paramsMap, ALIPAY_PUBLIC_KEY, CHARSET, SIGN_TYPE) 
if(signVerfied){ 
// TODO 验签成功后，按照支付结果异步通知中的描述，对支付结果中的业务内容进行二次校验
1、商户需要验证该通知数据中的out_trade_no是否为商户系统中创建的订单号，
2、判断total_amount是否确实为该订单的实际金额（即商户订单创建时的金额），
3、校验通知中的seller_id（或者seller_email) 是否为out_trade_no这笔单据的对应的操作方
4、验证app_id是否为该商户本身。
// 二次校验成功，继续商户自身业务处理，处理完成之后返回success

} else {
 // TODO 验签失败则记录异常日志，并在response中返回failure. 
} 
```

**注意：** 
1.  这里延签公钥是支付宝公钥, 不是应用公钥, 如果是按照支付宝的示例代码的话很容易填成应用公钥, 导致延签失败

5. 如果是异步通知处理失败
> 当商户后台、网络、服务器等出现异常，商户系统最终未接收到支付异步通知；需要自己手动向支付宝发送查询请求，根据查询出来的结果确定该交易是否成功

```java
AlipayClient alipayClient = new DefaultAlipayClient("https://openapi.alipay.com/gateway.do","app_id","your private_key","json","GBK","alipay_public_key","RSA2"); 
//创建查询请求的对应request
AlipayTradeQueryRequest request = new AlipayTradeQueryRequest();
request.setBizContent("{" + 
"\"out_trade_no\":\"20150320010101001\",
" + "\"trade_no\":\"2014112611001004680073956707\"" + 
"}"); 
AlipayTradeQueryResponse response = alipayClient.execute(request); 
if(response.isSuccess()){ 
//交易状态：WAIT_BUYER_PAY（交易创建，等待买家付款）、TRADE_CLOSED（未付款交易超时关闭，或支付完成后全额退款）、TRADE_SUCCESS（交易支付成功）、TRADE_FINISHED（交易结束，不可退款）
    System.out.println("调用成功"); 
} else {
    System.out.println("调用失败"); 

```

**注意：** 
1.  这里延签公钥是支付宝公钥, 不是应用公钥, 如果是按照支付宝的示例代码的话很容易填成应用公钥, 导致签名失败

#### 五：支付宝网关支付API
|     接口英文名 | 接口中文    | 
    | --------   | -----:   | 
    | alipay.trade.page.pay       | 统一收单下单并支付页面接口 |
    |  alipay.trade.refund        | 统一收单交易退款接口      |
    | alipay.trade.fastpay.refund.query      | 统一收单交易退款查询接口 |
    |  alipay.trade.query       | 统一收单线下交易查询接口     |
    | alipay.trade.close       | 统一收单交易关闭接口 |
    |  alipay.data.dataservice.bill.downloadurl.query       | 查询对账单下载地址      |

#### 六： 使用沙箱环境进行测试

> 蚂蚁沙箱环境(Beta)是协助开发者进行接口功能开发及主要功能联调的辅助环境。沙箱环境模拟了开放平台部分产品的主要功能和主要逻辑（当前沙箱支持产品请参考“沙箱支持产品列表”）。在开发者应用上线审核前，开发者可以根据自身需求，先在沙箱环境中了解、组合和调试各种开放接口，进行开发调通工作，从而帮助开发者在应用上线审核完成后，能更快速、更顺利的进行线上调试和验收工作。
 
 ![logo](/images/server/支付/sandbox.png)
 
> 可以体验一把土豪的感觉, 不用在真实环境下使用一分钱测试联调大法了

#### 七：总结

1. 支付宝的支付接口进行了高度封装，可以拿过来直接使用，不必关心怎样签名&验签、HTTP接口请求这些处理

2. 在进行数据传输通信的同时，需要校验传输数据的来源，数据有没有进行修改，防止恶意数据攻击

 
#### 注:
本文内容参考支付宝开放平台文档内容, 一切以官方文档为准, 链接地址: https://open.alipay.com/platform/home.htm