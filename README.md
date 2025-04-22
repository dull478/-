# -
把个数加减乘除(还是小白)
from flask import Flask, render_template_string, request

# 创建Flask应用实例
app = Flask(__name__)

# 定义HTML模板，包含计算器的表单和结果显示区域
HTML_TEMPLATE = """
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>简易计算器</title>
    <style>
        body { font-family: Arial, sans-serif; }
        .container { max-width: 400px; margin: 0 auto; padding: 20px; }
        input[type="text"] { width: 100%; padding: 10px; margin-bottom: 10px; }
        button { width: 100%; padding: 10px; background-color: #4CAF50; color: white; border: none; cursor: pointer; }
        button:hover { background-color: #45a049; }
        .result { margin-top: 20px; font-size: 18px; }
    </style>
</head>
<body>
    <div class="container">
        <h1>简易计算器</h1>
        <form method="post">
            <input type="text" name="num1" placeholder="输入第一个数字" required>
            <input type="text" name="num2" placeholder="输入第二个数字" required>
            <select name="operation">
                <option value="add">加法</option>
                <option value="subtract">减法</option>
                <option value="multiply">乘法</option>
                <option value="divide">除法</option>
            </select>
            <button type="submit">计算</button>
        </form>
        {% if result %}
        <div class="result">结果: {{ result }}</div>
        {% endif %}
    </div>
</body>
</html>
"""

# 定义路由处理函数，支持GET和POST请求
@app.route('/', methods=['GET', 'POST'])
def calculator():
    # 初始化结果变量
    result = None
    
    # 如果是POST请求，处理表单数据
    if request.method == 'POST':
        # 获取表单中的数字和操作类型
        num1 = float(request.form['num1'])
        num2 = float(request.form['num2'])
        operation = request.form['operation']
        
        # 根据操作类型执行相应的计算
        if operation == 'add':
            result = num1 + num2
        elif operation == 'subtract':
            result = num1 - num2
        elif operation == 'multiply':
            result = num1 * num2
        elif operation == 'divide':
            # 处理除法时检查除数是否为零
            if num2 != 0:
                result = num1 / num2
            else:
                result = "错误：除数不能为零"
    
    # 渲染HTML模板并传递结果
    return render_template_string(HTML_TEMPLATE, result=result)

# 启动Flask应用，开启调试模式
if __name__ == '__main__':
    app.run(debug=True)
