# 一纸宣告与两千页重构

Alpöge《Remark》（2026 年 11 月，宣告与 Claude 合作证明黎曼猜想并完成 Lean 形式化）的 4 页原文译注与背景导读，以及对《广义黎曼猜想证明（未简化 2000 页）》（实际 2178 页）的可信度评估，附该稿全文影印。

**成品**：一册 PDF，2227 页，A4，26.1 MB，288 条书签。站点首页提供下载。

## 结构

| 书内页 | 内容 |
|---|---|
| 1–44 | 封面、阅读指引、总目录、第一部（第 1–9 章）、第二部（第 10–20 章）、附录 A–C |
| 45 | 第三部扉页 |
| 46 | 第三部使用说明 |
| 47–49 | 第三部分卷目录（220 卷） |
| 50–2227 | 原稿影印（原书 p.1–2178） |

## 页码约定

- 第 1–49 页为本书连续编号。
- 第三部正文沿用**原书页码**（页脚数字），页眉右侧标注“原书 p.N”。第二部中“见第 349 页”“p2178”这类引用因此仍然有效。

## 第三部为什么只重排外框

原稿由 LaTeX（xdvipdfmx）排出，公式为 Computer Modern 矢量字形。PDF 文本抽取会把行内公式拆成碎片，据此重排会破坏数学内容；目前只有成品 PDF，没有 LaTeX 源码。因此第三部原页矢量嵌入、一字不动，另加扉页、使用说明、220 卷目录、逐页页眉与书签。

## 核查要点

- 源码：14 个分卷 229 MB，解包 1.4 GB，175,187 个 `.lean`，17,374,215 行，Lean `v4.33.1`。
- 引用：4,454 次“源码定位”，41 次带文件名（0.9%），10 次带行号（0.2%），0 次可机器解析到声明。
- 空洞：`sorry` / `admit` / `axiom` / `constant` / `native_decide` 计数均为 0；`*_holds` 定理 5,595 条。
- 主链：6 层，`Main.lean:4 riemannHypothesis := rhOfAll` 起，顶层无任何条件参数。
- 关键误读：`OpenLiteratureStatements` 只有 3 个字段，814 行文件里约 200 条 `*_of_literature` 引理均不使用该参数——重构稿“literature 黑箱导致缺口”的推论不成立，literature 是标注层，不是依赖层。
- 原稿自述（p.352）：“作为一份人类可读的稿件，本书尚不能无条件推出黎曼猜想。”

评级：对象存在性 A，结构完整性 A−，可追溯性 C+，自洽性 B−，数学有效性未定。

## 构建

```bash
# 前置 44 页
cd merged && python3 build.py && /usr/local/bin/weasyprint book.html book.pdf
# 第三部外框（扉页/说明/卷目）
cd fullbook && python3 build_part3.py && /usr/local/bin/weasyprint part3.html part3.pdf
# 装订：44 + 5 + 2178，统一页眉、页码、书签
python3 assemble.py
```

依赖：Python 3、WeasyPrint、PyMuPDF、fontTools；原稿 `src_pdf/gjrm.pdf` 与卷目数据 `src_pdf/toc.json`、`vol_ranges.json`。

## 声明

第一部为翻译与导读，不构成论证；第二部为文本与源码核对，不是数学评审；第三部内容版权归原稿作者。本书不判断黎曼猜想本身是否成立。
