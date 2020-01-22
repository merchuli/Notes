# Vue 學習筆記

>背景：使用 Angular 一年半以上，以此為基礎記錄轉 Vue 遇到的困惑點

## Overview

1. 常用指令

2. Guide

- Template Syntax (Essentials)
- Computed Properties and Watchers (Essentials)
- Slots (Components In Depth)

3. API

- Options & Data

4. 注意事項

5. 特性

- Reactivity
- 元件化



## 常用指令

### Create a project

```
vue create my-project
```

### Install eslint packages for Vue

```
vue add @vue/cli-plugin-eslint
```

Eslint reference: https://pjchender.blogspot.com/2019/07/vue-vue-style-guide-eslint-plugin-vue.html



## Guide

### Templae Syntax (Essentials)

＊此處僅列出與 Angular 使用方式較為不同之處，`<Anuglar 用法>` -> `<Vue 用法>`

`[key]` -> `:key`  (v-bind:key)

`(click)` -> `@click`  (v-on:click)



### Slots (Components In Depth) [1]

＊有點像是 Angular 中 ng-template / ng-container / ng-content 的用法

**Slot props allow us to turn slots into reusable templates that can render different content based on input props.** This is most useful when you are designing a reusable component that encapsulates data logic while allowing the consuming parent component to customize part of its layout.

- Shorthand: `#`



#### Slot Content

- can contain any template code, including HTML or other components
- If children component did not contain a slot element, any content provided between its opening and closing tag would be discarded.

```html
<!-- Parent Componet -->
<navigation-link url="/profile">
  Your Profile
</navigation-link>

<!-- Children Component: navigation-link -->
<a
  v-bind:href="url"
  class="nav-link"
>
	<slot></slot>
</a>

<!-- When the component renders, it turns out to be -->
<a
   v-bind:href="url"
   class="nav-link"
   >
  Your Profile
</a>
```



#### Named Slots & Scoped Slots

```html
<!-- Parent Compoent -->
<base-layout>
  <template v-slot:header>
    <h3>Header</h3>
    <h4>Here might be a page title for the Header</h4>
  </template>

  <h3>Default</h3>
  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template v-slot:footer="slotProps">
    <h3>Footer</h3>
    <p>Here's some user info</p>
    {{ slotProps.user.career }}
  </template>

  <b>Some other words, although put on the bottom of the parent component but this paragraph would show on the default block</b>
</base-layout>

<!-- Children Component: BaseLayout -->
<div class="container">
		<header>
			<slot name="header" />
		</header>
		<main>
			<p>Hello {{ user.name }},</p>
		</main>
		<footer>
			<!-- Won't working since the user is not binding to the slot
			<slot name="footer">
				{{ user.career }}
			</slot> -->
			<slot
				name="footer"
				:user="user"
			>
				<!-- Why not working ? -->
				<p>Hi {{ user.name }}, </p>
			</slot>
		</footer>
	</div>
```



#### Dynamic Slot Names

```html
<base-layout>
  <template v-slot:[dynamicSlotName]>
    ...
  </template>
</base-layout>
```



#### Named Slots Shorthand

```html
<current-user #default="{ user }">
  {{ user.firstName }}
</current-user>
```



## API

### Options & Data (data / props / propsData? / computed / methods / watch)

#### data

- 該 component 的變數

- Must be plain (含有零個或多個的 key / value )

- 不建議**觀察**擁有狀態行為的對象

```javascript
// Method 1: direct `instance creation` (new Vue)
var data = { count: 1 }

var vm = new Vue({
  data: data
})

vm.a // => 1
vm.$data === data // => true

// Method 2-1: Must use function when in Vue.extend()
var Component = Vue.extend({
  data: function () {
    return { count: 1 }
  },
})

// Method 2-2:  Or defining a new component
Vue.component('button-counter', {
  data() {
    return {
      count: 0
    }
  },
})
```

>  **Data Must Be a Function**
>
> Instead, **a component’s `data` option must be a function**, so that each instance can maintain an independent copy of the returned data object:
>
> If Vue didn’t have this rule, clicking on one button would affect the data of *all other instances*



#### props

父層傳下來的變數

> 一開始誤會是在此處定義這個 component 的變數

> 待確認之 Angualr 在區辨父元素傳進的 attribute 時好像沒有這麼友善，突然想到好像也有可以識別的

type 型別

default 預設值

required 是否為必須傳入的，如果沒有傳入會在非正式環境 console 出 warning

