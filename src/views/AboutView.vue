<template>
    <div class="about">
        <h1>This is an about page1{{ msg }}</h1>
        <!-- <button @click.once="con1(this.count)">点我有事件发生</button> -->
        <h1>改变次数{{count}}</h1>
    </div>
    <div>
        <div @click="select(item.id)" :key="item.id" v-for="(item) in arr" v-memo="[item.id === active]">
            {{ item.id }} - selected： {{ item.id === active }}
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref, reactive } from 'vue'
var count: number = ref(1)
const msg: string = ref<string>('test')
const arr = reactive<any[]>([])
for (let i = 0; i < 10000; i++) {
    arr.push({
        id: i + 1,
        name: "test"
    })
}

const active = ref(1)

const select = async (index: number) => {
    active.value = index;
    count.value++
    console.time()
    await Promise.resolve()
    console.timeEnd()
}
</script>

<style>
@media (min-width: 1024px) {
    .about {
        min-height: 100vh;
        display: flex;
        align-items: center;
    }
}
</style>
