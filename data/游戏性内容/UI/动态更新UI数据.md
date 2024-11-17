# 动态更新UI数据
***
## 1.使用
原理有一些复杂，毕竟涉及底层代码，所以我不会过多解释。
<br><br>
类路径:
```text
arc.scene.ui.layout.Cell
```
在其中的
```text
public Cell<T> update(Cons<T> updater){
    T t = get();
    t.update(() -> updater.get(t));
    return this;
}
```
需要注意的是，这只是Cell的方法，不是按钮的。

在这其中可以看到
```text
public Cell<Table> table(Cons<Table> cons){
    Table table = new Table();
    cons.get(table);
    return add(table);
}
```
```text
public Cell<Label> add(CharSequence text){
    return add(new Label(text));
}
```
```text
public Cell<Button> button(Cons<Button> cons, Runnable listener){
    Button button = new Button();
    button.clearChildren();
    button.clicked(listener);
    cons.get(button);
    return add(button);
}
```
大部分代码中都含有```Cell<T>```。
<br>
也就是说这样的方法基本都可以使用```update()```。

使用:
```text
dialog.cont.table(t -> {
//页面代码
}).update(scrollPane -> {
//动态刷新
//这里的代码会重复运行，可以自行更改
});
```
这里不止dialog.cont.table可以，也可以是Button或者其他含有Cell<T>的代码。

这样的话就完成了，可以实现闪烁字体或者持续更新的文字显示。
