## 1. 在不同的 web 框架中使用不同的库来校验接收的数据
在 Python 中，对于前后端分离的项目，通常有几种方式来验证用户传递过来的数据：
1. **手动编写验证函数：**
这是最直接的方式，你可以为每个 API 端点编写自定义的数据验证逻辑。这种方法可行但可能会导致大量的重复代码，并且难以维护。
2. **使用 WTForms：**
WTForms 是一个表单验证库，它虽然主要被用于传统的 Flask 应用中与模板结合使用，但也可以在前后端分离的项目中独立使用。WTForms 可以帮助你定义表单类和验证规则，但是它并不专门为 RESTful API 设计。
3. **使用 Marshmallow：**
Marshmallow 是一个 ORM/ODM 框架，它不仅可以用于数据序列化，还可以用于复杂的数据验证。Marshmallow 允许你创建 Schema 对象来定义如何序列化和验证数据。
4. **使用 Django REST framework：**
如果你的后端是基于 Django 构建的，那么 Django REST framework (DRF) 提供了强大的序列化和验证功能。通过 DRF 的序列化器（Serializers），你可以很容易地定义数据的结构，并包含验证规则。
5. **使用 Pydantic：**
用于数据验证和设置管理的现代化库。Pydantic 使用 Python 类型注解来验证数据。它特别适用于 FastAPI，但也可以在其他框架中使用。它非常方便，因为它允许你使用标准的 Python 类型来定义数据模型和验证规则。
6. **使用 Cerberus：**
Cerberus 是一个轻量级且可扩展的数据验证库，它允许你定义一个包含验证规则的 schema 字典。它不依赖于特定的框架，因此可以在任意的 Python 项目中使用。

每种方法都有其优缺点，选择哪种取决于具体需求、所使用的技术栈以及个人偏好。例如：

- 如果正在使用 Flask，可能会倾向于使用 WTForms、Flask-WTF 或 Marshmallow。
- 如果是 Django，Django REST framework 是首选。
- 对于更现代的异步框架（如 FastAPI），Pydantic 是内置支持的。
- 对于简单场景或者需要高度自定义验证逻辑的情况，手动编写验证函数可能更合适。

在实践中，使用现成的库可以节省大量时间，并且这些库通常经过了良好的测试，比起手动编写验证逻辑更可靠。


## 2. 对于 WTForms 的介绍
对于 WTForms 这个插件库，它主要有两个作用：
1. 表单验证
将用户提交上来的数据进行验证是否符合系统要求
2. 模版渲染
模板渲染基本是在前后端不分离的项目中使用的


### 2.1. 安装
安装：
```
pip install wtforms
```

### 2.2. 导入 WTForms 类
在 Python 文件中，导入所需的 WTForms 类，通常是 ```wtforms``` 和 ```wtforms.validators```
```python
from wtforms import Form, StringField, IntegerField
from wtforms.validators import DataRequired, Length
```

### 2.3. 定义表单类
创建一个表单类来定义自己的表单字段。表单类通常继承从 ```wtforms``` 导入的 ```Form```
```python
class ClientRegisterForm(Form):
    # username = StringField("username1", validators=[DataRequired()])   # label 为 username1
    # username = StringField(validators=[DataRequired()])   # label 为 username
    # 如果不设置 label 参数，wtforms 会使用默认的字段名称作为标签文本
    username = StringField(validators=[DataRequired(), Length(
        min=5, max=32
    )])
    password = StringField(validators=[DataRequired(), Length(
        min=6, max=32
    )])
```

### 2.4. 表单类实例化
在视图函数或者在其他函数中，实例化上面所定义的表单类
```python
@app.route('/register', methods=['GET', 'POST'])
def register():
    form = ClientRegisterForm(request.form)
    if request.method == 'POST' and form.validate():
        pass
    return render_template('register.html', form=form)
```

### 2.5. 在模板中使用表单
由于已经通过 ```render_template``` 传递 form 到 ```register.html``` 模板文件中了，现在就可以在模板文件中使用 form 表单了
```
<!-- register.html -->
<form method="POST">
    {{ form.username.label }} {{ form.username() }}
    {{ form.password.label }} {{ form.password() }}
    <input type="submit" value="Submit">
</form>
```

### 2.6. 处理表单的提交
在试图函数中，先获取客户端提交过来的表单数据，然后将其传入自己定义的表单类中，然后调用实例的 validate() 方法，就可以自动验证了
```python
@app.route('/register', methods=['GET', 'POST'])
def register():
    form = ClientRegisterForm(request.form)
    if request.method == 'POST' and form.validate():
        pass
    return render_template('register.html', form=form)
```

