# Vue Paginator

![ex_screenshot](./img/paginator.png)

## 의존성
- Nuxt.js
- Vuetify.js

## 사용 예시

```jsx
import Paginator from "~/components/Paginator";

export default {
  components: { Paginator },
  data() {
    return {
      searchInfo: {
        currentPage: 1,
        totalPage: 1
      }
    }
  },
  methods: {
    changePage(value) {
      //TODO: do something.
    }
  }
}
```

```html
<Paginator 
  @page="changePage"
  :cur-page="searchInfo.currentPage"
  :total-page="searchInfo.totalPage" 
/>
```