.bashrc  
```sh
PERL5LIB=~/.perl5lib

alias vi=vim
alias cpanm="cpanm --local-lib=~/.perl5lib"

stty stop undef
```
  
.screenrc
```screen
# ちかちか防止
vbell off

# mouse scroll
# termcapinfo xterm* ti@:te@

# ウィンドウサイズを変更しない
termcapinfo xterm* hs@:is=\E[r\E[m\E[2J\E[H\E[?7h\E[?1;4;6l

# mouse track
mousetrack on

# terminal 256色
termcapinfo xterm* 'Co#256:AB=\E[48;5;%dm:AF=\E[38;5;%dm'
altscreen on

# escape key
escape ^Xx

# 移動をCtrl + 矢印キーで
bindkey "^[[1;5C" focus right↲
bindkey "^[[1;5D" focus left↲
bindkey "^[[1;5A" focus up↲
bindkey "^[[1;5B" focus bottom↲
# 終了
bind \ quit

# status line
utf8 on
#shelltitle "$ |$SHELL"
caption always '%?%F%{= rk}%:%{= wk}%?%n %f %t%='
hardstatus alwayslastline '%{= kw}%02c:%s %{= .y}%H %L=%{= .b}%-w%46>%{= .r}%n %t*%{= .b}%+w%-16=%{= .y}[%l]'

```
  
