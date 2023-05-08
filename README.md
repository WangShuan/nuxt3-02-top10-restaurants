# 世界前10排名餐廳

## 建立與啟動 Nuxt 專案

開啟終端機，`cd` 到桌面或任何希望創建該專案的位置
執行命令： 
```shell
npx nuxi init 02-top10-restaurants
```

完成後，根據提示
先 `cd` 到專案目錄 `02-top10-restaurants` 中
執行命令：
```shell
npm install
```
安裝所有依賴項目
此時會發現專案目錄中**生成了 `node_modules` 資料夾**

確認您的專案已成功安裝好所有依賴後
即可執行命令：
```shell
npm run dev
```
啟動 Nuxt 應用程序。

## 主要架構

### 引入 Bootstrap

在本專案中將使用 Bootstrap 來處理樣式與架構，
所以需要透過 CDN 引入 CSS 與 JS 至 head 標籤中。

由於在 Nuxt 開發過程中並沒有 index.html 檔案，
如果要為編譯後的 head 標籤設置 meta 等資訊，
則需要將該資訊設置在 nuxt.config.ts 檔案中：
```typescript
export default defineNuxtConfig({
  app: {
    head: {
      link: [ // 新增 link 標籤
        {
          rel: "stylesheet", // 設置 rel
          href: "https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/css/bootstrap.min.css", // 設置 href
          crossorigin: "anonymous", // 設置 crossorigin
        },
      ],
      script: [ // 新增 script
        {
          src: "https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/js/bootstrap.bundle.min.js", // 設置 src
          crossorigin: "anonymous", // 設置 crossorigin
        },
      ],
    },
  },
});
```

### 路由設置 Pages

接著我們需要創建路由，才可以連結到特定的頁面

在 Nuxt 中可以通過資料夾規則來處理路由
該規則有以下幾個重點：
1. 必須在項目根目錄中新增資料夾，且資料夾名稱規定要是 `pages`(與 `components` 資料夾一樣不能使用其他名稱否則 Nuxt 無法辨識)
2. `pages` 底下的每個 `.vue` 檔案名稱即為路徑的名稱，所以假設檔案為 `about.vue` 則在網址中輸入 `/about` 即可開啟 `about.vue` 的頁面
3. 網址中的每個 `/` 就表示一個資料夾，所以假設網址路徑為 `/restaurants/:restaurantId`，`restaurants` 就會是 `pages` 底下的 `restaurants` 資料夾
4. 動態路由的建立方式為將參數 `id` 用 `[]` 包裹，即新增檔案 `[id].vue` 於 `/pages/restaurants` 底下

現在有兩種做法可以開始建立路由檔案，
第一種是沿用 app.vue 檔案，將檔案中的內容更改為：
```htmlembedded
<!-- app.vue -->

<template>
  <Navbar />
  <div>
    <NuxtPage />
  </div>
</template>
```
並開始新增 pages 底下的檔案
第二種是刪除 app.vue 檔案，並在 pages 底下新增檔案(比如 index.vue 與 restaurants.vue 等等)
>兩者的差別在於 app.vue 是 Nuxt 的入口頁，且可以設置全局內容
>比如本專案中的 `<Navbar />` 元件要顯示於每個頁面的上方，就適合採用 app.vue 檔案將其設置為全局內容
>而如果刪除 app.vue 檔案，則不存在全局內容，新增於 pages 底下的每個 .vue 檔案就都是獨立頁面

這邊不管採用的是哪一種做法，都需要在 pages 底下新增檔案
1. /pages/index.vue 檔案為首頁
2. /pages/restaurants.vue 檔案為餐廳列表頁
3. /pages/restaurants/[id].vue 檔案為餐廳細節頁

#### 首頁 /pages/index.vue

