## GitPitchのススメ

### スライドをGitで管理しよう

2018.02.07 みんなのPython勉強会#32

横山 直敬(@NaoY_py)

---?include=common/whoami_stapy.md

---

## GitPitchってなによ

- **Markdown**で書いたスライドを**Git**で管理できる

	- GitHub, GitLab, Bitbucketに対応

- **URLを指定するだけでスライドが公開できる**

---

## とても便利

---

## GitPitchの基本

- `PITCHME.md` に中身を書いていく

- リポジトリにPUSHする

- `https://gitpitch.com/user/repo` にアクセス

- スライドが公開できる

---

## GitPitchの独自記法

- 改ページ

- Markdownの挿入

- 画像の挿入

---

## GitPitchの独自記法

- 改ページ(右に追加)

> \---

+++

## GitPitchの独自記法

- 改ページ(下に追加)・このページのこと

- トピックを掘り下げるときに便利 

> +++

+++

## GitPitchの独自記法

- Markdownの挿入

- ルートディレクトリからの絶対パス

- 「お前誰よ」がこれ

> ---?include=foo/bar.md

+++

## GitPitchの独自記法

- 画像の挿入 その1 (インライン埋め込み)

- ルートディレクトリからの絶対パス

> \!\[alt](Slide_1/assets/images/foo.png)

+++?image=assets/images/connpass.png&size=50%

## GitPitchの独自記法

- 画像の挿入 その2 (背景画像)

- ルートディレクトリからの絶対パス

- 改ページの際に指定する

- `&size=` でサイズ指定できる

> ---?image=foo/bar.png


> &size=auto n%
> &size=w% h%

---

## GitPitchのここがハマる

- URL指定

	- スライドごとにディレクトリ分けたい…！
	
	- URL末尾にディレクトリ名をつける
	
`https://gitpitch.com/user/repo?p=Directory/` 

---

## ディレクトリ構成例

```
repo
├ assets
│ ├ css
│ │ └ PITCHME.css
│ └ images
│ 　 └ foo.png
├ common
│ └ whoami.md
├ Slide_1
│ ├ PITCHME.md
│ ├ PITCHME.yaml
│ └ assets
│ 　 └ images
│ 　 　 └ hoge.png
└ README.md
```

``https://gitpitch.com/user/repo?p=Slide_1``