.vimrc  
```vim
set number
set tabstop=4
set shiftwidth=4
set softtabstop=4
set encoding=utf-8
set fileencoding=utf-8

" 新しいウィンドウを開く場所
set splitbelow
set splitright

" terminal(キーマップ)
set term=xterm-256color
" 256色
if $TERM == 'screen'
        set t_Co=256
endif

" 空白、表示
set list
set listchars=tab:>-,trail:_,eol:↲,extends:>,precedes:<,nbsp:%

syntax on

" マウスホイール
set mouse=a
set ttymouse=xterm2

" vi との互換性OFF
set nocompatible
"カーソルを行頭，行末で止まらないようにする
set whichwrap=b,s,h,l,<,>,[,]
"BSで削除できるものを指定する
" indent  : 行頭の空白
" eol     : 改行
" start   : 挿入モード開始位置より手前の文字
set backspace=indent,eol,start

" ファイル形式の検出を無効にする
filetype off

" Vundle を初期化
set rtp+=~/.vim/bundle/vundle/
call vundle#rc()

" Vundle
Bundle 'gmarik/vundle'

" Perl
Bundle 'petdance/vim-perl'
Bundle 'hotchpotch/perldoc-vim'

" 入力補完
Bundle 'Shougo/neocomplete'
Bundle 'Shougo/neosnippet'
Bundle 'Shougo/neosnippet-snippets'
Bundle 'c9s/perlomni.vim'

" Syntastic
Bundle 'tpope/vim-pathogen'
Bundle 'scrooloose/syntastic'
call pathogen#infect()

" perl6 syntax
call plug#begin('~/.vim/plugged')
Plug 'vim-perl/vim-perl', {}
call plug#end()

" powerline
python from powerline.vim import setup as powerline_setup
python powerline_setup()
python del powerline_setup
set laststatus=2
set showtabline=2
set noshowmode

if has('vim_starting')
        set runtimepath+=~/.vim/bundle/neobundle.vim/
endif

" NeoBundleを初期化
call neobundle#begin(expand('~/.vim/bundle/'))
NeoBundle 'scrooloose/nerdtree.git'
call neobundle#end()

" ファイル形式検出、プラグイン、インデントを ON
filetype plugin indent on

"==========================================
" NERDTree ツリー表示
"==========================================
" 引数ない場合に表示
autocmd StdinReadPre * let s:std_in=1
autocmd VimEnter * if argc() == 0 && !exists("s:std_in") | NERDTree | endif
" クローズ時時にNERDTreeも終了
autocmd bufenter * if (winnr("$") == 1 && exists("b:NERDTree") && b:NERDTree.isTabTree()) | q | endif
let g:NERDTreeDirArrows = 1
let g:NERDTreeDirArrowExpandable = '＋'
let g:NERDTreeDirArrowCollapsible = '－'

"==========================================
" neocomplete.vim
"==========================================
"use neocomplete.
let g:neocomplete#enable_at_startup = 1
" Use smartcase.
let g:neocomplete#enable_smart_case = 1
" 自動で補完候補を出さない
let g:neocomplete#disable_auto_complete = 1
" Set minimum syntax keyword length.
let g:neocomplete#sources#syntax#min_keyword_length = 3
let g:neocomplete#lock_buffer_name_pattern = '¥*ku¥*'

" Define dictionary.
let g:neocomplete#sources#dictionary#dictionaries = {
                        \ 'perl' : $HOME.'/.vim/dict/perl.dict'
                        \}
" Define keyword.
if !exists('g:neocomplete#keyword_patterns')
        let g:neocomplete#keyword_patterns = {}
endif
let g:neocomplete#keyword_patterns['default'] = '¥h¥w*'

" Enable heavy omni completion.
if !exists('g:neocomplete#sources#omni#input_patterns')
        let g:neocomplete#sources#omni#input_patterns = {}
endif
" For perlomni.vim setting.
" https://github.com/c9s/perlomni.vim
let g:neocomplete#sources#omni#input_patterns.perl = '[^. \t]->\%(\h\w*\)\?\|\h\w*::\%(\h\w*\)\?'

"==========================================
" Snippet
"==========================================
let s:my_snippets = $HOME.'/.vim/snippets/'
let g:neosnippet#snippets_directory = s:my_snippets

"==========================================
" Syntastic
"==========================================
let g:syntastic_enable_perl_checker = 1
let g:syntastic_perl_checkers = ['perl', 'podchecker']

"==========================================
"パッケージ名の自動チェック
"==========================================
function! s:get_package_name()
        let mx = '^\s*package\s\+\([^ ;]\+\)'
        for line in getline(1, 5)
                if line =~ mx
                        return substitute(matchstr(line, mx), mx, '\1', '')
                endif
        endfor
        return ""
endfunction

function! s:check_package_name()
        let path = substitute(expand('%:p'), '\\', '/', 'g')
        let name = substitute(s:get_package_name(), '::', '/', 'g') . '.pm'
        if path[-len(name):] != name
                echohl WarningMsg
                echomsg "パッケージ名と保存されているパスが異なります。"
                echohl None
        endif
endfunction

au! BufWritePost *.pm call s:check_package_name()

"==========================================
" Key Bind
" =========================================
" Plugin key-mappings.
inoremap <expr><Nul> pumvisible() ? "\<down>" : neocomplete#start_manual_complete()
imap <expr><C-k>
                        \ neosnippet#expandable() <Bar><Bar> neosnippet#jumpable() ?
                        \ "\<Plug>(neosnippet_expand_or_jump)" : "\<C-n>"
" SuperTab like snippets behavior.
imap <expr><TAB>
                        \ neosnippet#expandable() <Bar><bar> neosnippet#jumpable() ?
                        \ "\<Plug>(neosnippet_expand_or_jump)" : pumvisible() ? "\<C-n>" : "\<TAB>"
smap <expr><TAB>
                        \ neosnippet#expandable() <Bar><bar> neosnippet#jumpable() ?
                        \ "\<Plug>(neosnippet_expand_or_jump)" : "\<TAB>"

" 補完をキャンセルしてカーソル移動
inoremap <expr><left> neocomplete#cancel_popup() . "\<left>"

map <C-c> :SyntasticCheck<CR>

" ide
noremap <C-c> yy
noremap <C-v> p
noremap <C-a> ggVG
noremap <C-i> =
noremap <C-z> u
noremap <C-y> <C-r>
noremap <C-d> dd
noremap <C-S-Right> >>
noremap <C-S-Left> <<
noremap <C-f> /

noremap <C-t> :tabnew<CR>:NERDTree<CR>
noremap <C-s> :w<CR>
noremap <C-o> :NERDTree<CR>
noremap <C-n> :NERDTreeToggle<CR>
```
