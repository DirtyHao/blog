 
## 传参的三种方式
http请求中参数传递有三种方式,即  

    1. request head 中url的queryparams.    
    2. request body 中的formdata.    
    3. request body 中的payload.

其中1和2，3两种很好区别,那什么时候请求会把参数放在formdata里,什么时候会放在payload里呢？  

    1. content-type为application/json、multipart/form-data等通过Payload传递,
    2. content-type为application/x-www-form-urlencoded时通过formdata传递
## 参数的三种格式
1. kv键值对的形式。对应的viewsource为 `a=a&b=b`  
![](http://wx1.sinaimg.cn/small/7defda4egy1fmt2f1mh1xj20qo03mt8x.jpg  ) 
2.  json 的形式。对应的viewsource为 `{"a":"a","b":{"b1":"b1"}}`  
![](http://wx2.sinaimg.cn/mw690/7defda4egy1fmt2z2m1z9j20os06yq3k.jpg  ) 
3.  formData的形式   
![](http://wx4.sinaimg.cn/mw690/7defda4egy1fmt2z35fi7j20ms0b4dh7.jpg  ) 

我们传递参数时选用哪种方式取决服务端的实现,不过content-type 和 参数的格式有一个标准的对应关系在里面。 这里已常用的三个content-type 为例。  

Content-type | 对应格式 
----|------
application/json | `{"a":"a","b":{"b1":"b1"}}`   
application/x-www-form-urlencoded | `a=a&b=b`  
multipart/form-data | `------WebKitFormBoundaryFee8luBJvHYrVILJContent-Disposition: form-data; name="a"a------WebKitFormBoundaryFee8luBJvHYrVILJ--` 
 

