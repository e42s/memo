# perl6の開発環境
mintty + screen + vim  
  
## minttyの設定
### 256色表示
`Terminal > Type > xterm-256color`
### powerline用フォントの準備
https://github.com/powerline/fontpatcher  
で、クライアントで使っているフォントにパッチをあてて、クライアントにインストール、minttyで指定.  
  
## screenのコンパイル
`--enable-colors256`オプションを付けてコンパイルしなおす.  
  
## vimプラグイン
### vim-perl
perl6のシンタックス  
https://github.com/vim-perl/vim-perl  
### powerline
vimのステータス表示  
https://github.com/powerline/powerline  

## .screenrc
```sh
# mouse track
mousetrack on

# terminal
termcapinfo xterm* 'Co#256:AB=\E[48;5;%dm:AF=\E[38;5;%dm'
altscreen on

# status line
utf8 on
caption always '%?%F%{= rk}%:%{= wk}%?%n %f %t%='
hardstatus alwayslastline '%{= kw}%02c:%s %{= .y}%H %L=%{= .b}%-w%46>%{= .r}%n %t*%{= .b}%+w%-16=%{= .y}[%l]'
```

### レイアウト用
```sh 
source ~/.screenrc

# create window
screen 0
screen 1
screen 2

# create layout
layout new ide

# setup layout ide
layout select 0
split
select 0
focus up
resize -v -l 70%
split -v
focus next
select 1
focus bottom
select 2
focus up
focus next
resize -l 70%

# select layout ide
layout select ide
layout show
```

## .vimrc
```vim
" terminal(キーマップ)
set term=xterm-256color
" 256色
if $TERM == 'screen'
        set t_Co=256
endif

" Perl6
call plug#begin('~/.vim/plugged')
Plug 'vim-perl/vim-perl', { 'for': 'perl', 'do': 'make clean carp dancer highlight-all-pragmas moose test-more try-tiny' }
call plug#end()

" Statusline
python from powerline.vim import setup as powerline_setup
python powerline_setup()
python del powerline_setup
set laststatus=2
set showtabline=2
set noshowmode
```
  
一つ一つググれば情報ある
