# 浏览器缓存

## 强缓存

Cache-Control与Expires
用户发送的请求，直接从客户端缓存中获取，不发送请求到服务器，不与服务器发生交互行为。

## 协商缓存

Last-Modified/ETag
用户发送的请求，发送到服务器后，由服务器判定是否从缓存中获取资源。

两者共同点：客户端获得的数据最后都是从客户端缓存中获得。

两者的区别：从名字就可以看出，强缓存不与服务器交互，而协商缓存则需要与服务器交互。

## Cache-Control与Expires

　　Cache-Control与Expires的作用一致，都是指明当前资源的有效期，控制浏览器是否直接从浏览器缓存取数据还是重新发请求到服务器取数据。只不过Cache-Control的选择更多，设置更细致，如果同时设置的话，其优先级高于Expires。

## Last-Modified/ETag与Cache-Control/Expires

　　配置Last-Modified/ETag的情况下，浏览器再次访问统一URI的资源，还是会发送请求到服务器询问文件是否已经修改，如果没有，服务器会只发送一个304回给浏览器，告诉浏览器直接从自己本地的缓存取数据；如果修改过那就整个数据重新发给浏览器；

　　Cache-Control/Expires则不同，如果检测到本地的缓存还是有效的时间范围内，浏览器直接使用本地副本，不会发送任何请求。两者一起使用时，Cache-Control/Expires的优先级要高于Last-Modified/ETag。即当本地副本根据Cache-Control/Expires发现还在有效期内时，则不会再次发送请求去服务器询问修改时间（Last-Modified）或实体标识（Etag）了。

　　一般情况下，使用Cache-Control/Expires会配合Last-Modified/ETag一起使用，因为即使服务器设置缓存时间, 当用户点击“刷新”按钮时，浏览器会忽略缓存继续向服务器发送请求，这时Last-Modified/ETag将能够很好利用304，从而减少响应开销。

## Last-Modified与ETag

你可能会觉得使用Last-Modified已经足以让浏览器知道本地的缓存副本是否足够新，为什么还需要Etag（实体标识）呢？HTTP1.1中Etag的出现主要是为了解决几个Last-Modified比较难解决的问题：

1.Last-Modified标注的最后修改只能精确到秒级，如果某些文件在1秒钟以内，被修改多次的话，它将不能准确标注文件的新鲜度

2.如果某些文件会被定期生成，当有时内容并没有任何变化，但Last-Modified却改变了，导致文件没法使用缓存

3.有可能存在服务器没有准确获取文件修改时间，或者与代理服务器时间不一致等情形

4.如果同样的一个文件位于多个CDN服务器上的时候内容虽然一样，修改时间不一样。

## 参考

- [浏览器缓存机制](https://www.cnblogs.com/slly/p/6732749.html)
- [缓存过程](https://www.cnblogs.com/shixiaomiao1122/p/7591556.html)
