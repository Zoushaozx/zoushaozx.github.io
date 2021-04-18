---
title: vs-code
date: 2021-02-21 15:27:50
tags:
---

# 添加vue模版

---

```
安装插件 snippet
command + Shift + P
输入vue
会出现 vue.json 修改内容
	"Print to console": {
    "prefix": "vue",
    "body": [
        "<template>",
        "  <div class=\"\">$0</div>",
        "</template>",
        "",
        "<script>",
        "export default {",
        "  components:{},",
        "  data(){",
        "    return {",
        "    }",
        "  },",
        "  created(){},",
        "  mounted(){},",
        "  watch:{},",
        "  computed:{},",
        "  methods:{},",

        "}",
        "</script>",
        "<style lang=\"scss\" scoped>",
        "</style>"
    ],
    "description": "A vue file template"
}
使用，vue加Tab键
```

---



# vue代码补全插件

---

```
HTML Snippets
Vetur
Vue 2 Snippets
```

# 快捷键设置

---

```
command +k +s
就可以设置了
```

