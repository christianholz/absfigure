# absfigure: Absolute figure and table placement for LaTeX

LaTeX's float mechanism (`\begin{figure}[t/b/h/p]`) only provides *relative* placement hints. There is no built-in way to say "put this figure at the bottom of page 6, column 2."

**absfigure** provides exactly that — for both figures and tables. Floats integrate with LaTeX's page builder — text reflows around them, `\caption`, `\label`, `\ref`, `\listoffigures`, and `\listoftables` all work normally, and ordinary floats continue to function alongside absolute ones.

## Quick example

```latex
\documentclass[twocolumn]{article}
\usepackage{graphicx}
\usepackage{absfigure}

\begin{document}

% Place a figure at the top of page 3, left column
\begin{absfigure}[page=3, pos=t, col=1]
  \centering
  \includegraphics[width=\columnwidth]{fig.pdf}
  \caption{Exactly where I want it}
  \label{fig:example}
\end{absfigure}

% Spanning figure (like figure*) at the bottom of page 4
\begin{absfigure*}[page=4, pos=b]
  \centering
  \includegraphics[width=\textwidth]{wide.pdf}
  \caption{A wide figure, exactly on page 4}
\end{absfigure*}

% Or just place on the current page (the default)
\begin{absfigure}[pos=b, col=r]
  \centering
  \includegraphics[width=\columnwidth]{here.pdf}
  \caption{Bottom of whatever page this code lands on}
\end{absfigure}

% Tables work the same way
\begin{abstable}[page=2, pos=t, col=1]
  \centering
  \caption{Results}
  \label{tab:results}
  \begin{tabular}{lcc}
    \hline
    Method & Accuracy & F1 \\
    \hline
    Ours   & 94.2     & 0.91 \\
    \hline
  \end{tabular}
\end{abstable}

Your document text here...

\end{document}
```

## Keys

| Key | Values | Default | Description |
|-----|--------|---------|-------------|
| `page` | integer, `current`, `end` | `current` | Target page number |
| `pos` | `t`/`top`, `b`/`bottom` | `t` | Top or bottom of the page |
| `col` | `1`/`l`/`left`, `2`/`r`/`right` | `1` | Which column (ignored for starred variants) |

All four environments — `absfigure`, `absfigure*`, `abstable`, `abstable*` — accept the same keys.

- **`page=current`** (or omit `page`): places the float on the page being built where the code appears.
- **`page=end`**: appends an extra page after the document with the float.
- **`page=`*N***: places on page *N*. If the float is defined after page *N* in the source, rerun LaTeX so the placement cache can be replayed on the next pass.

## Placement notes

- Multiple absolute floats targeting the same `page`/`col`/`pos` slot are stacked in source order.
- If a float targets an earlier page than where it appears in the source, run LaTeX again after editing. The package emits a warning when placement is not yet final.

## Requirements

- LaTeX2e
- `keyval` (part of the standard `graphics` bundle)
- `environ` (available on CTAN)

## Installation

For normal use, copy `absfigure.sty` to your project directory or to your local `texmf` tree.

For package maintenance / CTAN packaging:

- Canonical package sources live in `source/latex/absfigure/`.
- Run `latex source/latex/absfigure/absfigure.ins` from the package root to regenerate `absfigure.sty`.
- Run `latexmk -pdf -interaction=nonstopmode -output-directory=doc/latex/absfigure doc/latex/absfigure/absfigure.tex` to build the manual.
- Run `latexmk -pdf -interaction=nonstopmode -output-directory=doc/latex/absfigure doc/latex/absfigure/absfigure-test.tex` to build the example document.

## Documentation

Full documentation is in [`doc/latex/absfigure/absfigure.pdf`](doc/latex/absfigure/absfigure.pdf), generated from [`source/latex/absfigure/absfigure.dtx`](source/latex/absfigure/absfigure.dtx).

## License

MIT License. See [LICENSE](LICENSE) for details.