首頁簡單顯示個標題與餐廳列表頁連結即可，
其內容如下：
```htmlembedded
<!-- /pages/index.vue -->

<template>
  <Navbar />
  <div class="bg-white vh-100 text-center text-dark d-flex flex-column justify-content-center position-relative p-3">
    <h1 class="fw-bolder">Welcome To Top 30 Restaurants</h1>
    <NuxtLink class="link-success fw-bold fs-4" to="/restaurants">👉 Go To See The Top 30 Restaurant List</NuxtLink>
  </div>
</template>
```

#### 餐廳列表頁 /pages/restaurants.vue

餐廳列表頁主要會按照排名依序顯示餐廳名稱、點擊後可導連至餐廳細節頁，
其內容如下：
```htmlembedded
<!-- /pages/restaurants.vue -->

<template>
  <Navbar />
  <div class="my-5 pt-5 text-center text-dark d-flex flex-column justify-content-center container">
    <h1 class="lh-lg fw-bolder">Top 30 Restaurant List</h1>
    <div class="row">
      <div class="col-md-6 col-xl-4" v-for="item in restaurants" :key="item">
        <NuxtLink class="text-decoration-none" :to="`/restaurants/${item.id}`">
          <div class="card my-3 overflow-hidden">
            <div class="ratio ratio-16x9 card-img">
              <img :src="item.imageUrl" class="img-fluid">
            </div>
            <div class="d-flex flex-column justify-content-end">
              <div class="d-flex align-items-center justify-content-center bg-white px-2 py-1">
                <span class="fw-bolder rounded-pill badge" style="font-size: 14px;">
                  Top {{ item.rank }}
                </span>
                <h2 class="fs-5 fw-bolder px-2 py-1 m-0">
                  {{ item.name }}
                </h2>
              </div>
            </div>
          </div>
        </NuxtLink>
      </div>
    </div>
  </div>
</template>
```

#### 餐廳細節頁 /pages/restaurants/[id].vue

餐廳細節頁主要顯示所有當前餐廳的相關資訊，
其內容如下：
```htmlembedded
<!-- /pages/restaurants/[id].vue -->

<template>
  <Navbar />
  <div class="container my-5 pt-5">
    <div class="row g-0 justify-content-center">
      <div class="col-xl-8">
        <img class="img-fluid w-100" :src="restaurant.imageUrl" alt="">
      </div>
      <div class="col-xl-8 p-3 p-md-5 bg-light d-flex flex-column justify-content-between">
        <div class="d-flex flex-column align-items-start">
          <small class="badge rounded-pill text-bg-danger">TOP {{ restaurant.rank }}</small>
          <h1 class="text-success fw-bolder my-2 fs-3">{{ restaurant.name }}</h1>
        </div>
        <p class="lh-lg my-3 me-0 me-xl-5">{{ restaurant.content }}</p>
        <span>
          年營收：{{ restaurant.revenue }} 億美元
          /
          店舖數量：{{ restaurant.numberOfStores }} 家
        </span>
      </div>
    </div>
  </div>
</template>
```

#### 餐廳的數據 data.json

接著需要建立餐廳列表的數據資料，於項目根目錄中新增檔案 data.json：
```json
[
  {
    "id": 1,
    "rank": 1,
    "name": "麥當勞 McDonalds",
    "content": "麥當勞是源自美國南加州的跨國連鎖速食店，也是世界最大的速食連鎖店，主要販售漢堡及薯條、炸雞、碳酸飲料、冰品、沙拉、水果、咖啡等速食食品，目前總部位於美國芝加哥，根據麥當勞2022年公布的年報，截至2021年底全球共有40,031間店，分佈在119個國家及地區。",
    "revenue": 42.4,
    "numberOfStores": "39,222",
    "imageUrl": "https://images.unsplash.com/photo-1602400236316-f5e3b6d2314c?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=5070&q=80"
  },
  {
    "id": 2,
    "rank": 2,
    "name": "星巴克 Starbucks",
    "content": "星巴克股份有限公司，簡稱星巴克，又譯史塔巴克斯，是美國一家跨國連鎖咖啡店，也是全球最大的連鎖咖啡店，成立於1971年，發源地與總部位於美國華盛頓州西雅圖。除咖啡之外，亦有茶飲等飲料，以及三明治、糕點等點心類食品。",
    "revenue": 26.5,
    "numberOfStores": "33,295",
    "imageUrl": "https://images.unsplash.com/photo-1592321675774-3de57f3ee0dc?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxzZWFyY2h8MTV8fHN0YXJidWNrc3xlbnwwfHwwfHw%3D&auto=format&fit=crop&w=800&q=60"
  },
  ...
]
```

