## スライドをGitHubで管理しよう

### GitPitchのススメ

2018.02.07 みんなのPython勉強会#32

横山 直敬(@NaoY_py)

---?include=common/whoami_stapy.md

---

## GitPitchってなによ

- **Markdown**で書いたスライドを**GitHub**で管理できる

	- GitHub, GitLab, BitBucketにも対応

- URLを指定するだけでスライドが公開できる

---

### とても便利

---

## GitPitchの基本

- `PITCHME.md` に中身を書いていく

---

## ディレクトリ構成例

```
GitPitch_Slides
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