validator



#### methods

該 component 的 method



#### computed



### Instance Properties ( \$attrs / \$listener ) [3]

為了解決跨多級的傳遞關係，使用 vuex 太大材小用，使用 props + emit 跨多層級，中間層僅為中轉站但卻要多 props 程式碼會顯得太過凌亂，這時候就可以使用 \$attrs 跟 \$listener

main component

- first layer component
  - second layer component



Main Component

```vue
<template>
 <div id="app">
   <!-- 往下傳兩個 props，並監聽子元件的兩個事件 -->
   <first-layer-child
       :to-first-layer="To first layer"
       :to-second-layer="To second layer"
       @from-first-layer="onFromFirstLayer"
       @from-second-layer="onFromSecondLayer"> 
   </first-layer-child>
 </div>
</template>
<script>
 import FirstLayerChild from './FirstLayerChild.vue';
 export default {
 		 data() {
 		 		return {};
 		 },
 		 components: { FirstLayerChild },
		 methods: {
         onFromFirstLayer() {
             console.log('From First Layer');
         },
         onFromSecondLayer() {
             console.log('From Second Layer');
         }
 		 }
 };
</script>
```



First Layer Component

```vue
<template>
<div class="child-1">
		<p v-text="'First Layer:'"/>
  	<p v-text="`props: ${toFirstLayer}`" />
  	<p>$attrs: {{$attrs}}</p>
  	<hr>
 	 	<!-- Second Layer 元件中能直接觸發 onFromSecondLayer 的原因在於 First Layer 元件呼叫 Second Layer 元件時，使用 v-on 繫結了$listeners 屬性 -->
  	<!-- 通過 v-bind 繫結 $attrs屬性，Second Layer 元件可以直接獲取到 First Layer 元件中傳遞下來的props（First Layer 元件中 props 宣告的除外，因為 inheritAttrs 被設定為 false） -->
  <second-layer-child v-bind="$attrs" v-on="$listeners"></second-layer-child>
  </div>
</template>
<script>
  import SecondLayerChild from './SecondLayerChild.vue';
  export default {
    props: ['toFirstLayer'],
    data () {
      return {};
    },
    inheritAttrs: false,
    components: { SecondLayerChild },
    mounted () {
      this.$emit('fromFirstLayer');
    }
  };
</script>
```



Second Layer Component

```vue
<template>
   <div>
     <p v-text="'Second Layer: '" />
     <p v-text="`props: ${toSecondLayer}`" />
     <p>$attrs: {{$attrs}}</p>
     <hr>
   </div>
</template>
<script>
 export default {
   props: ['toSecondLayer'],
   data () {
   		return {};
   },
   inheritAttrs: false,
   mounted () {
   		this.$emit('fromSecondLayer');
   }
 };
</script>
```



**/////////////////////////// Draft Draft Draft Draft Draft Draft ///////////////////////////**

///////////////////////////

Event Modifier



///////////////////////////

Computed Properties and Watchers

#### Computed Setter

```javascript
// when you run vm.fullName = 'John Doe', the setter will be invoked and vm.firstName and vm.lastName will be updated accordingly.
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
```



#### watchers [2]

用來監聽 data 即時變化

This is most useful when you want to perform **asynchronous or expensive operations** in response to changing data.

```javascript

```



///////////////////////////

#### mixins

Mixins are a flexible way to distribute reusable functionalities for Vue components. A mixin object can contain any component options. When a component uses a mixin, all options in the mixin will be “mixed” into the component’s own options.



///////////////////////////

#### lifecycle hooks



///////////////////////////



## 注意事項

1. Properties that start with `_` or `$` will **not** be proxied on the Vue instance because they may conflict with Vue’s internal properties and API methods. You will have to access them as `vm.$data._property`.



## 特性

### Reactivity

### 元件化

>元件化的概念不確定是不是在學 Angular 2+ 時比較少提到或者是沒有這個觀念，在接觸 Vue 時會大大地感受到  `元件化` 這個概念，會突然有點轉不過來。

- Single Source of Truth (SSOT) 



## Reference

[1] Slots

https://www.youtube.com/watch?v=qCrogbJSHnk

[2] Watchers

https://www.youtube.com/watch?v=OEitxLemE_g

[3]

[https://codertw.com/%E5%89%8D%E7%AB%AF%E9%96%8B%E7%99%BC/219560/](https://codertw.com/前端開發/219560/)

