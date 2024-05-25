# vitepress-footer
### `index.ts`的设置

需要从外部将 url 传入到组件里面，这里选择是在`theme/index.ts`中监测，并使用`provide`和`inject`进行组件间的传输

1. 在`enhanceApp({ app, router }: EnhanceAppContext)`中创建`currentUrl`,注意是要创建为响应式的，这样后面变动时，子组件也会一起更改
2. 在路由变化时，生成每次访问的新链接
3. 使用`provide`传递下去，其实这里`provide`只会在创建的时候调用一次，但由于我们数据是响应式的，所以后续变动的时候，子组件的数据也会发生变动

```ts
 enhanceApp({ app, router }: EnhanceAppContext){
    //...其他设置
  const currentUrl = ref(null);
  if (typeof window !== "undefined") {
    router.onBeforeRouteChange = (to) => {
      currentUrl.value = window.location.origin + "/" + to.split("/").slice(-3).join("/");
    };
    app.provide("currentUrl", currentUrl);
  }
}
```

### 组件的设置

组件的接收就很简单了，直接`inject`就可以了

```ts
const currentUrl = inject("currentUrl");
```
