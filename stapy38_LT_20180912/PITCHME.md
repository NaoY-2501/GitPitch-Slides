## PythonでQRコード作ってHTMLに表示する

2018.09.12 みんなのPython勉強会#38
横山 直敬(@NaoY_py)

---?include=common/whoami_pyconjp.md

---

## QRコードを作るライブラリ

- **qrcode**

サンプルコード

```python
import qrcode
qr = qrcode.QRCode(
    version=1,
    error_correction=qrcode.constants.ERROR_CORRECT_L,
    box_size=10,
    border=4,
)
qr.add_data('Some data')
qr.make(fit=True)

img = qr.make_image(fill_color="black", back_color="white")
```

---

## 生成できる画像

- `PNG`や`SVG`

---

QRコードをWebページに表示したい

---

SVGはXMLで表現できる

---

## やってみた

続きはターミナル上で