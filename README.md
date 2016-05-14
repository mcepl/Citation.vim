citation.vim
============

A citation source for unite.vim

(https://github.com/rafaqz/citation.vim)

Citation.vim Imports zotero databases (with better_bibtex citation keys if
available) or exported bibtex files and inserts/runs/does whatever you want with
them using Unite.vim.

It also allows you to use your documents as file managers - open referenced pdfs
and urls directly from your citations or from the citation list. Browse all
citation details, notes and abstracts within vim. Yank, insert or preview .

![Citation.vim screenshot](screenshot.png?raw=true "Citation.vim screenshot")

A recent addition is caching: zotero and bibtex file import is cached until the
files change - this has drastically sped up loading of large citation databases.

Many thanks to termoshtt for unite-bibtex and smathot for gnotero and LibZotero code.


### Sources

This plugin provides a *lot* of unite sources.

citation/key
- returns the citation key string like [@smith2004] to be used as a reference.
- customisable prefix and suffix to produce latex/pandoc etc. citation styles

citation/file
- Returns the file attached to a citation, great for opening pdfs from vim
  directly, using the start action.
- If there are multiple files it just returns the first one.

citation/combined
- Preview all available citation data on one page.

The full list:

citation/abstract
citation/author
citation/combined
citation/date
citation/doi
citation/file
citation/isbn
citation/publication
citation/key
citation/language
citation/issue
citation/notes
citation/pages
citation/publisher
citation/tags
citation/title
citation/type
citation/url
citation/volume

You can also enter `:Unite citation` in vim for the full list of sources.

### Usage

1. Install [unite.vim](https://github.com/Shougo/unite.vim)
2. Install this plugin in vim however you like to do that.
3. Choose your source

  If you're using bibtex install [pybtex](http://pypi.python.org/pypi/pybtex)

  ```bash
  sudo easy_install pybtex
  ```

  Set variables:

  ```vimscript
  let g:citation_vim_file_path=["/path/to/your/bib/file/library.bib"]
  let g:citation_vim_file_format="bibtex"
  ```

  To use [zotero](https://www.zotero.org/)
    Set variables:

  ```vimscript
  let g:citation_vim_file_path=["/path/to/your/zotero/7XX8XX72/zotero_folder/"]
  let g:citation_vim_file_format="zotero"
  ```

4. Set your cache path:

  ```vimscript
    let g:citation_vim_cache_path='~/.vim/your_cache_path'
  ```

5. Set your citation suffix and prefix. Pandoc markdown style is the default.

  ```vimscript
  let g:citation_vim_outer_prefix="["
  let g:citation_vim_inner_prefix="@"
  let g:citation_vim_suffix="]"
  ```

6. Set some mappings. Copy and paste the following examples into your vimrc to get started.


### Examples mappings:

Set a unite leader:
nmap <leader>u [unite]
nnoremap [unite] <nop>

To insert a citation:

```vimscript
nnoremap <silent>[unite]c       :<C-u>Unite -buffer-name=citation   -start-insert -default-action=append      bibtex<cr>
```

To immediately open a file or url from a citation under the cursor:

```vimscript
nnoremap <silent><leader>co :<C-u>Unite -input=<C-R><C-W> -default-action=start -force-immediately citation/file<cr>
```

To immediately browse the file folder from a citation under the cursor:

```vimscript
nnoremap <silent><leader>cf :<C-u>Unite -input=<C-R><C-W> -default-action=file -force-immediately citation/file<cr>
```

To view all citation information from a citation under the cursor:

```vimscript
nnoremap <silent><leader>ci :<C-u>Unite -input=<C-R><C-W> -default-action=preview -force-immediately citation/combined<cr>
```



To preview, append, yank any other citation data from unite:

```vimscript
nnoremap <silent>[unite]cp :<C-u>Unite -buffer-name=citation -default-action=append  -auto-preview citation/XXXXXX<cr>
```

`:Unite citation` for the list of sources...



### Tweaks 

Customise the unite display, using the names of citation sources and a python
format string (the {} braces will be replaced by the sources):

```vimscript
let g:citation_vim_description_format = "{}∶ {} \˝{}\˝ ₋{}₋ ₍{}₎"
let g:citation_vim_description_fields = ["type", "key", "title", "author", "year"]
```

or this one is nice for showing journal/publisher (citations rarely have both):

```vimscript
let g:citation_vim_description_format="{}→ ′{}′ ₊{}₊ │{}{}│"
let g:citation_vim_description_fields=["key", "title", "author", "publisher", "journal"]
```

You might have noticed the weird characters in the description format string.
They are used for highlighting sections, to avoid confusion with
normal characters that might be in the citation.

To change description highlighting characters, copy and paste characters from this list:
- Apostrophes and quotes  ˝‘’‛“”‟′″‴‵‶‷
- Brackets                ⊂〔₍⁽     ⊃〕₎⁾ 
- Arrows                  ◀◁<‹    ▶▷>› 
- Blobs                   ♯♡◆◇◊○◎●◐◑∗∙⊙⊚⌂★☺☻▪■□▢▣▤▥▦▧▨▩
- Tiny                    、。‸₊⁺∘♢☆☜☞♢☼
- Bars                    ‖│┃┆∥┇┊┋
- Dashes                  ‾⁻−₋‐⋯┄–—―∼┈─▭▬┉━┅₌⁼‗

- And use these like a colon after words (notice that's not a normal colon)
        ∶∷→⇒≫ 

Long lines will occasionally break the display colors. It's a quirk of how unite
shortens lines.

### Troubleshooting

You can correct your .bib file with `pybtex-convert`:

    pybtex-convert /path/to/your.bib out.bib
