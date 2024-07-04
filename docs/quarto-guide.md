04/07/2024 13:00

# Quarto Guide

Quarto documentation: [https://quarto.org/docs/guide/](https://quarto.org/docs/guide/)    
Pandoc documentation: [https://pandoc.org/MANUAL.html](https://pandoc.org/MANUAL.html)

# LaTeX Styling

You can style PDF documents in Quarto with custom TEX template files and/or entries in \_quarto.yml.  

These are the files you need in order to change the PDF format:
- \_quarto.yml
- in-header.tex
- before-body.tex
- \_titlepage.tex
- optional: index.qmd

The full \_quarto.yml and TEX files can be found [here](https://gist.github.com/calnfynn/109760958bda6f43942357e0923dddc0).

Parts of this were derived from [Quarto Titlepages V1](https://github.com/nmfs-opensci/quarto_titlepages_v1/tree/main); their documentation might be helpful. 

## \_quarto.yml

[entire file](https://gist.github.com/calnfynn/109760958bda6f43942357e0923dddc0#file-quarto-yml) 

### Authors

To add authors and their details, go to \_quarto.yml and add/change the following:

```
author:
	- name: Jane Doe  
		affiliations:  
			- name: Minnesota Department of Natural Resources  
			address: 500 Lafayette Road Saint Paul, MN 55155  
		email: jd@college.edu
	- name: Neptune III.
		affiliations:
			- name: University of Somewhere
			department: Department of Marine Biology  
		email: fishguy@somewhere.edu

``` 

You can repeat this for as many authors as you need.   

### Abstract

To have an abstract on your cover page, add this to \_quarto.yml:
```md:
abstract: "This is an abstract. :) You can write a few sentences here and they will appear on the cover page."
```

### PDF Layout/Styling 

You can style the PDF file in various ways. All the following options can be found under `format: pdf:`:  
```md:
format: 
	pdf: 
		pagestyle: plain  
		geometry:  
			- top=35mm  
			- right=25mm  
			- bottom=35mm  
			- left=25mm  
			- heightrounded  
		classoption: openany  
		fontsize: 11pt  
		toc-title: Inhaltsverzeichnis  
		urlcolor: cyan  
``` 
To change the **page margins**, use `geometry`.  
To change the style of the **page numbers**, use `pagestyle`. Valid options are: plain (page numbers only), empty, headings (running headings on each page).   
`classoption` determines whether the **chapters open** only on even pages or on even and odd pages.  
Standard `fontsize` values in LaTeX are 10pt, 11pt, and 12pt.  
Set a **custom heading for the table of contents** with `toc-title`.   
`urlcolor` changes only the **color of URLs** but not the color of links within the PDF (like in the table of contents). Check [here](https://steeven9.github.io/USI-LaTeX/html/packages_hyperref_babel_xcolor3.html) for available colors.basic 

### Cover Image

```md:
format:  
	pdf:  
		title-bg-image: "images/cover.jpg"  
		title-bg-location: "UL"  
		title-bg-size: 1  
```

The last three options `title-bg-image`, `title-bg-location` and `title-bg-size` change the cover picture.  

Upload your picture to the images folder in your Codespace. You can use JPG or PNG format. Ideally the resolution of your picture should be 2480 x 3508 pixels for A4 format (300dpi).  

If you do not want your image to cover your entire title page, you can use `title-bg-size` and set it to how much of the page width your image should use, like 0.5 or 0.75. 
You can then use `title-bg-size` to adjust the placement. The options are "UL" (upper left), "UR" (upper right), "LR" (lower right), and "LL" (lower left).  

### Template Partials 

In order to use custom TEX files for PDF styling they have to be linked in \_quarto.yml: 

```
template-partials:
	- "_titlepage.tex"
	- "before-body.tex"  
include-in-header: "in-header.tex"  
```

See the next section for details about TEX files. 

## Custom TEX Files

Quarto uses so-called "partials" to format PDF files. There are different partials for different parts of a document and they can define the content as well as the formatting. 

### in-header.tex

[entire file](https://gist.github.com/calnfynn/109760958bda6f43942357e0923dddc0#file-in-header-tex)

This file contains all packages and format settings that are used in the other TEX files.

```
\usepackage{hyphenat} % auto hyphenation
\usepackage{graphicx} % colors 
\usepackage{wallpaper} % for the background image on title page
\usepackage{geometry} % for margins
\usepackage{babel} % language support
\usepackage{libertine} % font
\usepackage[most]{tcolorbox} % boxes behind cover page text
\usepackage{setspace} % line spacing
\usepackage{parskip} % paragraph spacing

\onehalfspacing % line spacing
\setlength{\parskip}{1.5em} % paragraph spacing
\setlength\parindent{0pt} % paragraph indent
```

### before-body.tex

[entire file](https://gist.github.com/calnfynn/109760958bda6f43942357e0923dddc0#file-before-body-tex)

This file loads the title page (and optionally front matter) before the body of the text. 

```
$if(has-frontmatter)$  
	\begin{frontmatter}  
	\begin{titlepage}
	$_titlepage.tex()$
	\end{titlepage}
  	\end{frontmatter}
$else$
	\begin{titlepage}
	$_titlepage.tex()$
	\end{titlepage}
$endif$

{\let\newpage\relax} % don't add an empty page 
```

### \_titlepage.tex

[entire file](https://gist.github.com/calnfynn/109760958bda6f43942357e0923dddc0#file-_titlepage-tex)

This TEX file directs how the information on the cover page is shown and formatted. It takes the information you entered in \_quarto.yml (title, subtitle, author, background image etc) and brings it together as a title page. 

The linked version contains:
- title and subtitle 
- authors and their details (workplace/university, mail adresses)
- abstract
- background image
- tagline at the bottom 
- settings for the title/subtitle font and color and the background

This part determines the color and shape of the background boxes: 
```
\tcbset{ % parameters for text background 
	frame code={},
	left=0pt, % margins
	right=0pt,
	top=0pt,
	bottom=0pt,
	colback=gray!10!black, % box color
	width=\dimexpr\textwidth\relax, % box width
	boxsep=15pt, % spacing between boxes
	arc=0pt, % no rounded edges
	opacityback=0.35, % translucent-ish background
	fontupper=\color{white}, % fontcolor
	fontlower=\color{white}
    }
```

This part contains the formatting of title and subtitle: 
```
{\Huge\bfseries\nohyphens{$title$}}\\[1\baselineskip]
$if(subtitle)$
	{\huge{$subtitle$}}\\[4\baselineskip]
$endif$
```

And this part is for a tagline at the bottom of a page:
```
\begin{tcolorbox}
\centering
{
  TIB\\
  Open Science Lab
}
\end{tcolorbox}
```

## Optional: Chapter 1 (index.qmd) without Heading Number

To leave chapter 1 unnumbered in the table of content, add `{.unnumbered}` after **\# the first heading** in index.qmd.

# Other Changes

## in `section.ipynb` 
- [github gist](https://gist.github.com/calnfynn/360c5f5bdcff96001336c946f6b13b59)
- `text = str(r.text) #changed from r.content` to get UTF-8 encoded text -> can handle umlauts and other special characters
- added `markdownify` to get markdown in `get_text()` instead of html -> makes converting to PDF/LaTeX more reliable
 
## in `_quarto.yml`
- added favicon to `book:` (`favicon: path/to/favicon.ico`)
