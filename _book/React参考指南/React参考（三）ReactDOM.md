chrome 
[测量资源加载时间](https://developers.google.com/web/tools/chrome-devtools/network-performance/resource-loading#view-network-timing-details-for-a-specific-resource)

[了解 Resource Timing](https://developers.google.com/web/tools/chrome-devtools/network-performance/understanding-resource-timing)


---
title: 实现复制或截图可直接粘贴图片
tag:
-  JavaScript
categories:
- 工作
---
在很多富文本编辑器或者编辑框中添加图片，都是通过点击按钮-选择图片-上传的形式，这需要使用者先保存好图片。在有些情况下，实现截图或复制图片后直接粘贴图片，让操作更简单。    
例如，在textarea中粘贴图片，通过监听paste动作来获取粘贴板中的图片数据，并将图片上传到服务器上，获取图片的url后在指定的地方显示，一般是将图片插入textarea中光标所在的地方。
<!-- more -->
 
## 实现
实现过程只需要一个js文件

```javascript
    var input = document.getElementsByTagName("textarea")[0];
    input.addEventListener('paste', pasteHandler);
    
    /*Handle paste event*/
    function pasteHandler(e){    //e:  ClipboardEvent
        //check if event.clipboardData is supported(chrome)
        if(e.clipboardData){
            var items = e.clipboardData.items;
            if(items){
                for(var i=0;i<items.length;i++){
                    if(items[i].kind == 'file' || items[i].type.indexOf('image') > -1){
                        //need to represent the image as a file
                        var blob = items[i].getAsFile();
    
                        var reader = new FileReader();
                        //need base64 to upload to server
                        reader.readAsDataURL(blob);
                        reader.onload = function(e) {   // ProgressEvent
                            uploadFile(e.target.result);  //or reader.result
                        }
                    }
                }
            }
        }
    };
    
    
    var uploadFile = function (src) {
        var fileBase64 = src.split(",")[1];
        var data = [{
            base64: fileBase64
        }];
        $.ajax({
            type: "POST",
            url: "/feedback/attach/upload",
            data: JSON.stringify({data:data}),
            dataType: "json",
            contentType: "application/json",
            success: function(data){
                if (200 == data.code && data.msg.attachments && data.msg.attachments.length) {
                    var attachs = data.msg.attachments;
    
                    var liNode = document.createElement("li");
                    var imageNode = document.createElement('img');
                    imageNode.setAttribute('src',attachs[0].thumbUrl);
                    $(liNode).attr("data-fileId", attachs[0].fileId);
                    $(liNode).attr("data-url", attachs[0].url);
    
                    liNode.appendChild(imageNode);
                    document.getElementById('images-paste').appendChild(liNode);
    
                }
            },
            error: function(err){
                console.log(err);
            }
        });
    };
```
 

1.   document.getElementsByTagName("textarea")， getElementsByTagName拿出来的是一个数组，在当前页面只有一个textarea,通过document.getElementsByTagName("textarea")[0]获得，但是建议用document.getElementsById()方法。

2.   通过dom节点的addEventListener方法监听paste动作，并作出相应的js处理

3.   e是一个event对象ClipboardEvent，通过 e.clipboardData 可获取剪贴板中的数据，并且可以通过 clipboard.items[i].type.indexOf('image')来判断数据是不是图片

4.   获取图片文件，通过getAsFile()方法获取图片的二进制数据

5.   读取文件，用FileReader来读取本地文件

      通过 reader.readAsDataURL(imageFile);将图片文件转化为dataURL,即  data:image/png;base64,xxx  形式的url,将它放在image元素的src中可以将本地文件显示出来。（前提是能够拿到本地文件）


 6.  当读取操作成功时，调用 reader.onload，同时,result属性中将包含一个data: URL格式的字符串以表示所读取文件的内容.e对象为 ProgressEvent, this.result（this:reader）就是e.target.result,即图片的base64

 
7.  用base64将图片上传至服务器

     在dataURL中获取base64编码  ，上传至服务器

        var fileBase64 = src.split(",")[1]


8.  从服务器返回图片url后，将图片放在id为images-paste的div中，若要放在光标处，需要另外写js处理


## 兼容性问题
在获取剪切板文件时，只有chrome浏览器可以支持,chrome 通过clipboardData对象获取剪切板中的文件

chrome浏览器的clipboardData.items属性与getAsFile方法结合使用，可获得剪切板中的文件的二进制数据。

浏览器对获取剪贴板文件的支持情况：

|| Firefox 14.0.1          | Chrome 22.0          |Safari 6.0|IE 9.0|Opera 12.01
| ------------- |:-------------:| :-----:|:-----:|:-----:|:-----:|
| paste event      | Yes | Yes|Yes|Yes|No
| paste event on non-editable element      | No | Yes|No|No|No
| clipboardData | No | Yes|Yes|Yes|No
| clipboardData.types   |No |Yes|   Yes|    No  |No
|clipboardData.items    |No |Yes    |No |No |No
|clipboardData.getData  |No |Yes    |Yes    |Yes|No
|clipboardData.setData  |No |Yes    |Yes    |Yes|No
|mime types |No |Yes    |Yes    |No |No
|custom types   |No |Yes    |Yes    |Yes    |No
|event.clipboardData    |No |Yes    |Yes    |No |No
|window.clipboardData   |No |No |No |Yes    |No
 

## 参考文章

1.  [Web API 接口](https://developer.mozilla.org/zh-CN/docs/Web/API )    

2.  [Pasting image from clipboardData object](http://codebits.glennjones.net/copypaste/pasteimagedata.htm)      

3.  [Clipboard API and events](https://w3c.github.io/clipboard-apis/ )        