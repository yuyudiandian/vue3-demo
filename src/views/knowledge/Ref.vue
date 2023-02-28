<template>
    <div class="about">
        <div>Ref:{{ man1 }}</div>
        <hr/>
        <div>shallowRef:{{ man2 }}</div>
        <hr/>
        <div>customRef:{{ customObj }}</div>
        <hr/>
        <div ref="dom">
            我是dom
            <h5>我是h5</h5>
        </div>
        <button @click="change">点我改变</button>
    </div>
</template>

<script setup lang="ts">
import { ref, isRef, shallowRef, triggerRef, customRef } from 'vue'
const man1 = ref({ name: "zy1" })
const man2 = shallowRef({ name: 'zy12' })
const dom = ref<HTMLDivElement>()
const change = () => {
    // man1.value.name = 'zy1变身'
    //shallowRef是浅响应 这样做是不会变化的 但是如果上面作为深响应的man1产生变化 也会导致它的变化
    man2.value.name = 'zy2被zy1的变化影响了'
    // triggerRef可以强制
    triggerRef(man2)
    // 修改customRef
    customObj.value = 'zy被修改了'
    // isRef用于检测是否是ref对象
    console.log(isRef(man1) ? "是的" : "否")
    // 打印ref的dom元素
    console.log("dom结构的innerHTML是",dom.value?.innerHTML);
    
}

// customRef自定义ref 这里加了个防抖
function MyRef<T>(value: T){
    let timer:any
    return customRef((track, trigger)=>{
        return {
            get () {
                track()
                return value
            },
            set (newVal) {
                clearTimeout(timer)
                timer = setTimeout(()=>{
                    value = newVal
                    timer = null
                    trigger()      
                },500)
                
            }
        }
    })
}

const customObj = MyRef<string>("zy")

</script>

<style>

</style>
