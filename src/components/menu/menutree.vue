<template>
  <div class="menutree">
    <label v-for="(menu, key) in menuList" :key="menu.id">
      <el-submenu :index="key + ''" v-if="menu.children">
        <template slot="title">
          <span>{{ menu.label }}</span>
        </template>
        <label>
          <menutree :menuList="menu.children"></menutree>
        </label>
      </el-submenu>
      <el-menu-item v-else :index="key + ''">
        <span slot="title">{{ menu.label }}</span>
      </el-menu-item>
    </label>
  </div>
</template>

<script>
import menutree from "@/components/menu/menutree";
export default {
  name: "menutree",
  data() {
    return {};
  },
  components: {
    menutree: menutree,
  },
  props: {
    menuList: {
      type: Array,
      default: () => {
        return [];
      },
    },
  },
};
</script>
<style lang="scss">
.el-menu--collapse .el-menu .el-submenu,
.el-menu--popup {
  min-width: 120px !important;
  padding: 0;
}
.el-menu--horizontal .el-menu .el-menu-item,
.el-menu--horizontal .el-menu .el-submenu__title {
  &:hover {
    color: #000 !important;
  }
  background-color: #fff !important;
  color: #666666 !important;
}

.el-menu--horizontal > .el-menu-item.is-active {
  background-color: #0381e9 !important;
}
</style>
