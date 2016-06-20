layout: '[post]'
title: 使用Flask框架返回Json数据
date: 2016-01-04 10:59:16
tags:
- Json
- Flask
- Python 
category:
- Python
---

## Flask
使用Flask框架
{% codeblock Python3 %}
from flask import Flask
from flask import jsonify

app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello World!'

@app.route('/_get_current_user')
def get_current_user():
    return jsonify(username="liyong",
                email="123123",
                id=123)

if __name__ == '__main__':
    app.run()
{% endcodeblock %}


