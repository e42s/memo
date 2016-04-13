# MSYS2で+luaのvimをインストール
パッケージ管理は`pacman`  
依存は
```sh
pacman -S パッケージ
```
で入れる.  
    
luaはソースから入れる必要がある.  
```sh
wget http://www.lua.org/ftp/lua-5.3.2.tar.gz
tar xvzf lua-5.3.2.tar.gz
cd lua-5.3.2
make mingws
make install
```

vimのソース取得
```sh
wget ftp://ftp.vim.org/pub/vim/unix/vim-7.4.tar.bz2
bzip2 -dc vim-7.4.tar.bz2 | tar xvf -
cd vim-7.4
```
以下のパッチを充てる  
```patch
--- a/src/if_lua.c
+++ b/src/if_lua.c
@@ -774,7 +774,7 @@ luaV_list_insert (lua_State *L)
 {
     luaV_List *lis = luaV_checkudata(L, 1, LUAVIM_LIST);
     list_T *l = (list_T *) luaV_checkcache(L, (void *) *lis);
-    long pos = luaL_optlong(L, 3, 0);
+    long pos = (long) luaL_optinteger(L, 3, 0);
     listitem_T *li = NULL;
     typval_T v;
     if (l->lv_lock)
```
コンパイル
```sh
./configure --enable-luainterp --enable-pythoninterp --with-lua-prefix=/usr/local
make
make install
```
