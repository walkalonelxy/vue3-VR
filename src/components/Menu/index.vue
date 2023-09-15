<template>
    <div class="menu">
        <div @click="handleSwitchMenu(menu.code)" :class="['menu__item', { active: menu.code === active }]" v-for="menu in menuList" :key="menu.code">{{ menu.name }}</div>
    </div>
</template>

<script lang="ts" setup>
import { reactive, ref, computed, watch } from 'vue'
import { useRouter } from 'vue-router'
const router = useRouter();
const active = ref();
const menuList = reactive([{
    name: 'three',
    code: '/three'
}, {
    name: 'VR',
    code: '/VR'
}]);
const handleSwitchMenu = (url: string) => {
    router.push(url)
}
watch(() => router.currentRoute.value, (route) => {
    active.value = route.path
}, {
    immediate: true
})
</script>
<style scoped lang="scss">
.menu {
    width: 100%;
    height: 30px;
    background-color: rgba(0, 0, 0, .1);
    display: flex;
    justify-content: center;

    .menu__item {
        height: 100%;
        background-color: skyblue;
        width: 70px;
        color: #fff;
        line-height: 30px;
        text-align: center;
        margin: 0 10px;
        font-size: 16px;

        &.active {
            font-weight: bold;
            color: blue;
        }
    }
}
</style>
