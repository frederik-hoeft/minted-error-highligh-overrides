# minted-error-highlight-overrides

Some (ugly) workarounds to prevent the minted LaTeX package from highlighting syntax errors.

## Styles: `default`

Workaround: simple override of the command used to draw the error box.

```tex
\AtBeginEnvironment{minted}{%
  \renewcommand{\fcolorbox}[4][]{#4}}
```

## Styles: `colorful`, `murphy`

Workaround: conditionally override the commands used to render the respective error colors. 

```tex
\usepackage{minted}
\usepackage{etoolbox}
\usepackage{xcolor}

\AtBeginEnvironment{minted}{%
  % Save the original definitions
  \let\originalcolorbox\colorbox
  \let\originaltextcolor\textcolor
  % Redefine \colorbox to modify specific color boxes
  \renewcommand{\colorbox}[3][]{%
    % Check if the color is the specific error color
    \IfStrEq{#2}{1.00,0.67,0.67}% Replace with the RGB code of your error highlight color
      {#3}% If it's the error color, remove the box (keep only the content)
      {\originalcolorbox[#1]{#2}{#3}}% Otherwise, use the original \colorbox
  }
  % Redefine \textcolor to modify specific color usage
  \renewcommand{\textcolor}[3][]{%
    % Check if the color is the specific error color
    \IfStrEq{#2}{1.00,0.00,0.00}% Replace with the RGB code of your error text color
      {#3}% If it's the error color, output the text without coloring
      {\originaltextcolor[#1]{#2}{#3}}% Otherwise, use the original \textcolor
  }
}
```

You'll have to check `.pygstyle` file for the RGB codes matching your style (can be found under "other logs and files" in the warning/error window in Overleaf):

```text
_minted-output/STYLE-NAME.pygstyle
```
