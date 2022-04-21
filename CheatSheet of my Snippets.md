# Snippets Cheat sheet

我的 Ultisnips 环境配置。

### Overview

| 描述               | `key` $\to$ `TeX`               | 描述              | `key` $\to$ `TeX`             |
| ------------------ | ------------------------------- | ----------------- | ----------------------------- |
| begin              | `beg` $\to$ `\begin`            | inline math       | `mk`  $\to$ `$$`              |
| display math       | `dm` $\to$ `\[\]`               | 下标              | `x1` $\to$ `x_1`              |
| 下标               | `x12` $\to$ `x_{12}`            | 上标2             | `sr` $\to$ `^2`               |
| 上标3              | `cb` $\to$ `^3`                 | 上标              | `td` $\to$ `^{}`              |
| fraction           | `1/` $\to$ `\frac{1}{}`         | fraction          | `4pi/` $\to$ `\frac{4pi}{}`   |
| fraction           | `(1 + 2)` $\to$ `\frac{1+2}{}`  | bar               | `bar` $\to$ `\bar`            |
| bar                | `zbar` $\to$ `\bar{z}`          | hat               | `hat` $\to$ `\hat`            |
| hat                | `zhat` $\to$ `\hat{z}`          | vector            | `v,.` / `v.,` $\to$ `\vec{v}` |
| Dirac bra          | `<a|` $\to$ `\bra{a}`           | Dirac ket         | `|a>` $\to$ `\ket{a}`         |
| Dirac braket       | `<a|b>` $\to$ `\braket{a}{b}`   | $\implies$        | `=>` $\to$ `\implies`         |
| $\impliedby$       | `=<` $\to$ `\impliedby`         | $\neq$            | `!=` $\to$ `\neq`             |
| $\geq$             | `>=` $\to$ `\ge`                | $\le$             | `<=` $\to$ `\le`              |
| $\to$              | `->` $\to$ `\to`                | $\Leftrightarrow$ | `<->` $\to$ `\Leftrightarrow` |
| $\mapsto$          | `!>` $\to$ `\mapsto`            | $\gg$             | `>>` $\to$ `\gg`              |
| $\ll$              | `<<` $\to$ `\ll`                | $\sim$            | `~~` $\to$ `\sim`             |
| $\forall$          | `AA` / `forall` $\to$ `\forall` | $\exists$         | `EE` $\to$ `\exists`          |
| $\mid$             | `||` $\to$ `\mid`               | $\leftarrow$      | `<-` $\to$ `\leftarrow`       |
| $\langle a\rangle$ | `<>` $\to$ `\angbr`             |                   |                               |

`dint`
$$
\int_{-\infty}^{+\infty} \text{something} \,\dd x
$$
`dfun`
$$
f \colon \mathbb{R} \to \mathbb{R},\ x\mapsto \text{something}
$$


## $\LaTeX$映射

### Environments

```tex
snippet beg "begin{} / end{}" bA
\begin{$1}
	$0
\end{$1}
endsnippet
```

Other environments

| Description | Mapping            | Description | Mapping         |
| ----------- | ------------------ | ----------- | --------------- |
| defination  | `EDE` $\to$ defi   | theorem     | `ETH` $\to$ thm |
| corollary   | `ECO` $\to$ cor    | property    | `EPT` $\to$ prt |
| remark      | `ERE` $\to$ remark | proposition | `EPP` $\to$ prp |
| law         | `ELA` $\to$ law    |             |                 |



### Inline and display math

```tex
snippet mk "Math" wA
$${1}$`!p
if t[2] and t[2][0] not in [',', '.', '?', '-', ' ']:
    snip.rv = ' '
else:
    snip.rv = ''
`$2
endsnippet

snippet dm "Math" wA
\[
    $1
.\] $0
endsnippet
```

### Sub- and superscripts

