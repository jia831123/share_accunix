---
theme: default
background: https://source.unsplash.com/collection/94734566/1920x1080
class: text-center
highlighter: shikiji
lineNumbers: false
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
drawings:
  persist: false
transition: slide-left
title: AccuNix 前端開發探索
mdc: true
---

# AccuNix 前端開發探索：

## 挑戰、解決方案與優化策略

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Next <carbon:arrow-right class="inline"/>
  </span>
</div>

<div class="abs-br m-6 flex gap-2">
  <button @click="$slidev.nav.openInEditor()" title="Open in Editor" class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon:edit />
  </button>
  <a href="https://github.com/slidevjs/slidev" target="_blank" alt="GitHub" title="Open in GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---

<div flex>
  <div>
    <p align="center" >
      <img width="300" src="https://accucms.accunix.net/img/logo-default.png">
    </p>
    <p flex>
      <span>
      <a href="https://github.com/vuejs/core">
          <img src="https://img.shields.io/badge/vue-3.0.5-brightgreen.svg" alt="vue">
        </a>
      </span>
      <span>
        <a href="https://github.com/element-plus/element-plus">
          <img src="https://img.shields.io/badge/element--plus-2.2.5-brightgreen.svg" alt="element-plus">
        </a>
      </span>
      <span>
        <a href="https://github.com/tailwindlabs/tailwindcss">
          <img src="https://img.shields.io/badge/tailwind-2.2.17-brightgreen.svg?style=flat" alt="tailwindcss">
        </a>
      </span>
      <span>
      <a href="https://github.com/tailwindlabs/tailwindcss">
          <img src="https://img.shields.io/badge/vite-2.4.4-brightgreen.svg?style=flat" alt="vite Welcome">
        </a>
      </span>
    </p>
  </div>
</div>
<div  grid grid-cols-2>
  <div>
    <h3>Accunix_vue3_spa</h3>
    <h2>專案建構套件</h2>
    <p>
    Vite+Vue3 + Typescript +Element Plus +
   Pinia + Tailwind css</p>
    <h2>專案緣由</h2>
    <p>
    Accunix 目前系統由 Accunix_develop 與 Accunix_vue3_spa 兩專案組成，其中 Accunix_develop 用於管理 backend 以及 frontend 的 source code，Accunix_vue3_spa 用於管理 frontend 的 source code</p>
  </div>
  <div >
    <img w-80 h-80 src="/pack_1.png"></img>
  </div>

</div>

---

## 目前專案

1. Accunix_develop:
   php laravel + Vue2 (MPA)
   (Backend+Frontend)
2. Accunix_vue3_spa:
   Vue3 (SPA)
   (Frontend)

```mermaid
flowchart LR
    Accunix_develop-->|frontend code|Accunix_vue3_spa
```

將逐漸將就有前端頁面由Accunix_develop 轉移至 Accunix_vue3_spa，新功能直接在Accunix_vue3_spa進行開發

## Accunix_develop 前端挑戰

1. 資料夾結構區分不明確
2. API呼叫散落在各個頁面，無法統一管理
3. 組件狀態依賴，除錯不易，且狀態追蹤困難

---

## 目錄

- 📁**程式結構和管理**
- 📥 **API 與 ws 呼叫架構**
- 📟 **router middleware**
- 🧑‍💻 **組件狀態**
- 🕹️ **表單驗證與資料動態檢查**
- 🔋 **functional programming**
- 🛠 **Service 化呼叫** -
- 🪬 **專案中的設計模式** -
- 🖥️ **專案 CI** -

---

## 一、 專案前端 資料夾結構

```markdown {all|2|8-11|16-18|all} twoslash
├── assets # 靜態檔案
├── components # component，與業務或資料層面無耦合之組件
├── hook # 共用hook
├── lib # lib
├── locales # i18用
├── providers # 依賴注入用
├── router # router
├── service # service
| ├──api # api 資料夾，每隻檔案對應一支api  
| ├──store # vuex store
| └──web-socket # webSocket 相關服務放置裡頭
|
├── style # css 用
├── router # router
├── type # router
├── views # 頁面views
| ├──comonents #根共用組件，與業務資料緊耦合之組件
| ├──....  
├.....
```

---

## 二、views 資料夾下配置

view 資料夾下配置

> 設計理念核心：看到url，就能找到檔案<br>
> 在views 底下 代表頁面資料夾以大寫開頭，其餘components 與 composable 皆以小寫開頭

### (一)範例1 登入頁需要修改

url

> https://accucms.accunix.net/login

目標資料夾

> views/Login/index.vue

共用組件 components
共用邏輯 composables

---

```markdown
├── views
| └──Login  
|------├──index.vue //(頁面)組件主要邏輯
|------├──components //(頁面)組件資料夾
|------------├──components //(組件)間共用組件資料夾
|------------└──composables //(組件)間共用邏輯資料夾
|------└──composables //(頁面)邏輯拆分資料夾
```

