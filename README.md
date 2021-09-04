# [Flask-wtform](https://wtforms.readthedocs.io/en/2.3.x/)
## [wtf-form](https://wtforms.readthedocs.io/en/2.3.x/)

- [Validators](https://wtforms.readthedocs.io/en/2.3.x/validators/)
- [Fields](https://wtforms.readthedocs.io/en/2.3.x/fields/)

## Creating Form
### main.py
```
from flask_wtf import FlaskForm
from wtforms import StringField, PasswordField
from wtforms.validators import DataRequired, Email, EqualTo, Length

class LoginForm(FlaskForm):
    name = StringField(label='name', validators=[DataRequired()])
    email = StringField(label='email', validators=[DataRequired(), Email(message="error got", granular_message=True)])
    password = PasswordField(label='password', validators=[DataRequired(), EqualTo('confirm', message='Passwords must match'),
                                                           Length(min=5, max=20, message="min 5 and max 20")])
    confirm  = PasswordField(label='Repeat Password')
```

### main.py
```
@app.route("/login", methods=['GET', 'POST'])
def login():
    form = LoginForm()
    # is_summited and validate short form
    if form.validate_on_submit():
        print("click and validate")
        print(request.form)
        return redirect(url_for("success"))

    return render_template("login.html", form=form)
```

### login.html
```
<form method="POST" action="{{url_for('login')}}">
    {{ form.csrf_token }}
    {{ form.name.label }}
    {{ form.name }}<br>
    {{ form.email.label }}
    {{ form.email }}<br>
    {% if form.errors %}
        <ul class=errors>
        {% for error in form.errors['email'] %}
            <li>{{ error }}</li>
        {% endfor %}
        </ul>
    {% endif %}
    {{ form.password.label }}
    {{ form.password }}<br>
    {{ form.confirm.label }}
    {{ form.confirm }}<br>
    {% if form.errors %}
        <ul class=errors>
        {% for error in form.errors['password'] %}
            <li>{{ error }}</li>
        {% endfor %}
        </ul>
    {% endif %}
    <button type="submit">Login</button>
</form>
```
----------------------------------------
## Example

### main.py
```
from flask import Flask, render_template, request, redirect, url_for
from flask_wtf import FlaskForm
from wtforms import StringField, PasswordField
from wtforms.validators import DataRequired, Email, EqualTo, Length

app = Flask(__name__)
app.config.from_object("config.DevelopmentConfig")


class LoginForm(FlaskForm):
    name = StringField(label='name', validators=[DataRequired()])
    email = StringField(label='email', validators=[DataRequired(), Email(message="error got", granular_message=True)])
    password = PasswordField(label='password', validators=[DataRequired(), EqualTo('confirm', message='Passwords must match'),
                                                           Length(min=5, max=20, message="min 5 and max 20")])
    confirm  = PasswordField(label='Repeat Password')


@app.route("/")
def home():
    return render_template("home.html")


@app.route("/success")
def success():
    return render_template("success.html")


@app.route("/login", methods=['GET', 'POST'])
def login():
    form = LoginForm()
    # is_summited and validate short form
    if form.validate_on_submit():
        print("click and validate")
        print(request.form)
        return redirect(url_for("success"))

    return render_template("login.html", form=form)


if __name__ == "__main__":
    app.run()
```

### login.html
```
{% extends "base.html" %}

<!--Title-->
{% block title %}
    My Website
{% endblock %}

<!--Body-->
{% block content %}
    <form method="POST" action="{{url_for('login')}}">
        {{ form.csrf_token }}
        {{ form.name.label }}
        {{ form.name }}<br>
        {{ form.email.label }}
        {{ form.email }}<br>
        {% if form.errors %}
            <ul class=errors>
            {% for error in form.errors['email'] %}
                <li>{{ error }}</li>
            {% endfor %}
            </ul>
        {% endif %}
        {{ form.password.label }}
        {{ form.password }}<br>
        {{ form.confirm.label }}
        {{ form.confirm }}<br>
        {% if form.errors %}
            <ul class=errors>
            {% for error in form.errors['password'] %}
                <li>{{ error }}</li>
            {% endfor %}
            </ul>
        {% endif %}
        <button type="submit">Login</button>
    </form>
{% endblock %}
```


### success.html
```
{% extends "base.html" %}

<!--Body-->
{% block content %}
    <h1>This is Success Page</h1>
{% endblock %}
```

### home.html
```
{% extends "base.html" %}

<!--Title-->
{% block title %}
    My Website
{% endblock %}

<!--Body-->
{% block content %}
    <a href="{{ url_for('login') }}">go to login</a>
{% endblock %}
```






