<template>
    <div class="about">
        <div class="left">
            <form action="">
                    <input v-model="formData.name" type="text">
                    <span>输入用户的名称为：{{ formData.name }}</span>
                    <br>
                    <input v-model="formData.age" type="text">
                    <span>输入用户的年龄为：{{ formData.age }}</span>
                    <br>
                    <!-- 防止默认提交事件造成浏览器刷新 -->
                    <button @click.prevent="submit">注册</button>
            </form>
            <form action="">
                    <input v-model="formData2.name" type="text">
                    <span>输入用户的名称为：{{ formData.name }}</span>
                    <br>
                    <input v-model="formData2.age" type="text">
                    <span>输入用户的年龄为：{{ formData.age }}</span>
                    <br>
                    <!-- 防止默认提交事件造成浏览器刷新 -->
                    <button @click.prevent="add">添加用户数据</button>
                </form>
        </div>
        <div class="right">
            <ul v-for="item in usersList" :key="item.id">
                <li>用户id：{{ item.id }}</li>
                <li>用户name：{{ item.name }}</li>
                <li>用户age：{{ item.age }}</li>
            </ul>      
        </div>
        <div>
            <span>测试ref和shallowRef是否会相互影响</span>
            <span>{{ test1 }}</span>
            <span>{{ test2 }}</span>
            <button @click="modify">点击修改</button>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref, reactive, readonly, shallowRef } from 'vue'

type formProps = {
    name: string,
    age: number
}
// ref支持所有类型 reactive只支持引用类型
const formData = reactive<formProps>({
    name: 'zy',
    age: 23
})
type formProps2 = {
    id: number,
    name: string,
    age: number | null
}
const formData2 = reactive<formProps2>({
    id: Math.floor(Math.random()*1000+1),
    name: 'zy',
    age: 23
})

const usersList = reactive<formProps2[]>([])

//ref取值和赋值都需要加value,但是reactive不需要
// formData.age = 86
const submit = () => { 
    console.log('提交成功！');
}
// reactive不支持直接引用赋值 否则会破坏响应式对象
// let arr = reactive<number[]>([1])
// arr = [1,2,3]
// console.log(arr)

// readonly可以将reactive创建的对象直接赋值为只读的
// let obj = reactive({ name: '1' })
// const read = readonly(obj)
// read.name = '2'


// 添加用户数据
const add = () => { 
    usersList.push({ ...formData2, id: Math.floor(Math.random() * 1000 + 1) })
    formData2.name = ''
    formData2.age = null
    console.log('添加用户数据成功')
}

// 测试ref是否会被shallowRef影响
// 实际上ref更新会触发模板的渲染，所有一单ref更新，所有的组件模板都会更新为最新的数据
const test1 = ref({ name: 'test1' })
const test2 = shallowRef({ name: 'test2' })
const modify = () => {
    // test1.value.name = "已经修改了test1"
    test2.value.name = "已经修改了test2"
    console.log('test2=', test2);
    
}

</script>

<style scoped>
.about{
    display: flex;
    justify-content: space-around;
}
</style>
