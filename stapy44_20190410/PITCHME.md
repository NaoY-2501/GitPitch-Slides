## Pythonにおける正規表現の話

### **\w**とUnicodeの意外な関係

2019.04.10 みんなのPython勉強会#44
横山 直敬(@NaoY_py)

---?include=common/whoami_stapy.md

---

**正規表現を普段よく使ってる人**


+++

**`\w`は日本語にもマッチする？**

+++

## 実際にやってみる

```python
$ python3
Python 3.7.0 (default, Jun 29 2018, 20:14:27)
[Clang 9.0.0 (clang-900.0.39.2)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import re
>>>
>>> p = re.compile(r'[\w]+')
>>> s = '平成'
>>> p.match(s)
<re.Match object; span=(0, 2), match='平成'>
```

+++

`\w`は半角英数とアンダースコアだったのでは…

