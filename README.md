### Simditor

---

Simditor is a browser-based WYSIWYG text editor.

It is used by [Tower](http://tower.im) -- a popular project management web application.

Supported Browsers: IE10+、Chrome、Firefox、Safari.
* [Download Zip](https://github.com/mycolorway/simditor/releases)
* Install with npm: $ npm install simditor</li>
* Install with bower: $ bower install simditor</li>

Demo and docs can be found [here](http://simditor.tower.im/).

====分割线=========================================


#### 样式更新说明(2018-9-2)

* 工具栏样式
* 引用样式
* 文档编辑区宽度（800px）
* placeholder暂时隐藏了，absolute不支持margin属性，后续调整js实现。
* *增加文档宽度标识* 
* *字数统计*

![效果图](https://github.com/shungdawei/simditor/blob/master/captures/Snipaste_2018-09-02_17-57-04.png)

#### 代码使用

由于不熟悉Simditor的JS代码，没集成，使用稍微麻烦点。

首先需要引用本项目中的样式[simditor.css](https://github.com/shungdawei/simditor/blob/master/styles/simditor.css)配合使用，文档宽度定义为 800px。如果需要调整的可以修改[editor.scss](https://github.com/shungdawei/simditor/blob/master/styles/editor.scss)中 `$simditor-body-width`值或直接修改simditor.css，搜索800px，840px替换。

然后在初始化的方法里加入相关代码。下面代码是在官网示例的基础上做的调整，供参考。

```
(function() {
  $(function() {
    var $preview, editor, mobileToolbar, toolbar;
    Simditor.locale = 'zh-CN';
    toolbar = ['title', 'bold', 'italic', 'underline', 'strikethrough', 'fontScale', 'color', '|', 'ol', 'ul', 'blockquote', 'code', 'table', '|', 'link', 'image', 'hr', '|', 'indent', 'outdent', 'alignment'];
    mobileToolbar = ["bold", "underline", "strikethrough", "color", "ul", "ol"];
    if (mobilecheck()) {
      toolbar = mobileToolbar;
    }
    editor = new Simditor({
      textarea: $('#txt-content'),
      //placeholder: '这里输入文字...',
      toolbar: toolbar,
      pasteImage: true,
      defaultImage: 'assets/images/image.png',
      upload: location.search === '?upload' ? {
        url: '/upload'
      } : false
    });

    // 1、文档宽度标识
    var docPlaceholderHTML = $("<div class=\"doc-placeholder\"></div>");
    docPlaceholderHTML.append("<span class=\"left\"></span>");
    docPlaceholderHTML.append("<span class=\"right\"></span>");
    $(".simditor-body").before(docPlaceholderHTML);

    $preview = $('#preview');
    if ($preview.length > 0) {
      return editor.on('valuechanged', function(e) {
        return $preview.html(editor.getValue());
      });
    }

    // 2、监听valuechanged事件，通过正则获取长度
    editor.on("valuechanged",function(e,src){
      var value = editor.getValue();
      var text = value.replace(/<\/?[^>]*>/g,'').replace(/[ | ]*\n/g,'\n').replace(/\n[\s| | ]*\r/g,'\n').replace(/&nbsp;/ig,'');
      $(".words-count .num").text(text.length);
  	});
  });

}).call(this);

```