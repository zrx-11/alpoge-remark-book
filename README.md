# 一纸宣告与两千页重构

Alpöge《Remark》（2026 年 11 月）4 页原文的逐段译注与背景导读，以及《广义黎曼猜想证明（未简化 2000 页）》（实际 2178 页）的可信度评估。网页版把前两部（20 章 + 附录）排成可阅读的单页；第三部原稿保留为 PDF 下载；补编记录 2026 年 11 月公开的 322 页手稿与其 Lean 4 形式化。

站点：https://zrx-11.github.io/alpoge-remark-book/

## 网页版结构

| 区块 | 内容 |
|---|---|
| 阅读指引 / 导读说明 | 这本书由哪三份材料组成、怎么读 |
| 第一部（第 1–9 章） | 黎曼猜想是什么、167 年发展史、五条历史路线、4 页原文逐段对照、术语表、脚注译注 |
| 第二部（第 10–20 章） | 源码规模、主链六层、语句核验、4454 次引用审计、循环性检查、评级 |
| 附录 A–C | 核查清单、未核实项、图表索引 |
| 补编 | 322 页 Dirichlet GRH 手稿；Lean `grh-compact` 内核验证与 Sutherland 复核记录；3 条已公开勘误 |
| 第三部 | 220 卷目录（可搜索）+ 全书 PDF 下载 |

## 补编：322 页手稿与 Lean 验证

2026 年 11 月 25–27 日出现的另一条线，与第二部审的 2178 页中文重构稿无关：

- 手稿 `yuiyuiui/Natural-Language-Proof-Generalized-Riemann-Hypothesis`，322 页，commit `163ad1f`，署名 Levent Alpöge 与 Claude，由 Kaiwen Jin 从 Lean 文本转写。
- 形式化 `lsernau/grh-lean` 的 `grh-compact`，钉 commit `72fead10`：794 个模块、112,827 行；五条定理只依赖 `propext`、`Classical.choice`、`Quot.sound`；`src` 内 `sorry`、`axiom`、`native_decide`、`ofReduceBool` 均为 0。
- 外部复核：Andrew Sutherland（MIT）验证页（2026-11-27）：两版 build 无错、compact 版 794/794 模块内核重放、`lake comparator --paranoid` 在双机通过、nanoda/con-leche/con-ron 接受、Schoenfeld 显式界亦通过。
- 手稿层已知 3 条勘误（issue #1、#4）：§11.11 p.149 的行列式尺寸不匹配、Prop 11.8 与 §§11.14–15 缺失显式前提、§11.15 p.170 跨步推理。三处均属转写，不是 Lean 层问题。

## 为什么第三部不进网页

原稿 2178 页由 LaTeX（xdvipdfmx）排出，公式是 Computer Modern 矢量字形。PDF 抽文会把行内公式拆成碎片（`q^d−1 = Σ_{e|d} eI×_e` 抽出来是“qd −1 / X / e|d / eI× / e”），据此转 HTML 等于把公式打散重拼，错误率无法估计，也无法核对。要真正重排需要 LaTeX 源码，目前只有成品 PDF。所以网页版只提供 220 卷导航，正文看 PDF。

## 页码约定

网页版按章节组织；文中出现的 `p.N`、`第 349 页` 均指 PDF 版页码。PDF 里第三部沿用原书页码（页脚数字），页眉另标“卷 X · 原书 p.N”。

## PDF

`book.pdf`，2227 页，A4，26.1 MB，288 条书签：

- 书内 p.1–44：前两部 + 附录
- p.45：第三部扉页；p.46：使用说明；p.47–49：220 卷目录
- p.50–2227：原稿影印（原书 p.1–2178）

## 本地生成

```bash
# 网页版：由 44 页书稿转排（merged/book.html → index.html）
python3 build_site.py
# PDF：前置 44 页 + 第三部外框 5 页 + 原稿 2178 页
cd ../merged && python3 build.py && /usr/local/bin/weasyprint book.html book.pdf
cd ../fullbook && python3 build_part3.py && /usr/local/bin/weasyprint part3.html part3.pdf && python3 assemble.py
```

## 声明

第一部为翻译与导读，不构成论证；第二部为文本与源码核对，不是数学评审；补编只记录形式化与外部复核结果，不构成对黎曼猜想本身的独立数学判断；第三部内容版权归原稿作者。
