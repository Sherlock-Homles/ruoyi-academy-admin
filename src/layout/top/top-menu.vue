<template>
  <div class="pull-auto top-menu">
    <el-menu
      mode="horizontal"
      text-color="#fff"
      active-text-color="#fff"
      background-color="transparent"
      :default-active="activeIndex"
      @select="handleSelect"
    >
      <template>
        <el-menu-item
          v-for="(item, key) in topMenuList"
          :key="key"
          :index="key + ''"
        >
          <svg-icon :icon-class="item.meta.icon" />
          <span slot="title">{{ item.meta.title }}</span>
        </el-menu-item>
      </template>
    </el-menu>
  </div>
</template>

<script>
import { mapGetters } from "vuex";
import menutree from "@/components/menu/menutree";
export default {
  name: "top-menu",
  props: {
    items: {
      type: Array,
      default: [],
    },
  },
  comments: { menutree },
  data() {
    return {
      activeIndex: "",
      topMenuList: [],
      currentMenu: "",
      leftMenuList: [],
    };
  },
  computed: {
    ...mapGetters(["tagCurrent", "menu", "projectMenuType", "tagList", "tag"]),
  },
  created() {
    this.handleSelect(0);
  },
  methods: {
    handleSelect(key) {
      let menuList = [];
      this.topMenuList = this.items;
      menuList = JSON.parse(JSON.stringify(this.items));
      this.activeIndex = key + "";
      this.currentMenu = menuList[key];
      this.getLeftMenuList(this.currentMenu);
      this.$tab.closeAllPage();
    },
    getLeftMenuList(moduleMenu) {
      if (moduleMenu.children && moduleMenu.children.length > 0) {
        this.leftMenuList = JSON.parse(JSON.stringify(moduleMenu.children));
        this.$router.push(
          `${moduleMenu.path}/${this.leftMenuList[0].path}/${this.leftMenuList[0].children[0].path}`
        );
      } else {
        this.leftMenuList = JSON.parse(JSON.stringify([moduleMenu]));
        this.leftMenuList[0].children = JSON.parse(
          JSON.stringify([moduleMenu])
        );
        this.$router.push(moduleMenu.path);
      }
      this.$emit("leftMenuList", this.leftMenuList, moduleMenu.path);
    },
  },
};
</script>

<style scoped lang="scss">
.top-menu {
  v-deep .el-menu--horizontal > .el-menu-item {
    float: left;
    height: 50px;
    line-height: 45px;
    font-size: 20px;
    width: 137px;
    padding: 0;
    color: #121212 !important;
    border: 1px solid #dfeaff !important;
    background-color: #dfeaff !important;
    font-weight: 400;
    margin-left: 7px;
    // width: 137px;
    margin-top: 10px;
    text-align: center;
    font-family: Source Han Sans CN;
    border-radius: 6px;
  }
  v-deep .el-menu--horizontal > .el-menu-item.is-active {
    color: #fff !important;
    border: 1px solid #3175ff !important;
    background-color: #3175ff !important;
    i {
      color: #fff !important;
      font-size: 20px;
    }
  }
  i {
    color: #2c313c !important;
    font-size: 20px;
  }
  .tunnyicon {
    margin-bottom: -2px;
  }
}
</style>
