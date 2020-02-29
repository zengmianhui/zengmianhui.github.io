
---

title: android小经验

urlname: uninya

date: 2020-01-12 18:28:30 +0800

tags: []

---
<a name="OgWCa"></a>
# 

<a name="AgUOM"></a>
# android 小经验
<a name="Ki9vI"></a>
## 参考链接
<br />[StaticLayout——AndroidDeveloper@google](https://developer.android.google.cn/reference/kotlin/android/text/StaticLayout?hl=en)
<a name="Iremp"></a>
## getMeasuredHeight和getMeasuredWidth为0
是由于getMeasuredHeight和getMeasuredWidth需要先经过测量，如果在未测量结束时调用则无法获取，可以通过在post方法内调用，post方法会在布局渲染结束后调用<br />

```java
view.post(new Runnable(){
	//content for your code
    int height=view.getMeasuredHeight();
});
```

<a name="e4Txb"></a>
## 返回需要绘制的宽度StaticLayout.getDesiredWidth(CharSequence source,TextPaint paint)

> StaticLayout is a Layout for text that will not be edited after it is laid out. Use `[DynamicLayout](https://developer.android.google.cn/reference/kotlin/android/text/DynamicLayout.html)` for text that may change.
> StaticLayout是一个针对文本的布局，它在布局后不能被编辑。使用针对文本的DynamicLayout可以改变。
> This is used by widgets to control text layout. You should not 
need to use this class directly unless you are implementing your own 
widget or custom display object, or would be tempted to call `[ Canvas.drawText()](https://developer.android.google.cn/reference/kotlin/android/graphics/Canvas.html#drawText(kotlin.CharSequence,%20kotlin.Int,%20kotlin.Int,%20kotlin.Float,%20kotlin.Float,%20android.graphics.Paint))` directly.
> 这是被控件使用去控制文本布局的。你应该不需要直接去使用它的，除非你想实现你自己的控件或者自定义显示对象，又或者试图直接调用Canvas.drawText()。

上面是摘取自官方文档，StaticLayout针对文本的布局，需要有很多针对文本封装好的方法getDesiredWidth()就是其中一个，getDesiredWidth()可以传入一段字符，计算字符绘制所需的宽度。

> _Return how wide a layout must be in order to display the specified text with one line per  paragraph._
> _返回一个布局的宽度用于显示指定每个段落中的一行文本<br />_

以上摘取自Android API文档<br />

