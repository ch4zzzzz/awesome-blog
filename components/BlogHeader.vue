<template>
    <Menu mode="horizontal" theme="light">
        <MenuItem v-for="item in showedMenuItems"
                :name="item.name"
                :key="item.id"
                class="header-menu-item">
            <nuxt-link :to="item.link">
                <Icon :type="item.icon"/>
                {{item.name}}
            </nuxt-link>
        </MenuItem>
    </Menu>
</template>

<script lang="ts">
import Vue, { VNode } from 'vue'
import Component from 'vue-class-component'

@Component
export default class BlogHeader extends Vue {
    menuItems: Array<object> = [
        {name: "首页", icon: "md-home", link: "/"},
        {name: "历史", icon: "md-time", link: "/"},
        {name: "关于", icon: "md-happy", link: "/"},
        {name: "订阅", icon: "logo-rss", link: "/"},
    ];

    get showedMenuItems () {
        return this.menuItems.slice().reverse();
    }
}


</script>

<style scoped>
.header-menu-item {
    float: right;
}
</style>


