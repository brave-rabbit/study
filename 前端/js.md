# 前端访问后端数据

接口是定义好的

```js
<script>

        $.ajax({
            // 设置要访问的地址
            url:'http://apis.juhe.cn/goodbook/catalog',
            // 要传递的参数
            data:{
                key:'2fc5d10107e863a36a6085ab93715c63'
            },
            // 解决跨域
            dataType:'jsonp',
            // 获取请求到的数据
            success:function(res){
                console.log(res.result)
                let str = '';
                // 拿到的数据循环遍历出来
                for(var i = 0;i<res.result.length;i++){
                    str += '<li>'+res.result[i].catalog+'</li>'
                }

                // 渲染到页面
                $('#list').html(str)
            }
        })
    </script>
```





```js
function logining(){
    /*兄弟页面刷新*/
    // parent.frames[1].location.reload();
}
```

