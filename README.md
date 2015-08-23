# 新浪微博 python SDK.

Folk 自 https://github.com/lxyu/weibo 并做了如下修改：

* 调用接口时可传入参数 requests
* 多返回值：`res, error = c.get('users/show',uid=12345)`
* 使用 requests 进行相关网络操作，因而相对于官方的 sdk，极大的简化了代码，并提高了可读性。

# 注意事项

因为我改成了多返回值，因此，在调用时不能再像原作者那样只使用一个返回值。

# 安装

```bash
git clone https://github.com/shajiquan/weibo
```

# 调用：

```python
_reqs = {
    "timeout":(1,2)
}
from weibo import Client
c = Client(API_KEY, API_SECRET, REDIRECT_URI)
res, error = c.get('users/show', uid=1282440983,_reqs=_reqs)

if error:
    raise ValueError("balabala")

```

下文是 https://github.com/lxyu/weibo 的原文：

---

并不十分华丽的分隔线

---

## 安装

推荐使用 pip 安装。

`pip install weibo`

## 使用说明

### 准备工作

* 首先，注册一个新浪开发者帐号，并在 [新浪开发平台](http://open.weibo.com/apps) 新建一个 app.
* 创建好应用之后在 '应用信息 -> '基本信息' 里面获取 App Key 和 App Secret.
* 在 '应用信息 -> '高级信息' 里面设置好 '授权回调页'.

### 示例

下面分别用 API_KEY, API_SECRET, REDIRECT_URI 代表准备工作里面的三个参数。

#### Token 认证

```python
>>> from weibo import Client
>>> c = Client(API_KEY, API_SECRET, REDIRECT_URI)
>>> c.authorize_url
'https://api.weibo.com/oauth2/authorize?redirect_uri=http%3A%2F%2F127.0.0.1%2Fcallback&client_id=123456'
```

复制链接到浏览器打开，获取 code.

```python
>>> c.set_code('abcdefghijklmn')
```

client 初始化完成。token 可以被保存下来供下次调用时直接使用。

```python
>>> token = c.token
>>> c2 = Client(API_KEY, API_SECRET, REDIRECT_URI, token)
>>> c2.get('users/show', uid=2703275934)
```

#### 帐号认证

除了使用 token 认证，还可以使用 username / password 进行认证。

```python
>>> from weibo import Client
>>> c = Client(API_KEY, API_SECRET, REDIRECT_URI, username='admin', password='secret')
>>> c.get('users/show', uid=1282440983)
```

#### 接口调用

参考 [微博开发文档](http://open.weibo.com/wiki/API%E6%96%87%E6%A1%A3_V2) 进行接口调用。

```python
>>> c.get('users/show', uid=1282440983)
>>> c.post('statuses/update', status='python sdk test, check out http://lxyu.github.io/weibo/')
```

client 兼容上传图片接口。

```python
>>> f = open('avatar.png', 'rb')
>>> c.post('statuses/upload', status='new avatar!', pic=f)
```