#### 數據 data.json 使用方式

在餐廳列表頁面中引入數據資料：
```htmlembedded
<!-- /pages/restaurants.vue -->

<script setup lang="ts">
import restaurants from "@/data.json"; // 引入數據資料
</script>
```

在餐廳細節頁面中接收傳入的 props 並為其設置好類型：
```htmlembedded
<!-- /pages/restaurants/[id].vue -->

<script setup lang="ts">
interface Restaurant {
  restaurant: {
    id: number;
    rank: number;
    name: string;
    content: string;
    revenue: number;
    numberOfStores: string;
    imageUrl: string;
  }
}
const props = defineProps<Restaurant>()
</script>
```

## 元件化

### Navbar 元件化

在本專案中，頁面的部分主要會有首頁、餐廳列表頁、餐廳細節頁三種頁面
且每個頁面的頂部都有 Navbar，這邊可以直接從 [Bootstrap](https://bootstrap5.hexschool.com/docs/5.1/components/navbar/) 中拷貝來使用
首先在根目錄中新增一個叫做 components 的資料夾，接著創建一個 Navbar.vue 檔案放置在 components 資料夾中
最後再將 Bootstart 中拷貝的導覽列放置到 Navbar.vue 檔案中即可

### 餐廳列表頁卡片元件化

在餐廳列表頁面中可以將 v-for 中的內容封裝成元件使用，
修改如下：
```htmlembedded
<!-- /pages/restaurants.vue -->

<template>
  <Navbar />
  <div class="my-5 pt-5 text-center text-dark d-flex flex-column justify-content-center container">
    <h1 class="lh-lg fw-bolder">Top 30 Restaurant List</h1>
    <div class="row">
      <div class="col-md-6 col-xl-4" v-for="item in restaurants" :key="item">
        <RestaurantCard :restaurant="item" />
      </div>
    </div>
  </div>
</template>
```

於 components 資料夾中新增的檔案 RestaurantCard.vue 
並在檔案中接收傳入的 props 為其設置類型：
```htmlembedded
<!-- /components/RestaurantCard.vue -->

<template>
  <NuxtLink class="text-decoration-none" :to="`/restaurants/${restaurant.id}`">
    <div class="card my-3 overflow-hidden">
      <div class="ratio ratio-16x9 card-img">
        <img :src="restaurant.imageUrl" class="img-fluid">
      </div>
      <div class="d-flex flex-column justify-content-end">
        <div class="d-flex align-items-center justify-content-center bg-white px-2 py-1">
          <span class="fw-bolder rounded-pill badge" style="font-size: 14px;">
            Top {{ restaurant.rank }}
          </span>
          <h2 class="fs-5 fw-bolder px-2 py-1 m-0">
            {{ restaurant.name }}
          </h2>
        </div>
      </div>
    </div>
  </NuxtLink>
</template>

<script setup lang="ts">
interface Restaurant {
  restaurant: {
    id: number;
    rank: number;
    name: string;
    content: string;
    revenue: number;
    numberOfStores: string;
    imageUrl: string;
  }
}
const props = defineProps<Restaurant>()
</script>
```

## 結構化 layouts

在每個頁面中最常見的重複區域不外乎 header 與 footer
本專案中的三種頁面都具有 `<Navbar />` 元件在頁面最上方
此時適合使用 layout 來進行處理
1. 在項目根目錄中新增名稱為 layouts 的資料夾
2. 在 layouts 的資料夾中建立預設結構的檔案，名稱為 default.vue
3. dafault.vue 中內容如下：
```htmlembedded
<template>
  <!-- 放置共用的 Navbar 元件 -->
  <Navbar /> 

  <!-- 使用插槽表示 pages 內容要顯示的位置 -->
  <slot></slot>
  
  <!-- 放置共用的 footer -->
  <footer class="bg-dark text-light m-0 p-2 text-center">
    <small>此網站僅供學習不含商業用途。</small>
  </footer>
</template>
```

接著在 app.vue 檔案中將所有內容使用 <NuxtLayout> 標籤包住即可：
```htmlembedded
<template>
  <div>
    <NuxtLayout>
      <NuxtPage />
    </NuxtLayout>
  </div>
</template>
```

除了預設的 default.vue 檔案外
也可在其他有重複佈局的地方新增其他客製化佈局
舉例來說假設每個頁面的 container 都是使用 div 搭配固定的 class
此時可以新增檔案 custom.vue(名稱隨意) 將 container 做成客製化佈局
接著在需要使用的頁面中一樣通過 <NuxtLayout> 標籤包住所有內容
並在 <NuxtLayout> 標籤中新增屬性 name 設置 layout 名稱(值為 .vue 檔案的名稱)
舉例如下：
```htmlembedded
<template>
  <div>
    <NuxtLayout name="custom">
      ...
    </NuxtLayout>
  </div>
</template>
```
>假設新增的客製化佈局名稱為 custom.vue 則 name 屬性值就為 custom
>依此類推，假設客製化佈局名稱為 foobar.vue 則 name 屬性值就為 foobar

## 錯誤處理 error.vue

在創建 Nuxt3 項目後，預設提供了一個錯誤頁面
對於常規的錯誤處理可使用預設的頁面滿足大部分需求
假設需要自定義錯誤頁面則可於項目根目錄中新增檔案 error.vue 自行設置頁面

另外在需要手動拋出錯誤的地方可以使用 `createError` 自定義錯誤內容
比如餐廳細節頁面，假設輸入不存在的餐廳網址則拋出自定義錯誤以顯示 error.vue 畫面：
```htmlembedded=
<script setup>
import restaurants from "@/data.json";
interface Restaurant {
  id: number;
  rank: number;
  name: string;
  content: string;
  numberOfStores: string;
  imageUrl: string;
}
const currentId = Number(useRoute().params.id);
const restaurant = restaurants.find(item => item.id === currentId) as Restaurant;

if (!restaurant) { // 當 restaurant 不存在
  throw createError({ // 拋出自定義錯誤，Nuxt 將自動切換為錯誤頁面
    statusCode: 404, // 設置狀態碼
    message: "This ID not found, please try again." // 設置訊息
  })
}
</script>
```

假設使用了 `throw createError` 則必須在 `error.vue` 中通過 `clearError` 清除錯誤並跳轉至首頁或其他頁面：
```htmlembedded=
<template>
  <NuxtLayout>
    <div
      class="bg-opacity-25 bg-success vh-100 text-center text-dark d-flex flex-column justify-content-center align-items-center p-3">
      <h1 class="fw-bolder">{{ err.statusCode }}</h1>
      <p>{{ err.message }}</p>
      <button @click="handleError()" class="btn btn-outline-success fw-bold" to="/restaurants">Go Home</button>
    </div>
  </NuxtLayout>
</template>

<script setup>
const err = useError();

const handleError = () => clearError({ redirect: '/' })
</script>
```

## SEO 等 meta 設置

關於網頁中的 SEO 相關設定方式有兩種：
1. 使用 Nuxt3 提供的元件(Html, Title, Link 等等)
2. 使用 Nuxt3 提供的 useHead 方法