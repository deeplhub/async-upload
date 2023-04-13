# 异步上传文件显示进度条

## 问题

我们在写网站时难免会遇到需要上传文件的场景，但当上传大文件时比如5个G的文件直接用表单直接提交文件会出现页面卡顿、未响应等影响用户体验的情况，而且用户也不知道文件上传的进度，他就会认为是不是网页卡死了从而直接关闭页面或者刷新页面而导致上传失败

## 解决

利用异步+监听来帮我们解决这个问题


## 创建一个FormData对象

我们可以手动创建一个表单对象来存放我们要上传的文件，代码如下：

```javascript
let fd = new FormData();
fd.append("file", fs); //fs 为要上传的文件
```

## 利用XMLHttpRequest发送异步请求去提交表单

- 创建一个XMLHttpRequest对象
- 将上步创建的表单对象放入request body中
- 开始上传
- 成功回调

```javascript
let xhr = new XMLHttpRequest();
// /upload为目标url，换成你自己的
xhr.open("POST", "/upload", true);
xhr.send(fd);
xhr.onloadend = function () {
  alert("上传成功")
}
```


## 加入监听

- 完成上述操作我们已经可以异步上传，但异步上传时用户还不知道上传的进度，所以我们需要在上传之前加上监听

```javascript
let xhr = new XMLHttpRequest();
// /upload为目标url，换成你自己的
xhr.open("POST", "/upload", true);
// uploadProgress为监听的回调
xhr.upload.addEventListener("progress", uploadProgress, false);
xhr.send(fd);
xhr.onloadend = function () {
  alert("上传成功")
}
```

- uploadProgress中获取上传进度

```javascript

function uploadProgress(evt) {
  if (evt.lengthComputable) {
    // evt.loaded：文件上传的大小 evt.total：文件总的大小
    console.log(evt.loaded);
    console.log(evt.total);
    let percentComplete = Math.round((evt.loaded) * 100 / evt.total);
    consolo.log("进度："+percentComplete+"%")
  }
}
```


<br>
<br>
<br>
<br>
<br>
<br>

## 参考地址

- https://www.kevinlu98.cn/archives/113.html