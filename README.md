# Todo List with Vue.js and Vuex
利用 Vue.js 與 Vuex 實作一個 Todo List。

## 功能介紹
實作的功能有

- 新增一個 todo
- 修改一個 todo的內容
- 變更一個 todo 的狀態：已完成或未完成
- 刪除一個 todo
- 切換檢視模式：看全部、已完成或未完成的 todo
- 在檢視按鈕上顯示目前全部、已完成和未完成的 todo 數量

## Demo
(補圖)

## 程式碼說明
### Vue 與 Vuex 的合作流程
使用者與 view 互動 (.vue) -> 產生 action (actions.js) -> 藉由 action 發送 commit 改變 state (mutations.js) -> 更新view (.vue)。

### Vuex 的工作劃分
- components：單純 view 的展示，使用者在此與 view 互動。
- getter.js：取得 view 的資料。
- actions.js：要執行的動作，發送 commit和傳遞資料以做後續處理。
- mutations.js：更新 state 的地方。
- mutations_type.js：將動作定義成常數的地方。

### Scoped CSS
當`<style>`標籤有 scoped 屬性的時候，樣式就只能作用於當前的元件。在同一個元件內，可以同時有作用域和無作用域的樣式。

## 遇到的問題 / 解法
### 在`<input>`上綁定`v-model`，導致在 mutations 以外的地方改變 state
解法：使用 event handler 或 watch。例如，在更新 todo 的文字時，過去都是利用`v-model`雙向綁定的特性改變 model 的值，但在此使用 vuex 統一管理狀態，因此 todo 文字的改變就必須使用 action 發送 commit，交由 mutation 來處理。

發送 dispatch (components)

```javascript
updateTodo: function(index, content) {
  this.$store.dispatch('updateTodo', {
    index: index,
    content: content
  })
}
```

發送 commit (actions.js)

```javascript
export const updateTodo = ({ commit }, payload) => {
  commit(types.UPDATETODO, payload);
}
```

mutation：更新 state (mutations.js)

```javascript
[types.UPDATETODO](state, payload) {
  state.todos[payload.index].text = payload.content;
  state.todos[payload.index].isEdit = false;
}
```
// $event.target.value無法傳送到action: store dispatch & payload -> commit, EX: updateTodo

### 引入外部 css 檔案
解法

- 使用import，例如：`@import "../assets/css/style.scss";`
- 在`<style>`加上source，例如：`<style src="../assets/css/style.scss"></style>`

## React + Redux 與 Vue + Vuex 的比較
- getter.js：類似 react 的container，取得資料
- action.js：類似 react 的container，發送 dispatch
- mutations.js：類似 redux 的 reducer，改變狀態
- mutations_type.js：類似 react 的 actionTypes，將動作定義成常數

## 參考資料 / 推薦閱讀
- [Vuex(英)](https://vuex.vuejs.org/en/)
- [Vuex(簡中)](https://vuex.vuejs.org/zh-cn/)
- [第一次用 Vue.js 就愛上 [改]](https://www.slideshare.net/kurotanshi/vuejs-62131923)
- [Vue js 大型專案架構](https://www.slideshare.net/hinablue/vue-js)
- [實作小範例入門 Vue & Vuex 2.0系列](http://ithelp.ithome.com.tw/users/20103326/ironman/1114)
- [Vue.js 30天](http://ithelp.ithome.com.tw/users/20103424/ironman/1049)
- [Bootstrap Vue](https://bootstrap-vue.github.io/docs)

---
