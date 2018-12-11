## Djangoでdefaultdictがレンダリングできない件

2018.12.11 みんなのPython勉強会#40
横山 直敬(@NaoY_py)

---?include=common/whoami_pyconjp.md

---

### 背景

- (仕事で)defaultdictに格納したデータをtemplateに表示したかった

- どういうわけかレンダリングされない(´・ω・`)

- OrderedDictはレンダリングされる

- バージョンによるもの？

---

### 確かめてみた

- 仕事で使っているバージョン (Django 1.9.3)

- 最新 (Django 2.1.4)

- Jinja2 (念の為)

---

### views

```python
# mysite/myapp/views.py
from collections import defaultdict, OrderedDict

from django.shortcuts import render


def index(request):

# dictionary
normal_d = {
    'foo': 'foo',
    'bar': 'bar',
    'baz': 'baz',
}

# defaultdict(list)
default_d_1 = defaultdict(list)
s = [('yellow', 1), ('blue', 2), ('yellow', 3), ('blue', 4), ('red', 1)]

for k, v in s:
    default_d_1[k].append(v)

# defaultdict(str)
default_d_2 = defaultdict(int)
keys = ['ham', 'spam', 'eggs', 'ham', 'spam', 'spam', 'eggs']

for k in keys:
    default_d_2[k] += 1

# OrderedDict
ordered_d = OrderedDict()
s = [(0, 'zero'), (1, 'one'), (2, 'two')]
for k, v in s:
    ordered_d[k] = v

context = {
    'normal': normal_d,
    'default_d_list': default_d_1,
    'defaukt_d_int': default_d_2,
    'ordered_d': ordered_d,
}

return render(request, 'myapp/index.html', context)
```
    
---
    
### templates
    
```html
<!--mysite/myapp/temaplates/myapp/index.html-->
<html>
<head>
    <title>BP Advent Calendar 2018</title>
</head>
<body>
    <div class="normal">
        <h1>dictionary</h1>
        <table>
            <tr><th>key</th><th>value</th></tr>
            {% for k, v in normal.items %}
            <tr><td>{{ k }}</td><td>{{ v }}</td></tr>
            {% endfor %}
        </table>
    </div>

    <div class="defaultdict-list">
        <h1>defaultdict(list)</h1>
        <table>
            <tr><th>key</th><th>value</th></tr>
            {% for k, v in default_d_list.items %}
            <tr><td>{{ k }}</td><td>{{ v }}</td></tr>
            {% endfor %}
        </table>
    </div>

    <div class="defaultdict-int">
        <h1>defaultdict(int)</h1>
        <table>
            <tr><th>key</th><th>value</th></tr>
            {% for k, v in default_d_int.items %}
            <tr><td>{{ k }}</td><td>{{ v }}</td></tr>
            {% endfor %}
        </table>
    </div>

    <div class="OrderedDict">
        <h1>OrderedDict</h1>
        <table>
            <tr><th>key</th><th>value</th></tr>
            {% for k, v in ordered_d.items %}
            <tr><td>{{ k }}</td><td>{{ v }}</td></tr>
            {% endfor %}
        </table>
    </div>

</body>
</html>
```

---

### Django 1.9.3でやってみる

```
$ pwd
/Users/user/Python/works/advent_cal_2018/django_1.9.3\
$ . env/bin/activate
(env) $ cd mysite/
(env) $ python manage.py runserver
```

[Djnago1.9.3](localhost:8000/)

---

### Django 2.1.4でやってみる

```
$ pwd
/Users/user/Python/works/advent_cal_2018/django_2.1.4\
$ . env/bin/activate
(env) $ cd mysite/
(env) $ python manage.py runserver
```

[Djnago1.9.3](localhost:8001/)

---

**(´・ω・`)**

---

### Jinja2でHTMLをレンダリングする

```python
# jinja2/defaultdict_survey.py
from collections import defaultdict, OrderedDict

from jinja2 import Template

template = """
<html>
<head>
    <title>BP Advent Calendar 2018</title>
</head>
<body>
    <div class="normal">
        <h1>dictionary</h1>
        <table>
            <tr><th>key</th><th>value</th></tr>
            {% for k, v in normal.items() %}
            <tr><td>{{ k }}</td><td>{{ v }}</td></tr>
            {% endfor %}
        </table>
    </div>

    <div class="defaultdict-list">
        <h1>defaultdict(list)</h1>
        <table>
            <tr><th>key</th><th>value</th></tr>
            {% for k, v in default_d_list.items() %}
            <tr><td>{{ k }}</td><td>{{ v }}</td></tr>
            {% endfor %}
        </table>
    </div>
    
    <div class="defaultdict-int">
        <h1>defaultdict(int)</h1>
        <table>
            <tr><th>key</th><th>value</th></tr>
            {% for k, v in default_d_int.items() %}
            <tr><td>{{ k }}</td><td>{{ v }}</td></tr>
            {% endfor %}
        </table>
    </div>

    <div class="OrderedDict">
        <h1>OrderedDict</h1>
        <table>
            <tr><th>key</th><th>value</th></tr>
            {% for k, v in ordered_d.items() %}
            <tr><td>{{ k }}</td><td>{{ v }}</td></tr>
            {% endfor %}
        </table>
    </div>

</body>
</html>
"""

template = Template(template)

# dictionary
normal_d = {
    'foo': 'foo',
    'bar': 'bar',
    'baz': 'baz',
}

# defaultdict
default_d = defaultdict(list)
s = [('yellow', 1), ('blue', 2), ('yellow', 3), ('blue', 4), ('red', 1)]

for k, v in s:
    default_d[k].append(v)

# defaultdict(list)
default_d_1 = defaultdict(list)
s = [('yellow', 1), ('blue', 2), ('yellow', 3), ('blue', 4), ('red', 1)]

for k, v in s:
    default_d_1[k].append(v)

# defaultdict(str)
default_d_2 = defaultdict(int)
keys = ['ham', 'spam', 'eggs', 'ham', 'spam', 'spam', 'eggs']

for k in keys:
    default_d_2[k] += 1

# OrderedDict
ordered_d = OrderedDict()
s = [(0, 'zero'), (1, 'one'), (2, 'two')]
for k, v in s:
    ordered_d[k] = v

context = {
    'normal': normal_d,
    'default_d': default_d,
    'ordered_d': ordered_d,
}

rendered = template.render(
    normal=normal_d,
    default_d_list=default_d_1,
    default_d_int=default_d_2,
    ordered_d=ordered_d)

print(rendered)
```

---

