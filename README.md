# navigation-pane

![docface](https://github.com/bacadra/atom-navigation-pane/blob/master/docface.png?raw=true)

## Sections panel

Package offer panel to show navigation tree. The items of tree are created manually by inserting special marks into text editor. Multiple scopes are supported (see below) with their own marker system.


## Highlight section

For each header, the package can create a marker to highlight the text in the editor. The marker style can be customized. Markers can be turned off or on with the command `navigation-pane:toggle-markers` or by right-clicking on the panel and using the `Toggle markers`.


## Section folding

There are functions which provide fold actions (fold, unfold or toggle) of sections. The special future is to collapse all section to create a view like table of content.


## Customize the appearance

The panel can be adapted to the user's needs in many ways. Several options are outlined below. The colors can be adjusted to user preference and ui/syntax theme in `styles.less` (File/Stylesheet..).

* e.g. all markers has highlighted background, but level 1, 2 and 3 has their own color
  ```
  .navigation-marker {
    background: rgba(233, 228, 141, 0.3);
  }
  .navigation-marker-1 {
    background: rgba(197, 218, 131, 0.3);
  }
  .navigation-marker-2 {
    background: rgba(250, 192, 209, 0.3);
  }
  .navigation-marker-3 {
    background: rgba(200, 197, 243, 0.3);
  }
  ```

* e.g. add top border to markers with level 1
  ```
  .navigation-marker-1 {
    border-top: 0.016px solid @text-color-info;
  }
  ```

* e.g.: change color of active item text
  ```
  .navigation-pane .list-nested-item.current > span.tree-item-text {
    color: #304ee2;
  }
  ```

* e.g. change color of hover item text
  ```
  .navigation-pane .list-nested-item > span.tree-item-text:hover {
    color: #304ee2;
  }
  ```

* e.g. item text wrapping
  ```
  .navigation-pane {
    white-space: normal;
  }
  ```

* change indent of list items
  ```
  .navigation-pane .list-nested-item {
    @navpane-indent: 50px;
    padding-left: @navpane-indent;
    .icon {
      margin-left: -@navpane-indent+2px;
    }
  }
  ```

* change font to monospace (or any other...)
  ```
  .navigation-pane {
    font-family: monospace;
  }
  ```


# Supported scopes

## REGEX testing

You can test and analyze regex's below at [regex101](https://regex101.com/). Just select flavor as `ECMAScript (JavaScript)` and paste statement.


## ASCII

Global regular expression is `/^(=={0,5}|#\#{0,5})[ \t]+(.+?)(?:[ \t]+\1)?$/`.


## LaTeX

Global regular expression is `/([^%\n]*)%(\$+)%(.*)|^[^\%\n]*\\((?:part|chapter|section|subsection|subsubsection|paragraph|subparagraph))\*?(?:\[(.*)\])?{(.*)}`. The `\part{...}` is equal level 4, `\chapter{...}` is level 5 etc.

* e.g. `%$% Countries` -> `1. Countries`
* e.g. `%$$% United Kingdom` -> `1.1. United Kingdom`
* e.g. `\part{Resources}` -> `1.1.1.1. Resources`
* e.g. `\part[Resources]{Resources but to long to TOC}` -> `1.1.1.1. Resources`


## BibTeX

Global regular expression is `([^%\n]*)%(\$+)%(.*)|^[ ]*\@(\w*)[ ]*{[ ]*([^\,]*)`. The `@<type>{<text>,` is level 6.

* e.g. `%$% Bibliography about countries` -> `1. Bibliography about countries`
* e.g. `%$$% United Kingdom` -> `1.1. United Kingdom`
* e.g. `@book{jk2021, ...` -> `1.1.1.1.1.1. jk2021`


## Markdown

Global regular expression is `^ *(\#+) (.*)`. The level is defined as count of `#`. The number of levels is endless.

* e.g. `# Countries` -> `1. Countries`
* e.g. `## United Kingdom` -> `1.1. United Kingdom`


## Python

Global regular expression is `/^([^#\n]*)#(?:%%)?(\$+[spv1]?|a)([\!\*-]?)#(.*)/` where count of `$` mean the level on list.

Additional letter can be used to provide additional parse effect:

* `s`: get only text from first string which occur in this line,
* `p`: python def or class; show only type and name of object
* `v`: variable; show only name of variable
* `1`: use only first word (split by whitespace), without optional after-colon

Additional letter can be used to provide additional visual effect:

* `*`: technical section
* `!`: important section
* `-`: add top border in tree

As special case you can use `#a#` which mean auto level base on pattern `<any>(<lvl as int>, "<text>"<any>)`. It is useful e.g. in PyLaTex or similar.

* e.g. `#$# Countries` -> `1. Countries`
* e.g. `#$$# United Kingdom` -> `1.1. United Kingdom`
* e.g. `a = 5 #$$v#` -> `1.1. a`
* e.g. `class MyCounty(Country): #$$p#` -> `1.1. MyCounty`
* e.g. `document.section(1, 'Countries') #a!#` -> `1. Countries`
* e.g. `document.section(2, 'United Kingdom') #a#` -> `1.1. United Kingdom`


## ReStructuredText

Global regular expression is `/^(.+)\n([!-/:-@[-[-~])\2+$/`.


## SOFiSTiK

Global regular expression is `^ *(#define [^\n=]+$|#enddef)|(^(?! *\$)[^!\n]*)!(\$+)!(.*)|^!.!chapter +([^=\n]*)|^ *.?prog +([^\n]*)(?:\n *head +(.+))?|^ *!.! +(.*)`. The chapter is level 4, prog is level 5 and label is level 6.

* e.g. `!$! Design slab` -> `1. Design slab`
* e.g. `!$$! Req. reinforcement` -> `1.1. Req. reinforcement`
* e.g. `!+!Chapter Design` -> `1.1.1.1. Design`
* e.g. `+prog aqua` -> `1.1.1.1.1. aqua`
* e.g. `+prog aqua \n head sections` -> `1.1.1.1.1.1. aqua: head sections`


# Contributions

If you have idea how to improve the package, see bugs or want to support new scope, then feel free to share it via GitHub.
