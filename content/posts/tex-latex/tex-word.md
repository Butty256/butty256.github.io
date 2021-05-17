---
title: "LuaLaTeX でフォント細かく変更する方法（MS Word は使いたくない！）"
date: 2021-02-02
categories: [TeX-LaTeX]
tags: [Qiita,Font,LuaLaTeX]
katex: true
---

この記事は [Qiita](https://qiita.com/Butty256/items/29adc574b15345d8afa2) に投稿した内容です。

# はじめに

課題や予稿，論文等ではフォント等のフォーマット指定がよくあります。MS Word のテンプレートしか無かったり，MS Mincho などの古いフォントを指定されるとイライラします。そこで LaTeX で書きたいと思います。ここではエンジンに LuaLaTeX を使用します。LuaLaTeX は PDF を直接出力することからフォントの埋め込みが容易で新しいイケイケな LaTeX だからです。

# ドキュメントクラス

ドキュメントクラスには `jlreq` を利用します。`jlreq` は日本語組版処理の要件に準拠した新しいドキュメントクラスで，日本に浸透しつつあります。オプションの `fontsize=10.5bp` はデフォルトのフォントサイズを $\mathrm{10.5\\,bp}$ とすることを示しています。`jlreq` では英・和のフォント比がデフォルトで $1$ なので，両方のフォントサイズが $\mathrm{10.5\\,bp}$ になります。MS Word では $1\\,\mathrm{pt}=1/72\\,\mathrm{inch}$ ですが，TeX では $1\\,\mathrm{pt}=1/72.27\\,\mathrm{inch}$ と定義されています。TeX には $1\\,\mathrm{bp}=1/72\\,\mathrm{inch}$ となる単位が存在します。このことから $\mathrm{pt}$ の代わりに $\mathrm{bp}$ を使用しています。

```TeX
\documentclass[fontsize=10.5bp]{jlreq}
```

# フォントの変更

Times New Roman や MS Mincho などを指定されたことを想定して説明します。`luatexja-fontspec` を使えば簡単に設定できます。問題は太字や斜体の設定です。Times New Roman のようにはじめから太字や斜体が用意されているフォントがほとんどですが，MS Mincho などは用意されていません。MS Word などでは MS Mincho を太字にするとき疑似的に太字にしています。LuaLaTeX でもオプションにより疑似太字，疑似斜体を実現することができます。`FakeBold=2`，`FakeSlant=0.33`に設定したとき MS Word と同様の結果になりました。

{{< figure src="../../images/tex-word/fig1.png" width="320" >}}

```TeX
\usepackage{luatexja-fontspec}
\setmainfont[Ligatures={Rare,TeX}]{Times-New-Roman}
\setsansfont{Arial}
\setmainjfont[
	YokoFeatures       = {JFM=jlreq},
	TateFeatures       = {JFM=jlreqv},
	BoldFont           = MS-Gothic,
	BoldFeatures       = {FakeBold=2},
	ItalicFont         = MS-Mincho,
	ItalicFeatures     = {FakeSlant=0.33},
	BoldItalicFont     = MS-Gothic,
	BoldItalicFeatures = {FakeBold=2, FakeSlant=0.33}
]{MS-Mincho}
\setsansjfont[
	YokoFeatures       = {JFM=jlreq},
	TateFeatures       = {JFM=jlreqv},
	BoldFont           = MS-Gothic,
	BoldFeatures       = {FakeBold=2},
	ItalicFont         = MS-Gothic,
	ItalicFeatures     = {FakeSlant=0.33},
	BoldItalicFont     = MS-Gothic,
	BoldItalicFeatures = {FakeBold=2, FakeSlant=0.33}
]{MS-Gothic}
```

次に数式フォントについてです。現在 MS Word では数式フォントに Cambria を使用しています。`unicode-math` を使用すれば簡単に設定できます。

{{< figure src="../../images/tex-word/fig2.png" width="160" >}}

```TeX
\usepackage{unicode-math}
\setmathfont{Cambria-Math}
```

# フォントサイズの変更

`\Large` 等をあらかじめ設定しておくことで，要求された体裁を容易に守ることができます。変更例はこのようになります。また，`vpt` や `xpt` を変更することでデフォルトフォントサイズを変更することもできます。ここでは割愛させてもらいます。

```TeX
\renewcommand\tiny{\@setfontsize\tiny{5bp}{6bp}}
\renewcommand\scriptsize{\@setfontsize\scriptsize{7bp}{8bp}}
\renewcommand\footnotesize{\@setfontsize\footnotesize{8bp}{10bp}}
\renewcommand\small{\@setfontsize\small{9bp}{12bp}}
\renewcommand\normalsize{\@setfontsize\normalsize{10bp}{15bp}}
\renewcommand\large{\@setfontsize\large{12bp}{15bp}}
\renewcommand\Large{\@setfontsize\Large{14.4bp}{16bp}}
\renewcommand\LARGE{\@setfontsize\LARGE{17.28bp}{18bp}}
\renewcommand\huge{\@setfontsize\huge{20.74bp}{30bp}}
\renewcommand\Huge{\@setfontsize\Huge{24.88bp}{36bp}}
```

# まとめ

~~アンチMS Wordな人のために~~ LuaLaTeX での細かいフォント設定についてまとめました。曖昧なところや間違ってるところがあると思うのでその時は指摘してください。ソースコードは下の文献から見に行けます。

# 文献

1. [ソースコード (GitHub)](https://github.com/Butty256/TeX-TikZ-Heatran/)
1. [LuaTeX-ja パッケージ (CTAN)](http://mirrors.ibiblio.org/CTAN/macros/luatex/generic/luatexja/doc/luatexja-ja.pdf)
1. [unicode-math (CTAN)](https://ctan.org/pkg/unicode-math)
1. [Point typography (Wiki)](https://en.wikipedia.org/wiki/Point_(typography))