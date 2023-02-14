## （一） Flask 视图
当一个 url 请求进入后台时，一般有两种方式来进行处理：**视图函数**和**视图类**。

视图函数直接使用一个函数来进行处理并返回数据给浏览器，视图类则是使用类来进行处理并返回的，所以当需要进行的处理比较简单，则可以考虑使用前者，处理比较复杂就考虑使用后者，但是最终还是需要根据使用环境和需求而定。

### 1. 视图函数
#### （1） 说明
视图函数直接使用一个函数来进行处理并返回数据给浏览器
#### （2） 注册方式
##### ① 使用 ```app.route``` 装饰器对函数进行注册
代码示例：
```py
# app.route(rule, endpoint, methods, ...)

@app.route('/', endpoint='index')
def index():
    return 'Hello World'
```
##### ② 使用 ```add_url_rule``` 进行注册
代码示例：
```py
# add_url_rule(rule, endpoint, view_func, methods, ...)

def index():
    return 'Hello World'

app.add_url_rule('/', endpoint='index', view_func=index)
```

### 2. 视图类
#### （1） 说明
上面的视图函数的示例简单而灵活，但是如果要把这个视图变成一个可以用于其他模型和模板的通用视图，那么这个视图还是不够灵活。因此，我们就需要引入可插拨的、基于类的视图。
#### （2） 注册方式
##### ① 标准视图类
```Flask.views.View``` 是 Flask 的标准视图类，用户定义的视图类需要继承于 ```Flask.views.View``` 。使用视图类的步骤如下：
1. 用户定义一个视图类，继承于 ```Flask.views.View```
2. 在视图类中定义方法 ```dispatch_request```，处理请求、返回 HTML 文本给客户端
3. 使用 ```app.add_url_rule(rule, view_func)``` 将 URL 路径和视图类绑定

---

代码示例：
```py
from flask import Flask, views

app = Flask(__name__)

class IndexView(views.View):
    """
    自定义视图类，继承 views.View
    """
    def dispatch_request(self):
        print('app.url_map = {}'.format(app.url_map))
        return 'hello world'

# 将路由规则和视图类 IndexView 进行绑定
app.add_url_rule(rule='/', view_func=IndexView.as_view("index"))
# app.add_url_rule(rule='/', view_func=IndexView.as_view("index"), methods=['POST'])
# as_view 函数：视图类的本质是视图函数，函数 View.as_view() 会返回一个视图函数
# IndexView.as_view("index") 代表创建了一个名称为 index 的视图函数
# app.add_url_rule 实际上是将路由规则和视图函数（有视图类 IndexView 的 as_view 方法转换而来）绑定
```

---

使用视图类的好处：
- 支持继承，可以把一些共性的东西放在父类中，其他子类可以继承

app.py 代码
```py
from flask import Flask, views, render_template

app = Flask(__name__)

class BaseView(views.View):
    # 如果子类忘记定义 get_template 就会报错
    def get_template(self):
        raise NotImplementedError()

    # 如果子类忘记定义 get_data 就会报错
    def get_data(self):
        raise NotImplementedError()

    def dispatch_request(self):
        # 获取模板需要的数据
        data = self.get_data()
        # 获取模板文件路径
        template = self.get_template()
        # 渲染模板文件
        return render_template(template, data=data)

class IndexView(BaseView):
    def get_template(self):
        return "index.html"

    def get_data(self):
        return {
            'title': '首页'
        }

app.add_url_rule('/', view_func=IndexView.as_view('index'))
```

index.html 代码
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    title = {{ data['title'] }}
</body>
</html>
```
##### ② 请求视图类
对于 REST 式的 API 来说，为每种 HTTP 方法提供相对应的不同函数显得尤为有用。使用 ```flask.views.MethodView``` 可以轻易做到这点。在这个类中，每个 HTTP 方法 都映射到一个同名的类方法（名称为小写字母），当对应的 url 访问进来后就会根据 url 的请求方式查找对应的请求方法，并将该方法的返回值返回给浏览器，如果没有定义对应的请求方法就会报错，提示 ```Method Not Allowed```

代码示例：
```py
from flask import Flask, views, jsonify

app = Flask(__name__)

class UserAPI(views.MethodView):
    def get(self):
        # user = User.query.all()
        user = {
            'username': 'whxcer',
            'gender': '男'
        }
        return jsonify(user)

    def post(self):
        # user = User.from_form_data(request.form)
        pass

app.add_url_rule('/user', view_func=UserAPI.as_view('users'))
```

使用这种方式，不必提供 methods 属性，它会自动使用相应的类方法。


## （二） 参考
[Flask（10）- 标准类视图](https://www.cnblogs.com/poloyy/p/15008909.html)
[Flask 文档-可插拨视图](https://dormousehole.readthedocs.io/en/latest/views.html)
