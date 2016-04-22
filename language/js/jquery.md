## jQueryでプラグイン
```js
(function($){
    $.fn.plugin = function(options){
        var defaults = {
            key : "value"
        };
        var config = $.extend(defaults, options);
        return this.each(function(){
            $(this);
            config.key
        });
    };
})(jQuery);
```

## ロードタイミング
### DOMツリー読み込み後
```js
$(function(){
    ...
})
```
  
### 画像、CSS取得後
```js
$(window).on("load", function(){
    ...
})
```
  
### 画面リサイズ
```js
$(window).on("resize", function() {
    ...
})
```
