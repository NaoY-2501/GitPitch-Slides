## スライドをGitHubで管理しよう

### GitPitchのススメ

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

- `https://gitpitch.com/user/repo` にアクセスする

- スライドが公開できる

---

## GitPitchのここがハマる

- URL指定

	- スライドごとにディレクトリ分けたい…！
	
- `https://gitpitch.com/user/repo?p=Directory/` で解決

---

## ディレクトリ構成例

```
Slides
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

``https://gitpitch.com/user/Slides?p=Slide_1``

---

