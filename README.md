# 总结正则表达式和解析url
## 解析url
#### 将url的查询参数解析为字典对象？
解决方案一般是拆字符串和用正则匹配。比价推荐的方法是正则匹配。因为url允许用户随意输入，如果用拆字符串的方法，有任何一个地方没考虑到容错，
就会导致程序出错。而正则就没有这个问题。它只匹配出正确的配对。非法的全部过滤掉。不过这里把两种方式都写一下。

**拆分字符串的方式**

```
function getUrlParms(){
     var args=new Object(); 
     var query=location.search.substring(1);//获取查询串 
     var pairs=query.split("&");    // 以&分割
     
     for(var i=0;i<pairs.length;i++){ 
         var pos=pairs[i].indexOf('=');         //查找name=value 
         if(pos==-1) continue;                  //如果没有找到就跳过 
         var argname=pairs[i].substring(0,pos); //提取name 
         var value=pairs[i].substring(pos+1);   //提取value 
         args[argname]=decodeURIComponent(value); //存为属性 
     }
     return args;
}
```

**正则的方式**

```
function getQueryObject(url){
  var urlStr=url==null?window.location.href:url;    //校验传进来的url。如果为null 则重新从window对象中取得
  var search = urlStr.substring(urlStr.lastIndexOf("?")+1);  //获取到问号后边的部分
  var obj={};
  var reg = /([^?&=]+)=([^?&=]*)/g;
  search.replace(reg,function(rs,$1,$2){
      var name = decodeURIComponent($1);
      var val = decodeURIComponent($2);
      obj[name]=String(val);
      return rs;
  });
  return obj;
}
```
明天解释正则~~


