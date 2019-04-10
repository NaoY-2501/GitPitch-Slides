## Pythonにおける正規表現の話

### **\w**とUnicodeの意外な関係

2019.04.10 みんなのPython勉強会#44
横山 直敬(@NaoY_py)

---?include=common/whoami_stapy.md

---

## 正装はじめました

今日の正装

**sora tob sakana**

---

## 正規表現の話です

- きっかけ

  - PyQでユーザーさんからの質問が結構多い話題
  
  - 回答のために調査していたら自分の正規表現力もちょっと上がった
  
  - 共有しておこう！
  

+++

**正規表現、使ってますか？**


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
>>> p = re.compile(r'\w+')
>>> s = '平成'
>>> p.match(s)
<re.Match object; span=(0, 2), match='平成'>
```

+++

`\w`は半角英数とアンダースコアだったのでは……

+++

## 参考. JavaScriptの\w

半角英数とアンダースコアだけマッチする。

```javascript
const re = /\w+/;
'abc'.match(re);
["abc", index: 0, input: "abc", groups: undefined]0: "abc"groups: undefinedindex: 0input: "abc"length: 1__proto__: Array(0)
'123'.match(re);
["123", index: 0, input: "123", groups: undefined]
'ab_123'.match(re);
["ab_123", index: 0, input: "ab_123", groups: undefined]
'Ab_123'.match(re);
["Ab_123", index: 0, input: "Ab_123", groups: undefined]
'ほげ'.match(re);
null
'①'.match(re);
null
```

---

## Pythonにおける\wの正体

> ユニコードパターンに対しては、 \w は unicodedata モジュールで提供されている
Unicode データベースで letters としてマークされている全ての文字とマッチします。

[Python3 公式ドキュメント - 正規表現 HOWTO](https://docs.python.org/ja/3/howto/regex.html#matching-characters)

+++

## つまり……

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
>>> superscript_two = '\u00B2'  # No:上付きの2
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

## Q. すべてのLetter, Numberにマッチする？

A. 一部例外もある

```
unicode_letters.csv
match : 19978/20188
unmatch : 210/20188
unicode_numbers.csv
match : 1653/1754
unmatch : 101/1754
unicode_punctuations.csv
match : 1/156
unmatch : 155/156
```

リポジトリは後で貼っておきます！

+++

## ここまでのまとめ

- Pythonでは`\w`がマッチする範囲がとても広い

- UnicodeのLetter + Number + アンダースコア

    -  **20,000**種類以上にマッチする

---

## \Wのマッチする範囲

- 逆説的に`\w`ではマッチしない範囲

    - `\w`よりもずっと広い

- カテゴリ: Mark, Other, Punctuation, Separator, Symbol

+++

```python
>>> import re
>>> p = re.compile(r'\W')
>>> p.match('\u0061')  # Letter アルファベットa
>>>
>>> p.match('\u0030')  # Number アラビア数字0
>>>
>>> p.match('\u0CCA')  # Mark カンナダ語(インド)の母音記号(O)
<re.Match object; span=(0, 1), match='ೊ'>
>>> p.match('\u0000')  # Other Null
<re.Match object; span=(0, 1), match='\x00'>
>>> p.match('\u0040')  # Punctuation アットマーク
<re.Match object; span=(0, 1), match='@'>
>>> p.match('\u0020')  # Separator スペース
<re.Match object; span=(0, 1), match=' '>
>>> p.match('\u2318')  # Symbol Macのcmd記号(PLACE OF INTEREST)
<re.Match object; span=(0, 1), match='⌘'>
```

---

## \wが活きるシーン

- 複数の言語が混じったテキスト

- 一定の形式から情報を抽出する

+++

## 例. 言語が混じったテキスト

```python
>>> import re
>>> # 伊藤計劃の著作リスト
>>> project_itoh = [
...     '【日本語】虐殺器官(2007)',
...     '【한국어】하모니(2008)',  # ハーモニー
...     '【中文】屍者的帝國(2012)'  # 屍者の帝国
... ]
>>> # "言語, タイトル, 日本での出版年"でグルーピングする
>>> p = re.compile(r'【(\w+)】(\w+)\((\d+)\)')
>>> for each in project_itoh:
...     p.match(each).groups()
...
('日本語', '虐殺器官', '2007')
('한국어', '하모니', '2008')
('中文', '屍者的帝國', '2012')
```

---

## 漢字だけマッチさせたい

- ググるとよく出てくる `[一-龠]`は漢字すべてを網羅していない

- 例えば `々` が含まれていない

    - `Lm`に分類される
    
    - LetterModifier(修飾子)

なにかいい方法は……

+++

## Unicode文字プロパティを使う

- `ISO 15924`(文字体系)のUnicodeエイリアスを指定してマッチさせられる

- `regex`モジュールが対応している 

- 漢字は`\p{Han}`で表す

+++

## regexを使ってみる

`pip install regex`

```python
import regex
>>> regex.findall(r'\p{Han}', '令和')
['令', '和']
>>> regex.findall(r'\p{Han}', '堂々')  # 々もマッチする
['堂', '々']
>>> regex.findall(r'\p{Hiragana}', 'あ')
['あ']
>>> regex.findall(r'\p{Katakana}', 'ア')
['ア']
>>> regex.findall(r'\p{Arabic}', '\u06FF')
['ۿ']
```

---

## 正規表現と上手く付き合う

- 文字・数字を広くマッチさせたいときは `\w`

- ひらがな・カタカナだけ

    - `[ぁ-ん]`, `[ァ-ン]`を使う
    
    - Unicode文字プロパティも有効
    
- 漢字だけ

    - Unicode文字プロパティがbetter
    
---

## Reference(1)

- [Python3 Unicode HOWTO](https://docs.python.org/ja/3/howto/unicode.html)

- [Python3 正規表現 HOWTO](https://docs.python.org/ja/3/howto/regex.html)

- [Python3 組み込み型-テキストシーケンス型](https://docs.python.org/ja/3/library/stdtypes.html?text-sequence-type-str#text-sequence-type-str)

- [PyPI regex](https://pypi.org/project/regex/)

+++

## Reference(2)

- [Wikipedia Unicode](https://ja.wikipedia.org/wiki/Unicode)

- [Wikipedia Unicode Character Property](https://en.wikipedia.org/wiki/Unicode_character_property)

- [Wikipedia ISO 15924](https://ja.wikipedia.org/wiki/ISO_15924)

- [TechRacho \[連載:正規表現\] Unicode文字プロパティについて(1)](https://techracho.bpsinc.jp/hachi8833/2013_09_13/13433)

+++

## Reference(3)

- [FileFormat.info Unicode Charancter Categories](https://www.fileformat.info/info/unicode/category/index.htm)

- [Qiita 正規表現での漢字マッチをUnicodeプロパティーを使って綺麗に書く方法 in Python](https://qiita.com/mhangyo/items/c567f93b0e20b2e6af04)