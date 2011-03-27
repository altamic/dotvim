"Use native Vim settings, rather than original Vi's ones (much better!).
"Note that this must be the first option, because it changes other
"options as a side effect.

set nocompatible

set autoread

set autoindent

set complete-=i     " Searching includes can be slow

"multi-platform terminal setup. Pretend 256 colors, don't set for less.
"Mostly of the times it's a matter of setting $TERM to 'xterm-256color'
"or similar
if has("gui_running")
  set t_Co=256  "tell the term has 256 colors
    if has("gui_gnome")
        set term=gnome-256color
    else
        set guitablabel=%M%t
        set lines=40
        set columns=115
    endif
    if has("gui_mac") || has("gui_macvim")
        set guifont=Monaco:h16.00
    endif
    if has("gui_win32") || has("gui_win32s")
        set guifont=Consolas:h16
        set enc=utf-8
    endif
else
  let g:CSApprox_loaded = 1 "dont load csapprox if we have no gui support
  "silences an annoying warning
  set term=xterm-256color
  set t_Co=256  "Set terminal to 256 colors
endif

set encoding=utf-8    "default encoding

set tabstop=2         "tabs and
set shiftwidth=2      "space preferences

set incsearch   "find the next match as we type the search
set hlsearch    "hilight searches by default

set linebreak     "wrap lines at convenient points
set history=2000  "store lots of :cmdline history

"Status bar
set statusline=%f                       "tail of the filename
set statusline+=%m                      "modified flag
set statusline+=%=%r                    "read only flag
set statusline+=%h                      "help file flag
set statusline+=%w                      "
set statusline+=%y                      "filetype

set statusline+=[%{&ff},%{&encoding}]   "file format, file encoding
set statusline+=%P                      "percent through file
set statusline+=[line%3l\ of\ %L,\ col%2v]  "line, column

set laststatus=2                        "always show status line

"fold settings
set foldmethod=syntax               "fold based either on syntax or indent
set foldtext=getline(v:foldstart)   "show start line of folder
set foldlevel=6                     "deepest fold level
set foldcolumn=0                    "fold column width, set 0 for disabling fold

"turn on syntax highlighting
syntax on

set listchars=tab:·\ ,trail:-,extends:>,precedes:<,nbsp:+  "show trailing whiteshace and tabs
set list                                                   "show unprintable chars by default

"mark syntax errors with :signs
let g:syntastic_enable_signs=1

"some stuff to get the mouse going in term
set mouse=a
set ttymouse=xterm2

"default colorscheme
colorscheme convergent

" Set up commands for FuzzyFinder and FuzzyFinderTextMate
map <D-u> :ruby finder.rescan!<CR>:FuzzyFinderRemoveCache<CR>:exe ":echo 'rescan complete'"<CR>

"make <c-l> clear the highlight as well as redraw
"nnoremap <C-L> :nohls<CR><C-L>
"inoremap <C-L> <C-O>:nohls<CR>

"map to bufexplorer
"nnoremap <C-B> :BufExplorer<cr>

"tree file finder
"nmap <silent> <Leader>p :NERDTreeToggle<CR>

"snipmate setup
source ~/.vim/snippets/support_functions.vim
autocmd vimenter * call s:SetupSnippets()
function! s:SetupSnippets()

    "if we're in a rails env then read in the rails snippets
    if filereadable("./config/environment.rb")
        call ExtractSnips("~/.vim/snippets/ruby-rails", "ruby")
        call ExtractSnips("~/.vim/snippets/eruby-rails", "eruby")
    endif

    call ExtractSnips("~/.vim/snippets/html", "eruby")
    call ExtractSnips("~/.vim/snippets/html", "xhtml")
    call ExtractSnips("~/.vim/snippets/html", "php")
endfunction

"make <c-l> clear the highlight as well as redraw
"nnoremap <C-L> :nohls<CR><C-L>
"inoremap <C-L> <C-O>:nohlFilebtabs switch

" Fuzzy Finder
nnoremap <tab>       :FuzzyFinderFile<CR>
nnoremap <C-x><tab>  :FuzzyFinderBuffer<CR>
map      <C-s><tab>  :NERDTreeToggle<CR>

" Alternate file
map      <C-a><tab>  <C-^>

"map <tab> :FuzzyFinderBuffer<CR>
"map <D-i> :FuzzyFinderTextMate<CR>
"map <leader>b :FuzzyFinderBuffer<CR>
"map <leader>f :FuzzyFinderFile<CR>
"map <D-i> :FuzzyFinderMruFile<CR>

