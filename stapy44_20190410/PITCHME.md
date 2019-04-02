## Pythonにおける正規表現の話

### **\w**とUnicodeの意外な関係

2019.04.10 みんなのPython勉強会#44
横山 直敬(@NaoY_py)

---?include=common/whoami_stapy.md

---

## 正規表現の話です

- きっかけ

  - PyQでユーザーさんからの質問が結構多い話題
  
  - 回答する中で自分の正規表現力もちょっと上がった
  
  - 共有しておこう！
  
+++

`\w`の話しかしません

(でも`\W`にも少し触れます)

+++

**正規表現を普段よく使ってる人**


+++

## ググると出てくる\wの定義

**[a-zA-Z_0-9]**

**半角英数とアンダースコア**

+++

**では`\w`は日本語にもマッチする？**

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

`\w`は半角英数とアンダースコアだったのでは……

---

## \wの正体

> ユニコードパターンに対しては、 \w は unicodedata モジュールで提供されている
Unicode データベースで letters としてマークされている全ての文字とマッチします。

[Python3 公式ドキュメント - 正規表現 HOWTO](https://docs.python.org/ja/3/howto/regex.html#matching-characters)

+++

## つまりどういうことだってばよ

Unicode上で`letters` (文字)と定義されてるものが含まれるよ!

+++

## UnicodeのCategory

Unicodeにおいて文字はカテゴリに分かれている

`Letter, Number, Punctuation, ...`

Lx, Nx, Pxと表される

+++

## Letterの種類

- Lu : Uppercase(大文字)

- Ll : Lowercase(小文字)

- Lo : Other(その他)

  - ひらがな, ハングル, ヘブライ文字, アラビア文字……

など

+++

## Loをマッチさせてみる

```python
>>> import re
>>> p = re.compile(r'\w')
>>> some_hebrew_chr = '\u05EA'
>>> some_hebrew_chr
'ת'
>>> p.match(some_hebrew_chr)
<re.Match object; span=(0, 1), match='ת'>
>>> some_arabic_chr = '\u062E'
>>> some_arabic_chr
'خ'
>>> p.match(some_arabic_chr)
<re.Match object; span=(0, 1), match='خ'>
>>> hiragana_a = '\u3042'
>>> p.match(hiragana_a)
<re.Match object; span=(0, 1), match='あ'>
```

+++

## ではアンダースコアはletters？

```python
>>> import unicodedata
>>> unicodedata.name('_')
'LOW LINE'
>>> unicodedata.category('_')
'Pc'
```

`Pc`は`PunctuationConnector`

+++

## ここまでのまとめ

- Pythonでは`\w`がマッチする範囲がとても広い

- UnicodeのLetter + Number + アンダースコア