# Vue 基础

## 模板语法

```vue
<template>
  <div>
    <!-- 文本插值 -->
    <span>{{ message }}</span>
    <span v-text="message"></span>
    
    <!-- HTML 渲染 -->
    <div v-html="htmlContent"></div>
    
    <!-- 属性绑定 -->
    <img :src="imageUrl" :alt="altText">
    <div :class="{ active: isActive, 'text-danger': hasError }"></div>
    <div :style="{ color: textColor, fontSize: fontSize + 'px' }"></div>
    
    <!-- 条件渲染 -->
    <p v-if="show">显示</p>
    <p v-else>隐藏</p>
    <p v-show="show">v-show</p>
    
    <!-- 列表渲染 -->
    <ul>
      <li v-for="(item, index) in items" :key="item.id">
        {{ index }} - {{ item.name }}
      </li>
    </ul>
    
    <!-- 事件处理 -->
    <button @click="handleClick">点击</button>
    <input @input="handleInput" @keyup.enter="submit">
    <form @submit.prevent="handleSubmit"></form>
    
    <!-- 双向绑定 -->
    <input v-model="inputValue">
    
    <!-- 修饰符 -->
    <input v-model.trim="inputValue">
    <input v-model.number="numValue">
    <form @submit.prevent></form>
  </div>
</template>
```

---

## Options API

```vue
<script>
export default {
  name: "MyComponent",
  
  // 数据
  data() {
    return {
      count: 0,
      message: "Hello"
    };
  },
  
  // 计算属性
  computed: {
    reversedMessage() {
      return this.message.split("").reverse().join("");
    }
  },
  
  // 方法
  methods: {
    increment() {
      this.count++;
    }
  },
  
  // 侦听器
  watch: {
    message(newVal, oldVal) {
      console.log(`message changed: ${oldVal} → ${newVal}`);
    }
  },
  
  // 生命周期钩子
  created() {
    console.log("组件创建");
  },
  mounted() {
    console.log("挂载到 DOM");
  },
  updated() {
    console.log("更新");
  },
  unmounted() {
    console.log("卸载");
  }
};
</script>
```

---

## 组件

### 父子通信

```vue
<!-- 父组件 -->
<template>
  <ChildComponent 
    :title="pageTitle"
    :items="list"
    :initialValue="100"
    @update="handleUpdate"
    @delete="handleDelete"
  />
</template>

<!-- 子组件 -->
<script>
export default {
  // 接收 Props
  props: {
    title: {
      type: String,
      required: true
    },
    items: {
      type: Array,
      default: () => []
    },
    initialValue: {
      type: Number,
      default: 0
    }
  },
  
  emits: ["update", "delete"],  // Vue 3.3+
  
  methods: {
    handleClick(id) {
      this.$emit("update", id);
      this.$emit("delete", id);
    }
  }
};
</script>
```

### 跨级通信

```vue
<!-- provide -->
<script>
import { provide } from "vue";
export default {
  setup() {
    provide("theme", "dark");
    provide("user", reactive({ name: "Alice" }));
  }
};
</script>

<!-- inject -->
<script>
import { inject } from "vue";
export default {
  setup() {
    const theme = inject("theme");
    const user = inject("user");
    return { theme, user };
  }
};
</script>
```

---

## 生命周期

```
beforeCreate → created → beforeMount → mounted 
    → beforeUpdate → updated → beforeUnmount → unmounted
```

| 钩子 | 用途 |
|------|------|
| `beforeCreate` | 实例初始化，data/methods 不可用 |
| `created` | 实例创建完成，可访问 data |
| `beforeMount` | 模板编译完成，即将挂载 |
| `mounted` | DOM 挂载完成，可操作 DOM |
| `beforeUpdate` | 状态变更，DOM 更新前 |
| `updated` | DOM 更新完成 |
| `beforeUnmount` | 组件卸载前 |
| `unmounted` | 组件卸载完成 |

---

## 响应式注意

```javascript
// ⚠️ Vue 2：数组/对象直接修改 index/property 不触发更新
this.items[0] = newValue;      // ❌ 不生效
this.$set(this.items, 0, newValue);  // ✅
this.items.splice(0, 1, newValue);   // ✅
this.items = [...this.items];         // ✅

// ⚠️ 添加新属性不触发响应
this.newProp = "value";         // ❌ 不生效
this.$set(this, "newProp", "value");  // ✅
```

---

*Related: [[02-vue/00-Index|Vue]]*