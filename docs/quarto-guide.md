# Quarto Guide

21/06/2024 15:03

Quarto documentation: [https://quarto.org/docs/guide/](https://quarto.org/docs/guide/)    
Pandoc documentation: [https://pandoc.org/MANUAL.html](https://pandoc.org/MANUAL.html)

## Changes

### in `section.ipynb` 
- [github gist](https://gist.github.com/calnfynn/360c5f5bdcff96001336c946f6b13b59)
- `text = str(r.text) #changed from r.content` to get UTF-8 encoded text -> can handle umlauts and other special characters
- added `markdownify` to get markdown in `get_text()` instead of html -> makes converting to PDF/LaTeX more reliable
 
### in `_quarto.yml`
- added metadata: subtitle, license, copyright, citation, funding
- added a LaTeX-lookalike CSS to `format: epub: css:` https://latex.vercel.app/ (as a local file)
- added favicon to `book:` (`favicon: path/to/favicon.ico`)
- added styling info to `format: pdf:`: include-in-header, pagestyle, geometry, font-size, template-partials, title-bg-size
- added .tex template-partials to `format: pdf: template-partials:`
	- `in-header.tex` for fonts, formatting & packages
	- `before-body.tex` for frontmatter (not yet done)
	- `_titlepage.tex` for a cover image + title + author names
	- code: [github gist](https://gist.github.com/calnfynn/79512286e7ccfc3f474950be9ecf1f93)
	- more information about partials: [quarto doc](https://quarto.org/docs/journals/templates.html#template-partials)