### 2.7. 表单验证的规则（WTForms内置的验证器）
```python
from wtforms import Form, StringField, IntegerField
from wtforms.validators import DataRequired, Length

class ClientRegisterForm(Form):
    username = StringField(validators=[DataRequired(), Length(
        min=5, max=32
    )])
    password = StringField(validators=[DataRequired(), Length(
        min=6, max=32
    )])
```
从上面的代码中，我们可以看到，在 **StringField** 类的参数中，有一个 **validators** 的参数，里面有两个导入的验证规则 **DataRequired** 和 **Length** ，这些都是从 **wtforms.validators** 中导入的，是 wtforms 模块中自带的

### 2.8. 表单验证的规则（自定义验证器）
由于 wtforms 内置的验证器并不一定完全符合我们的项目需求，因此可以通过自己定义验证器来验证对应的字段数据。
自定义验证器的步骤：
1. 定义一个方法，方法的名字规则是：```validate_字段名(self, field)```。
2. 在方法中，使用 ```field.data``` 可以获取到这个字段的具体的值。
3. 验证时，如果数据满足条件，那么可以什么都不做。如果验证失败，就应该抛出一个 ```wtforms.validators.ValidationError``` 的异常，并且把验证失败的信息传到这个异常类中。

```python
from wtforms import Form, StringField, IntegerField
from wtforms.validators import DataRequired, Length, ValidationError

class ClientRegisterForm(Form):
    username = StringField(validators=[DataRequired(), Length(
        min=5, max=32
    )])
    password = StringField(validators=[DataRequired(), Length(
        min=6, max=32
    )])
    info = StringField()

    def validate_info(self, field):
        # 自定义信息验证器
        # 如果信息不符合要求，可以通过调用 raise ValidationError('Error message') 来触发验证错误
        if len(field.data) < 6 or len(field.data) > 32:
            raise ValidationError("Error message")
```


## 3. 针对 GET、POST 请求的数据格式在 WTForms 中的使用
### 3.1. 对于 GET 请求（即 url 中携带参数）
通过 **GET** 方法传递 URL 查询参数，可以通过 flask 中的 **request.args** 来查询到，flask 的 **request.args** 就是一个类似于 **MultiDict** 的对象，因此可以直接传给 WTForms
```python
from flask import Flask, request
from WTForms import Form, StringField, validators

app = Flask(__name__)

class SearchForm(Form):
    q = StringField('Search', [validators.Length(min=2, max=50)])


@app.route('/search')
def search():
    form = SearchForm(formdata=request.args)
    if form.validate():
        # 执行搜索逻辑
        pass
    else:
        # 处理验证错误
        pass
```

这里的关键是将 **request.args** 作为 formdata 参数传递给表单类实例。WTForms 将会处理 **request.args** 中的查询字符串，并根据定义的表单字段和验证规则来验证数据。如果查询参数中包含了有效的数据，则 form.validate() 返回 True，否则返回 False。如果验证失败，相关的错误信息会附加到对应的表单字段上。

### 3.2. 对于 POST 请求，传递参数类型为 form 相关
对于 POST 请求传递过来的参数类型为 **multipart/form-data** 或 **application/x-www-form-urlencoded** 的，可以通过 flask 中的 **request.form** 来获取到，**request.form** 是类似于 **MultiDict** 的对象（即 **ImmutableMultiDict** 对象），可以直接传给 WTForms
```python
from flask import request
from WTForms import Form, StringField, validators

app = Flask(__name__)

class SearchForm(Form):
    q = StringField('Search', [validators.Length(min=2, max=50)])


@app.route('/search', methods=["POST"])
def search():
    form = SearchForm(formdata=request.form)
    if form.validate():
        # 执行搜索逻辑
        pass
    else:
        # 处理验证错误
        pass
```

### 3.3. 对于 POST 请求，传递参数类型为 json 相关
对于 post 请求传递过来的参数类型为 **application/json** 的数据，可以通过 flask 中的 **request.json** 或 **request.get_json()** 来获取到，json 类型的数据也可以直接传给 WTForms，但是指定的**关键字参数**就不是 **formdata**，而是 **data**。
```python
from flask import request
from WTForms import Form, StringField, validators

app = Flask(__name__)

class SearchForm(Form):
    q = StringField('Search', [validators.Length(min=2, max=50)])


@app.route('/search', methods=["POST"])
def search():
    data = request.json
    # data = request.get_json()
    form = SearchForm(data=data)
    if form.validate():
        # 执行搜索逻辑
        pass
    else:
        # 处理验证错误
        pass
```
