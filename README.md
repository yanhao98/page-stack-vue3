## Page Stack for Vue3
#### [Github](https://github.com/Murphy-Tong/page-stack-vue3) / [Npm](https://www.npmjs.com/package/page-stack-vue3)

### 简介：
按照app页面栈的方式，控制路由跳转，打开新页面时保存旧页面的状态，返回时销毁页面，适合移动端H5项目

[在线预览](https://cloud.yotako.cn/ps/#/)

### Usage：

1.  安装依赖

    ```shell
    yarn add page-stack-vue3
    ```
2.  在view-router中使用
    ```javascript

    import PageStack from "page-stack-vue3";
    import { cloneVNode, defineComponent, ref, VNode } from "vue";
    import { RouteLocation, RouterView } from "vue-router";

    export default defineComponent({
    setup() {
        const psRef = ref();
        const onPageResume = function () {
        document.title = `第${psRef.value?.getPageSize() || 1}页`;
        };
        const lifeCb = {
            onResume: onPageResume,
        };
        return function () {
            return (
                <RouterView>
                    {function({Component,route}: {Component: VNode;route:RouteLocation}) {
                        return (
                            <PageStack ref={psRef} disableAnimation={true} lifeCycleCallback={lifeCb}>
                                {Component && cloneVNode(Component, { key: route.path })}
                            </PageStack>
                        );
                    }}
                </RouterView>
            );
        };
    },
    });


    ```
3.  Component Optios
    ```javascript
    {
        //vue router实例
        router: {
            type: Object as PropType<Router>,
            require: false,
        },
        //是否将路由query参数填充到页面组件的props中，router不传则此参数无效
        mergeQueryToProps: {
            type: Boolean,
            default: false,
        },
        //关闭页面切换动画
        disableAnimation: {
            type: Boolean,
            default: false,
        },
    }
    
    ```

### 注意事项

1.  如果要跟transition一起使用，请将transition作为子组件使用，并配置**disableAnimation=true**
    ```javascript
    <page-stack :disableAnimation="true">
        <transition ...props>
            <component is="xxx"/>
        </transition>
    </page-stack>
    ```
2.  本组件会重写page的key
3.  生命周期同[keep-alive](https://cn.vuejs.org/)组件
   




