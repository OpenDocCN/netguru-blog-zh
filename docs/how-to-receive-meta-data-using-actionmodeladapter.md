# 如何使用 ActionModelAdapter 接收元数据

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/how-to-receive-meta-data-using-actionmodeladapter>

 ## 问题

使用 findAll，时，没有元数据可供使用:

```
store.findAll('item').then((response) => {
 let meta = response.get('meta'); //undefined
})
```

但是，meta 出现在响应中:

```
{
 "items":
 [...],
 "meta":
   {
     total: 6
   }
}
```

解决办法

## 事实证明， findAll 方法不支持元数据。你必须以如下方式使用查询方法:

As it turns out, findAll method doesn't support metadata. You have to use query method in the following way:

```
store.query('item', {}).then((response) => { 
 let meta = response.get('meta'); //6
}) 
```