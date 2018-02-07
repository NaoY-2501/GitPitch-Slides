## GitPitchのススメ

### スライドをGitで管理しよう

2018.02.07 みんなのPython勉強会#32

横山 直敬(@NaoY_py)

---?include=common/whoami_stapy.md

---

## GitPitchってなによ

- [GitPitch](https://gitpitch.com/)

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

- **改ページ(右に追加)**

> \---

+++

## GitPitchの独自記法

- **改ページ(下に追加)**  (このページのこと)

- トピックを掘り下げるときに便利 

> +++

+++

## GitPitchの独自記法

- **Markdownの挿入**

- ルートディレクトリからの絶対パス

- 「お前誰よ」がこれ

> ---?include=foo/bar.md

+++

## GitPitchの独自記法

- **画像の挿入** その1 (インライン埋め込み)

- ルートディレクトリからの絶対パス

> \!\[alt](Slide_1/assets/images/foo.png)

+++

## GitPitchの独自記法

- **画像の挿入** その2 (背景画像)

- ルートディレクトリからの絶対パス

- 改ページで指定する(下方向の改ページもOK)

- `size` オプション でサイズ指定できる

	- `&size=auto n% , &size=w% h%` など

> ---?image=foo/bar.png

---

## CSS

- CSSが使える

	- 書式周りの設定が多い

-  ``PITCHME.yaml`` でCSSを設定

```
theme-override : assets/css/PITCHME.css
```
PITCHME.yaml

+++

## CSS

- とりあえず書くと捗る

```
/* 画像の外枠なし*/
.reveal section img{
    border: 0;
    box-shadow: none;
}

/* 見出しの頭文字だけ大文字にする*/
.reveal h1,
.reveal h2,
.reveal h3,
.reveal h4,
.reveal h5,
.reveal h6 {
    text-transform: none;
}
```
PITCHME.css

---

## PITCHME.yaml

- 様々な設定を書くファイル

	- ``PITCHME.md`` と同じ階層に置く

	- CSS, テーマ, スライド時アニメーション
	
	- スライドマスター設定がコードでできる
	
- 続きは[公式ドキュメント](https://github.com/gitpitch/gitpitch/wiki)で

---

## GitPitchのここがハマる

- スライドごとにディレクトリ分けたい…！

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

+++

## GitPitchのここがハマる

- これで解決できる

	- URL末尾にディレクトリ名をつける
	
`https://gitpitch.com/user/repo?p=Slide_1/`

---

## GitPitchはいいぞ

---

## 参考

- [GitPitch Wiki](https://github.com/gitpitch/gitpitch/wiki)

- [NaoY-2501/GitPitch-Slides](https://github.com/NaoY-2501/GitPitch-Slides)