if has("autocmd")
  " Enable file type detection.
  " Use the default filetype settings, so that mail gets 'tw' set to 72,
  " 'cindent' is on in C files, etc.
  " Also load indent files, to automatically do language-dependent indenting.
  filetype on                     "Turns on file type detection
  filetype indent on              "Enable loading the plugin files for specific file types
  filetype plugin on              "Allow plugins to do fancy things with file-type detection
  set fileformats=unix,dos,mac    "This gives the end-of-line (<EOL>) formats that will be used/tried

  " augroup mkd
    " autocmd BufRead *.mkd  set ai formatoptions=tcroqn2 comments=n:>
    " autocmd BufRead *.markdown set ai formatoptions=tcroqn2 comments=n:>
  " augroup END

  " Put these in an autocmd group, so that we can delete them easily.
  augroup vimrcEx
  au!

  " For all text files set 'textwidth' to 78 characters.
  autocmd FileType text setlocal textwidth=78
  autocmd BufNewFile,BufRead *.haml             set ft=haml
  autocmd BufNewFile,BufRead *.feature,*.story  set ft=cucumber
  autocmd BufRead * if ! did_filetype() && getline(1)." ".getline(2).
        \ " ".getline(3) =~? '<\%(!DOCTYPE \)\=html\>' | setf html | endif

  autocmd FileType javascript             setlocal et sw=2 sts=2 isk+=$
  autocmd FileType html,xhtml,css         setlocal et sw=2 sts=2
  autocmd FileType eruby,yaml,ruby        setlocal et sw=2 sts=2
  autocmd FileType cucumber               setlocal et sw=2 sts=2
  autocmd FileType gitcommit              setlocal spell
  autocmd FileType ruby                   setlocal comments=:#\  tw=79
  autocmd FileType vim                    setlocal et sw=2 sts=2 keywordprg=:help

  autocmd Syntax   css  syn sync minlines=50

  " When editing a file, always jump to the last known cursor position.
  " Don't do it when the position is invalid or when inside an event handler
  " (happens when dropping a file on gvim).
  " Also don't do it when the mark is in the first line, that is the default
  " position when opening a file.
  autocmd BufReadPost *
    \ if line("'\"") > 1 && line("'\"") <= line("$") |
    \   exe "normal! g`\"" |
    \ endif

  augroup END
endif

"jump to last cursor position when opening a file
"dont do it when writing a commit log entry
autocmd BufReadPost * call SetCursorPosition()
function! SetCursorPosition()
    if &filetype !~ 'commit\c'
        if line("'\"") > 0 && line("'\"") <= line("$")
            exe "normal! g`\""
            normal! zz
        endif
    end
endfunction

" use fakeclip
set clipboard=unnamed

" case only matters with mixed case expressions
set ignorecase
set smartcase

" keep cursor away from top/bottom
set scrolloff=6

" keep cursor away from sides
set sidescrolloff=5

" quick tabs movements
" map <M-1> :tabn1<CR>
" map <M-2> :tabn2<CR>
" map <M-3> :tabn3<CR>
" map <M-4> :tabn4<CR>

" learning the hard way
" http://cloudhead.io/2010/04/24/staying-the-hell-out-of-insert-mode/
" inoremap <Left>  <NOP>
" inoremap <Right> <NOP>
" inoremap <Up>    <NOP>
" inoremap <Down>  <NOP>

" broswe popmenu with C-n and C-p
inoremap <Up> <C-R>=pumvisible() ? "\<lt>C-p>" : "\<lt>Up>"<CR>
inoremap <Down> <C-R>=pumvisible() ? "\<lt>C-n>" : "\<lt>Down>"<CR>

" Show highlighting groups for current word
nmap <C-S-O> :call <SID>SynStack()<CR>
function! <SID>SynStack()
  if !exists("*synstack")
    return
  endif
  echo map(synstack(line('.'),col('.')), 'synIDattr(v:val, "name")')
endfunction

"
let g:NERDCreateDefaultMappings = 0
let g:NERDSpaceDelims = 1
let g:NERDShutUp = 1
nmap \\           <Plug>NERDCommenterInvert
xmap \\           <Plug>NERDCommenterInvert

" strip spaces at the end of lines
command! -bar -range=% Trim :<line1>,<line2>s/\s\+$//e

runtime! plugin/matchit.vim
runtime! macros/matchit.vim

map Y       y$



" call pathogen to open bundles
"call pathogen#runtime_append_all_bundles()

