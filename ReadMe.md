# Blended ZIO documentation

This [site](http://blended-scala.org) is built with [HUGO](https://gohugo.io/), version 0.75 and the [HUGO book template](https://themes.gohugo.io/hugo-book/) with slight modifications.

To build the site, check out the repository und make sure, the submodules are also updated:

```bash
$ git clone https://github.com/blended-zio/blended-zio-site.git
$ cd blended-zio
$ git submodule init
$ git submodule update
```

Then the site can be run locally with

```
hugo server --renderToDisk -w
```

Here, `--renderToDisk` is required to properly serve images generated from markup.

Documentation can include [mermaid diagrams](http://mermaid-js.github.io/mermaid/) via the `mermaid` shortcode.

For example:

```
{{< mermaid >}}
flowchart LR
  subgraph Shop X
    Bx((Shop X)) --> FX1((Fs X1)) --> Bx
    Bx --> FX2((Fs X2)) --> Bx
  end

  subgraph Shop Y
    By((Shop y)) --> FY1((Fs Y1)) --> By
    By --> FY2((Fs Y2)) --> By
  end

  subgraph Shop Z
    Bz((Shop z)) --> FZ1((Fs Z1)) --> Bz
    Bz --> FZ2((Fs Z2)) --> Bz
  end

  A((Datacenter)) --> Bx --> A
  A --> By --> A
  A --> Bz --> A
{{< /mermaid >}}
```

## Additions to the original book template

### codesection shortcode

To help with code documentation, the various modules are included in the site via submodules, so the code con be referenced by a specialized HUGO shortcode, [codesection](https://github.com/blended-zio/blended-zio-site/blob/main/themes/book/layouts/shortcodes/codesection.html).

For example:

```
{{< codesection file="modules/blended-zio-jmx/ziojmx/jvm/blended/zio/jmx/metrics/ServiceMetrics.scala" lang="scala" section="update" >}}
```

The `file` parameters is mandatory and references the source file to be included in the documentation.

The `lang` parameter configures the language for the syntax highlighter and defaults to `scala` if ommitted. See [here](https://github.com/alecthomas/chroma#supported-languages) for a list of supported lunguages for syntax highlighting.

The `section` parameter can be used to restrict which part of the source file shall be included. If used, the shortcode will start the inclusion at a line which contains `doctag<section>` where section is the name of the section. In the example above that would be `doctag<update>`.

The inclusion ends at a line matching `end:doctag<section>`, again section being the name of the section.

For example:

```
... Some code ...
// doctag<update>
... Code included in the docs
// end:doctag<update>
... More code
```

If the section parameter is omitted, the entire file is included in the docs.

Line numbers are displayed in all codeblocks for better reference. The numbers do match the actual line number in the source code file, so in most cases they won't start at `1` unless the entire file is included.

### Git Ribbons

A slight modification of the [CSS based GitRibbon]( https://github.com/simonwhitaker/github-fork-ribbon-css) is included and wired up in the body partial.

If the parameter `giturl` is set within a pages front matter, a Git Ribbon will be displayed and points to the given `giturl`. By default the title displayed on the ribbon will be __Fork me on Azure__, but can be changed with the page parameter `gitribbon` set in the pages front matter.

For example, the main documentation pages display a `Edit me on Azure` ribbon.

Example:
```
---
giturl : "https://dev.azure.com/blended-zio/_git/blended-zio"
gitribbon : "Edit me on Github"
---
... Page content omitted ...
```