```tex
snippet '([A-Za-z])(\d)' "auto subscript" wrA
`!p snip.rv = match.group(1)`_`!p snip.rv = match.group(2)`
endsnippet

snippet '([A-Za-z])_(\d\d)' "auto subscript2" wrA
`!p snip.rv = match.group(1)`_{`!p snip.rv = match.group(2)`}
endsnippet

snippet sr "^2" iA
^2
endsnippet

snippet cb "^3" iA
^3
endsnippet

snippet td "superscript" iA
^{$1}$0
endsnippet

priority 100
context "math()"
snippet "\b([a-zA-Z])ii" "?i" riA
`!p snip.rv=match.group(1)`_{i}
endsnippet
```

### Fractions

```tex
snippet '((\d+)|(\d*)(\\)?([A-Za-z]+)((\^|_)(\{\d+\}|\d))*)/' "Fraction" wrA
\\frac{`!p snip.rv = match.group(1)`}{$1}$0
endsnippet

priority 1000
snippet '^.*\)/' "() Fraction" wrA
`!p
stripped = match.string[:-1]
depth = 0
i = len(stripped) - 1
while True:
	if stripped[i] == ')': depth += 1
	if stripped[i] == '(': depth -= 1
	if depth == 0: break;
	i -= 1
snip.rv = stripped[0:i] + "\\frac{" + stripped[i+1:-1] + "}"
`{$1}$0
endsnippet
```

### Postfix

In *Quantum Mechanics* `\hbar` may lead to a conflict.

```tex
priority 10
context "math()"
snippet "bar" "bar" riA
\overline{$1}$0
endsnippet

priority 100
context "math()"
snippet "([a-zA-Z])bar" "bar" riA
\overline{`!p snip.rv=match.group(1)`}
endsnippet

priority 10
context "math()"
snippet "hat" "hat" riA
\hat{$1}$0
endsnippet

priority 100
context "math()"
snippet "([a-zA-Z])hat" "hat" riA
\hat{`!p snip.rv=match.group(1)`}
endsnippet

context "math()"
snippet "(\\?\w+)(,\.|\.,)" "Vector postfix" riA
\vec{`!p snip.rv=match.group(1)`}
endsnippet
```

### Dirac Brakets

```tex
context "math()"
snippet "\<(.*?)\|" "bra" riA
\bra{`!p snip.rv = match.group(1).replace('q', f'\psi').replace('f', f'\phi')`}
endsnippet

context "math()"
snippet "\|(.*?)\>" "ket" riA
\ket{`!p snip.rv = match.group(1).replace('q', f'\psi').replace('f', f'\phi')`}
endsnippet

context "math()"
snippet "(.*)\\bra{(.*?)}([^\|]*?)\>" "braket" riA
`!p snip.rv = match.group(1)`\braket{`!p snip.rv = match.group(2)`}{`!p snip.rv = match.group(3).replace('q', f'\psi').replace('f', f'\phi')`}
endsnippet
```

### 'Arrows'

```tex
snippet => "implies" Ai
\implies
endsnippet

snippet =< "implied by" Ai
\impliedby
endsnippet

context "math()"
snippet != "equals" iA
\neq 
endsnippet

context "math()"
snippet <= "leq" iA
\leq
endsnippet

context "math()" 
snippet >= "geq" iA
\geq
endsnippet

priority 100
context "math()"
snippet -> "to" iA
\to 
endsnippet

snippet <- "leftarrow" iA
\leftarrow
endsnippet

priority 200
context "math()"
snippet <-> "leftrightarrow" iA
\Leftrightarrow 
endsnippet

priority 200
context "math()"
snippet <> "lrangle" iA
\angbr
endsnippet

context "math()"
snippet !> "mapsto" iA
\mapsto 
endsnippet

context "math()"
snippet >> ">>" iA
\gg
endsnippet

context "math()"
snippet << "<<" iA
\ll
endsnippet


context "math()"
snippet ~~ "~" iA
\sim 
endsnippet

context "math()"
snippet || "mid" iA
 \mid 
endsnippet
```

### 量词

```tex
priority 100
context "math()"
snippet "(forall|AA|∀)" "forall" riA
\forall
endsnippet

context "math()"
snippet EE "exist" iA
\exists
endsnippet
```

