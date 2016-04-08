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