資料夾架構優勢

1. 維護容易:找尋目標頁面僅根據url 就能在對應資料夾下找到對應的組件
2. 組件與邏輯權限職責清晰:該資料夾下component 僅服務該層級中的所有組件,同理composables 僅服务該層級中的所有組件
3. 心智負擔低:views層級底下資料夾邏輯一致，擴充或抽離邏輯組件容易

---

## transition: fade-out

# What is Slidev?

Slidev is a slides maker and presenter designed for developers, consist of the following features

- 📝 **Text-based** - focus on the content with Markdown, and then style them later
- 🎨 **Themable** - theme can be shared and used with npm packages
- 🧑‍💻 **Developer Friendly** - code highlighting, live coding with autocompletion
- 🤹 **Interactive** - embedding Vue components to enhance your expressions
- 🎥 **Recording** - built-in recording and camera view
- 📤 **Portable** - export into PDF, PNGs, or even a hostable SPA
- 🛠 **Hackable** - anything possible on a webpage

<br>
<br>

Read more about [Why Slidev?](https://sli.dev/guide/why)

<!--
You can have `style` tag in markdown to override the style for the current page.
Learn more: https://sli.dev/guide/syntax#embedded-styles
-->

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

<!--
Here is another comment.
-->

---

## layout: default

# Table of contents

```html
<Toc minDepth="1" maxDepth="1"></Toc>
```

<Toc maxDepth="1"></Toc>

---

transition: slide-up
level: 2

---

# Navigation

Hover on the bottom-left corner to see the navigation's controls panel, [learn more](https://sli.dev/guide/navigation.html)

## Keyboard Shortcuts

|                                                    |                             |
| -------------------------------------------------- | --------------------------- |
| <kbd>right</kbd> / <kbd>space</kbd>                | next animation or slide     |
| <kbd>left</kbd> / <kbd>shift</kbd><kbd>space</kbd> | previous animation or slide |
| <kbd>up</kbd>                                      | previous slide              |
| <kbd>down</kbd>                                    | next slide                  |

<!-- https://sli.dev/guide/animations.html#click-animations -->

<img
  v-click
  class="absolute -bottom-9 -left-7 w-80 opacity-50"
  src="https://sli.dev/assets/arrow-bottom-left.svg"
  alt=""
/>

<p v-after class="absolute bottom-23 left-45 opacity-30 transform -rotate-10">Here!</p>

---

layout: image-right
image: https://source.unsplash.com/collection/94734566/1920x1080

---

# Code

Use code snippets and get the highlighting directly, and even types hover![^1]

```ts {all|5|1-6|9|all} twoslash
// TwoSlash enables TypeScript hover information and errors in markdown code blocks
// Learn more at https://www.typescriptlang.org/dev/twoslash/
function getUser(id: number): User {
  return undefined as any
}
function saveUser(id: number, user: User) {
  // ...
}
// ---cut---
interface User {
  id: number
  firstName: string
  lastName: string
  role: string
  // ^?
}

function updateUser(id: number, update: User) {
  const user = getUser(id)
  const newUser = { ...user, ...update }
  saveUser(id, newUser)
}
```

<arrow v-click="[3, 4]" x1="400" y1="420" x2="230" y2="330" color="#564" width="3" arrowSize="1" />

[^1]: [Learn More](https://sli.dev/guide/syntax.html#line-highlighting)

<style>
.footnotes-sep {
  @apply mt-20 opacity-10;
}
.footnotes {
  @apply text-sm opacity-75;
}
.footnote-backref {
  display: none;
}
</style>

---

# Components

<div grid="~ cols-2 gap-4">
<div>

You can use Vue components directly inside your slides.

We have provided a few built-in components like `<Tweet/>` and `<Youtube/>` that you can use directly. And adding your custom components is also super easy.

```html
<Counter :count="10" />
```

<!-- ./components/Counter.vue -->
<Counter :count="10" m="t-4" />

Check out [the guides](https://sli.dev/builtin/components.html) for more.

</div>
<div>

```html
<Tweet id="1390115482657726468" />
```

<Tweet id="1390115482657726468" scale="0.65" />

</div>
</div>

<!--
Presenter note with **bold**, *italic*, and ~~striked~~ text.

Also, HTML elements are valid:
<div class="flex w-full">
  <span style="flex-grow: 1;">Left content</span>
  <span>Right content</span>
</div>
-->

---

## class: px-20

# Themes

Slidev comes with powerful theming support. Themes can provide styles, layouts, components, or even configurations for tools. Switching between themes by just **one edit** in your frontmatter:

<div grid="~ cols-2 gap-2" m="t-2">

```yaml
---
theme: default
---
```

```yaml
---
theme: seriph
---
```

<img border="rounded" src="https://github.com/slidevjs/themes/blob/main/screenshots/theme-default/01.png?raw=true" alt="">

<img border="rounded" src="https://github.com/slidevjs/themes/blob/main/screenshots/theme-seriph/01.png?raw=true" alt="">

</div>

Read more about [How to use a theme](https://sli.dev/themes/use.html) and
check out the [Awesome Themes Gallery](https://sli.dev/themes/gallery.html).

---

## preload: false

# Animations

Animations are powered by [@vueuse/motion](https://motion.vueuse.org/).

```html
<div v-motion :initial="{ x: -80 }" :enter="{ x: 0 }">Slidev</div>
```

<div class="w-60 relative mt-6">
  <div class="relative w-40 h-40">
    <img
      v-motion
      :initial="{ x: 800, y: -100, scale: 1.5, rotate: -50 }"
      :enter="final"
      class="absolute top-0 left-0 right-0 bottom-0"
      src="https://sli.dev/logo-square.png"
      alt=""
    />
    <img
      v-motion
      :initial="{ y: 500, x: -100, scale: 2 }"
      :enter="final"
      class="absolute top-0 left-0 right-0 bottom-0"
      src="https://sli.dev/logo-circle.png"
      alt=""
    />
    <img
      v-motion
      :initial="{ x: 600, y: 400, scale: 2, rotate: 100 }"
      :enter="final"
      class="absolute top-0 left-0 right-0 bottom-0"
      src="https://sli.dev/logo-triangle.png"
      alt=""
    />
  </div>

  <div
    class="text-5xl absolute top-14 left-40 text-[#2B90B6] -z-1"
    v-motion
    :initial="{ x: -80, opacity: 0}"
    :enter="{ x: 0, opacity: 1, transition: { delay: 2000, duration: 1000 } }">
    Slidev
  </div>
</div>

<!-- vue script setup scripts can be directly used in markdown, and will only affects current page -->
<script setup lang="ts">
const final = {
  x: 0,
  y: 0,
  rotate: 0,
  scale: 1,
  transition: {
    type: 'spring',
    damping: 10,
    stiffness: 20,
    mass: 2
  }
}
</script>

<div
  v-motion
  :initial="{ x:35, y: 40, opacity: 0}"
  :enter="{ y: 0, opacity: 1, transition: { delay: 3500 } }">

[Learn More](https://sli.dev/guide/animations.html#motion)

</div>

---

# LaTeX

LaTeX is supported out-of-box powered by [KaTeX](https://katex.org/).

<br>

Inline $\sqrt{3x-1}+(1+x)^2$

Block

$$
{1|3|all}
\begin{array}{c}

\nabla \times \vec{\mathbf{B}} -\, \frac1c\, \frac{\partial\vec{\mathbf{E}}}{\partial t} &
= \frac{4\pi}{c}\vec{\mathbf{j}}    \nabla \cdot \vec{\mathbf{E}} & = 4 \pi \rho \\

\nabla \times \vec{\mathbf{E}}\, +\, \frac1c\, \frac{\partial\vec{\mathbf{B}}}{\partial t} & = \vec{\mathbf{0}} \\

\nabla \cdot \vec{\mathbf{B}} & = 0

\end{array}
$$

<br>

[Learn more](https://sli.dev/guide/syntax#latex)

---

# Diagrams

You can create diagrams / graphs from textual descriptions, directly in your Markdown.

<div class="grid grid-cols-4 gap-5 pt-4 -mb-6">

```mermaid {scale: 0.5, alt: 'A simple sequence diagram'}
sequenceDiagram
    Alice->John: Hello John, how are you?
    Note over Alice,John: A typical interaction
```

```mermaid {theme: 'neutral', scale: 0.8}
graph TD
B[Text] --> C{Decision}
C -->|One| D[Result 1]
C -->|Two| E[Result 2]
```

```mermaid
mindmap
  root((mindmap))
    Origins
      Long history
      ::icon(fa fa-book)
      Popularisation
        British popular psychology author Tony Buzan
    Research
      On effectivness<br/>and features
      On Automatic creation
        Uses
            Creative techniques
            Strategic planning
            Argument mapping
    Tools
      Pen and paper
      Mermaid
```

```plantuml {scale: 0.7}
@startuml

package "Some Group" {
  HTTP - [First Component]
  [Another Component]
}

node "Other Groups" {
  FTP - [Second Component]
  [First Component] --> FTP
}

cloud {
  [Example 1]
}

database "MySql" {
  folder "This is my folder" {
    [Folder 3]
  }
  frame "Foo" {
    [Frame 4]
  }
}

[Another Component] --> [Example 1]
[Example 1] --> [Folder 3]
[Folder 3] --> [Frame 4]

@enduml
```

</div>

[Learn More](https://sli.dev/guide/syntax.html#diagrams)

---

src: ./pages/multiple-entries.md
hide: false

---

---

layout: center
class: text-center

---

# Learn More

[Documentations](https://sli.dev) · [GitHub](https://github.com/slidevjs/slidev) · [Showcases](https://sli.dev/showcases.html)
