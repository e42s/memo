# コンストラクタ
```perl
sub new {
  my $pkg = shift;
  my $self = {
    @_
  };
  return bless $self, $pkg;
}
```

# サブルーチン
## メソッド呼び出し
以下のサブルーチン
```perl
package Hoge;

sub hoge {
  my ($arg1, $arg2) = @_;
  ...
}
```
  
## 呼び出し
### ::で参照
```perl
Hoge::hoge("foo");
```
=> `$arg1`に `foo` , `$arg2` に `undef`  
  
  
### ->で参照
```perl
Hoge->hoge("foo");
```
=>`$arg1`は`Hoge`（パッケージ名）, `$arg2`に `foo`
