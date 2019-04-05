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

**正規表現を普段よく使ってる人**


+++

今日は`\w`の話をします

(でも`\W`にも少し触れます)

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

## Unicodeとは
 
世界中の様々な文字に固有の番号をつけて管理している文字の集合

- 番号をUnicodeコードポイントと呼ぶ

- Unicodeコードポイントは16進数で表す

- 例. `U+0061`は`a`を表す

+++

## Python3の文字列はUnicode

> 文字列はUnicode コードポイントのイミュータブルな シーケンス です。

[Python3 組み込み型-テキストシーケンス型](https://docs.python.org/ja/3/library/stdtypes.html?text-sequence-type-str#text-sequence-type-str)

+++

## 文字列とUnicodeを相互に変換する

文字列(str)はUnicodeコードポイントに変換できる

```python
>>> decimal = ord('a')  # 文字のUnicodeコードポイントを整数で返す
>>> decimal
97
>>> hex(decimal)  # 97を16進数に変換する
'0x61'
>>> '\u0061'  # Unicodeとして表現
'a'
>>> chr(decimal)  # Unicodeコードポイントから文字を返す
'a'
```

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
>>> some_hebrew_chr = '\u05EA'  # ヘブライ文字
>>> p.match(some_hebrew_chr)
<re.Match object; span=(0, 1), match='ת'>
>>>
>>> some_arabic_chr = '\u062E'  #アラビア文字
>>> p.match(some_arabic_chr)
<re.Match object; span=(0, 1), match='خ'>
>>>
>>> hiragana_a = '\u3042'  # ひらがな
>>> p.match(hiragana_a)
<re.Match object; span=(0, 1), match='あ'>
```

+++

## 数字は？

- Nd : Decimal Digit(10進数)

- Nl : Letter(数を表す文字)

    - ローマ数字など

- No : Other(その他)

    - 分数など

+++

## 数字をマッチさせてみる

```python
>>> import re
>>> p = re.compile('r\w')
>>> digit_zero = '\u0030'  # Nd:アラビア数字
>>> p.match(digit_zero)
>>> p = re.compile(r'\w')
>>> p.match(digit_zero)
<re.Match object; span=(0, 1), match='0'>
>>>
>>> devanagari_one = '\u0967'  # Nd:デーヴァナーガリー数字(サンスクリット)
>>> p.match(devanagari_one)
<re.Match object; span=(0, 1), match='१'>
>>>
>>> myanmar_two = '\u1042'  # Nd: ミャンマー数字
>>> p.match(myanmar_two)
<re.Match object; span=(0, 1), match='၂'>
>>>
>>> roman_three = '\u2162'  # Nl:ローマ数字
>>> p.match(roman_three)
<re.Match object; span=(0, 1), match='Ⅲ'>
>>>
>>> hangzhou_four = '\u3024'  # Nl:蘇州号碼 
>>> p.match(hangzhou_four)
<re.Match object; span=(0, 1), match='〤'>
>>>
>>> circled_five = '\u2464'  # No:⑤
>>> p.match(circled_five)
<re.Match object; span=(0, 1), match='⑤'>
>>>
>>> superscript_two = '\u00b2'  # No:上付きの2
>>> p.match(superscript_two)
<re.Match object; span=(0, 1), match='²'>
>>> 
```

+++

**「半角英数+アンダースコア」とはなんだったのか**

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

## でもマッチする

```python
>>> import re
>>> p = re.compile(r'\w')
>>> underscore = u'\u005F'
>>> p.match(underscore)
<re.Match object; span=(0, 1), match='_'>
```

+++

## ここまでのまとめ

- Pythonでは`\w`がマッチする範囲がとても広い

- UnicodeのLetter + Number + アンダースコア