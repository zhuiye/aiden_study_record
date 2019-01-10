# Fetch 接口学习

## 参考链接

https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API/Using_Fetch

## 简单的请求

```javascript
/***
	encodeURIComponent()对值进行编码 
***/
let reqData = "";
for (let key in data) {
    reqData += key + "=" + encodeURIComponent(data[key]) + "&";
}
//表单上传方式
fetch('https:wwww.example.com/webAction', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/x-www-form-urlencoded;charset=UTF-8',
                },
    		   credentials: 'include', //带上cookie凭证
                body: reqData
            })
            .then((response) => response.json())
            .then((responseData) => {
                console.log('responseData', responseData);
                if(successCallback){
                    successCallback(responseData);
                }
            })
            .catch(function(err) {
                if(failCallback){
                    failCallback(err);
                }
            });
```

## 上传JSON数据

```javascript
    {
      method: 'POST', // or 'PUT'
      body: JSON.stringify(data), // data can be `string` or {object}!
      headers: {
              'content-type': 'application/json',
                 
          }
}     

```

## 上传文件

**FormData配合**

```javascript
var formData = new FormData();
var fileField = document.querySelector("input[type='file']");

formData.append('username', 'abc123');
formData.append('avatar', fileField.files[0]);

fetch('https://example.com/profile/avatar', {
  method: 'PUT',
  body: formData
})
.then(response => response.json())
.catch(error => console.error('Error:', error))
.then(response => console.log('Success:', response));
```

## 上传多张文件

```javascript
var formData = new FormData();
var photos = document.querySelector("input[type='file'][multiple]");

formData.append('title', 'My Vegas Vacation');
formData.append('photos', photos.files);

fetch('https://example.com/posts', {
  method: 'POST',
  body: formData
})
.then(response => response.json())
.then(response => console.log('Success:', JSON.stringify(response)))
.catch(error => console.error('Error:', error));
```

