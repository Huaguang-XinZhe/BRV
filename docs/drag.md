<img src="https://i.loli.net/2021/08/14/9EUvCSnONYixWDT.gif" width="250"/>

为数据模型实现接口`ItemDrag`即可开启拖拽功能

```kotlin
data class DragModel(override var itemOrientationDrag: Int = ItemOrientation.ALL) : ItemDrag
```

> 注意如果你的数据模型被Gson反序列化后, 会删除所有的字段初始化值

这里我们可以重写访问函数来解决问题, 让该值固定返回

```kotlin hl_lines="3"
class DragModel() : ItemDrag {
    override var itemOrientationDrag: Int = 0
        get() = ItemOrientation.ALL // 只会返回该值
}
```

## ItemOrientation

该类包含拖拽可配置的方向

|  字段  |    描述  |
| ---- | ---- |
|   `ALL`   |   全部方向   |
|   `VERTICAL`   |   垂直方向   |
|   `HORIZONTAL`   |   水平方向   |
|   `LEFT`   |   向左   |
|   `RIGHT`   |   向右   |
|   `UP`   |   向上   |
|   `DOWN`   |   向下   |
|   `NONE`   |   禁用   |

## 自定义

如果想要扩展ItemTouchHelper或监听可以给BindingAdapter的变量`itemTouchHelper`赋值

```kotlin
rv.linear().setup {
  addType<Model>(R.layout.item)

  itemTouchHelper = ItemTouchHelper(object : DefaultItemTouchCallback(this) {

    /**
     * 当拖拽动作完成且松开手指时触发
     */
    override fun onDrag(
        source: BindingAdapter.BindingViewHolder,
        target: BindingAdapter.BindingViewHolder
    ) {
        // 这是拖拽交换后回调, 这里可以同步服务器
    }

  })

}.models = data
```

> `DefaultItemTouchCallback`是BRV内部的触摸事件处理, 你可以覆写他或者直接`ItemTouchHelper.Callback`
