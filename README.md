# vue.preventMacBackScroll

```js
/**
 * preventMacBackScroll
 * 当滚动到右边顶点的时候禁止右滑
 * 当滚动到左边顶点的时候禁止左滑
 * 起因, mac 下 Chrome/Safari 内嵌了手势操作, 当横向滚动条滚动到最后的时候, 会触发浏览器的向前一页向后一页的操作.
 * 为了用户体验, 理应禁止此操作.
 * 目前已在 mac 上测试 [Chrome✅, Safari✅, Firfox✅]
 * @example
 * v-preventMacBackScroll 不传参数, 默认阻止当前绑定指令的组件默认滚动事件.
 * v-preventMacBackScroll="'.el-table__body-wrapper'" 传参数, 则阻止传入的class 类名的 dom 元素
 */
export const preventMacBackScroll = {
  /**
   * 组件插入插槽的时候触发
   *
   * @param {*} el
   * @param {*} direct 当前指令内容
   */
  inserted(el, direct) {
    el.wheelEventFunc = (e) => {
      let scrollDiv = e.currentTarget
      if (direct.value) {
        scrollDiv = e.currentTarget.querySelector(direct.value)
      }
      // 当滚动到右边顶点的时候禁止滑
      if (scrollDiv && scrollDiv.scrollLeft + e.deltaX > scrollDiv.scrollWidth - scrollDiv.offsetWidth) {
        e.preventDefault()
      }
      // 当滚动到左边顶点的时候禁止左滑
      if (scrollDiv && scrollDiv.scrollLeft === 0 && e.deltaX < 0) {
        e.preventDefault()
      }
    }
    el.addEventListener('wheel', el.wheelEventFunc)
  },
  /**
   * 组件销毁的时候触发
   *
   * @param {*} el
   */
  unbind(el) {
    el.removeEventListener('wheel', el.wheelEventFunc)
  }
}
```