### $\int$ and $f$

```
priority 300
context "math()"
snippet dint "integral" wA
\int_{${1:-\infty}}^{${2:\infty}} ${3:${VISUAL}} \,\dd $0
endsnippet

snippet dfun "Definition of function" iA
$1\colon ${2:\mathbb{R}} \to ${3:\mathbb{R}},\ ${4:x} \mapsto $0
endsnippet
```

### Fast Changing

```
snippet "(\\(tiny|scriptsize|footnotesize|small|normalsize|large|Large|LARGE|huge|Huge))" "next type of font size" wr
`!p
symbol_list = FontSizeSequence
index = symbol_list.index(match.group(1))
snip.rv = symbol_list[(index + 1) % len(symbol_list)]
`
endsnippet

context "math()"
snippet "(\\(leftarrow|longleftarrow|Leftarrow|Longleftarrow|xleftarrow))" "next type of left arrow" wr
`!p
symbol_list = LeftArrows
index = symbol_list.index(match.group(1))
snip.rv = symbol_list[(index + 1) % len(symbol_list)]
`
endsnippet

context "math()"
snippet "(\\(rightarrow|longrightarrow|Rightarrow|Longrightarrow|xrightarrow))" "next type of right arrow" wr
`!p
symbol_list = RightArrows
index = symbol_list.index(match.group(1))
snip.rv = symbol_list[(index + 1) % len(symbol_list)]
`
endsnippet

context "math()"
snippet "(\\(leftrightarrow|longleftrightarrow|Leftrightarrow|Longleftrightarrow|iff))" "next type of left right arrow" wr
`!p
symbol_list = LeftrightArrows
index = symbol_list.index(match.group(1))
snip.rv = symbol_list[(index + 1) % len(symbol_list)]
`
endsnippet
```



##  配置 global 环境

 定义数学环境：

```python
global !p
texMathZones = ['texMathZone'+x for x in ['A', 'AS', 'B', 'BS', 'C',
'CS', 'D', 'DS', 'E', 'ES', 'F', 'FS', 'G', 'GS', 'H', 'HS', 'I', 'IS',
'J', 'JS', 'K', 'KS', 'L', 'LS', 'DS', 'V', 'W', 'X', 'Y', 'Z']]

texIgnoreMathZones = ['texMathText']

texMathZoneIds = vim.eval('map('+str(texMathZones)+", 'hlID(v:val)')")
texIgnoreMathZoneIds = vim.eval('map('+str(texIgnoreMathZones)+", 'hlID(v:val)')")

ignore = texIgnoreMathZoneIds[0]

def math():
    synstackids = vim.eval("synstack(line('.'), col('.') - (col('.')>=2 ? 1 : 0))")
    try:
        first = next(
            i for i in reversed(synstackids)
            if i in texIgnoreMathZoneIds or i in texMathZoneIds
        )
        return first != ignore
    except StopIteration:
        return False
endglobal
```

定义 Sequence：

```python
HatSequence = r"\overline \bar \hat \tilde \widetilde \widehat".split()
DotsSequence = r"\ldots \cdots \vdots \ddots".split()
FontSizeSequence = r"\tiny \scriptsize \footnotesize \small \normalsize \large \Large \LARGE \huge \Huge".split()
SpaceSequence = r"\,|\:|\;|\ |\quad|\qquad|\!".split("|")
LeftArrows = r"\leftarrow|\longleftarrow|\Leftarrow|\Longleftarrow|\xleftarrow".split("|")
RightArrows = r"\rightarrow|\longrightarrow|\Rightarrow|\Longrightarrow|\xrightarrow".split("|")
LeftrightArrows = ["\\" + x for x in "leftrightarrow|longleftrightarrow|Leftrightarrow|Longleftrightarrow|iff".split("|")]
```

## References

1. https://castel.dev/post/lecture-notes-1/,  作者：[Gilles Castel](https://castel.dev/)
2. https://github.com/sillybun/zyt-snippet, 作者：sillybun（知乎：张同学）
