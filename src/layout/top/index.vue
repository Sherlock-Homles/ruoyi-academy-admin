<template>
  <el-container class="container">
    <div class="logo-box">
      <div class="logo">
        <el-image
          class="img"
          :src="require('@/assets/logo/logo.png')"
          fit="contain"
        ></el-image>
        <span class="logo-title">XXX学院</span>
      </div>
    </div>
    <div class="top-menu">
      <topMenu
        ref="topMenuComponent "
        :items="menuList"
        @leftMenuList="getLeftMenuList"
      />
    </div>
    <div class="user-box">
      <div class="right-menu">
        <template v-if="device !== 'mobile'">
          <search id="header-search" class="right-menu-item" />
          <screenfull id="screenfull" class="right-menu-item hover-effect" />
        </template>

        <el-dropdown
          class="avatar-container right-menu-item hover-effect"
          trigger="click"
        >
          <div class="avatar-wrapper">
            <img :src="avatar" class="user-avatar" />
            <i class="el-icon-caret-bottom" />
          </div>
          <el-dropdown-menu slot="dropdown">
            <router-link to="/user/profile">
              <el-dropdown-item>个人中心</el-dropdown-item>
            </router-link>
            <el-dropdown-item divided @click.native="logout">
              <span>退出登录</span>
            </el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
      </div>
    </div>
  </el-container>
</template>

<script>
import { mapGetters } from "vuex";
import { getRouters } from "@/api/menu";
import Hamburger from "@/components/Hamburger";
import Screenfull from "@/components/Screenfull";
import Search from "@/components/HeaderSearch";
import topMenu from "./top-menu.vue";
export default {
  data() {
    return {
      menuList: [],
    };
  },
  components: {
    Hamburger,
    Screenfull,
    Search,
    topMenu,
  },
  created() {
    this.init();
  },
  computed: {
    ...mapGetters(["sidebar", "avatar", "device"]),
    activeMenu() {
      const route = this.$route;
      const { meta, path } = route;
      // if set path, the sidebar will highlight the path you set
      if (meta.activeMenu) {
        return meta.activeMenu;
      }
      return path;
    },
    setting: {
      get() {
        return this.$store.state.settings.showSettings;
      },
      set(val) {
        this.$store.dispatch("settings/changeSetting", {
          key: "showSettings",
          value: val,
        });
      },
    },
    topNav: {
      get() {
        return this.$store.state.settings.topNav;
      },
    },
  },
  methods: {
    toggleSideBar() {
      this.$store.dispatch("app/toggleSideBar");
    },
    async logout() {
      this.$confirm("确定注销并退出系统吗？", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      })
        .then(() => {
          this.$store.dispatch("LogOut").then(() => {
            location.href = "/index";
          });
        })
        .catch(() => {});
    },
    init() {
      this.menuList = [];
      this.menuList.push({
        path: "/index",
        meta: { title: "首页", icon: "dashboard", affix: true },
      });
      getRouters().then((res) => {
        res.data.forEach((item) => {
          this.menuList.push(item);
        });
      });
    },
    getLeftMenuList(list, moduleMenu) {
      this.$emit("leftMenuList", list, moduleMenu);
    },
  },
};
</script>

<style lang="scss" scoped>
.container {
  height: 100%;
  justify-content: space-between;
  background: linear-gradient(90deg, #266fd4 10.45%, #f3f8ff 96.35%);
  background-repeat: no-repeat;
  background-size: auto 100%;
  .logo-box {
    width: 394px;
    justify-content: space-between;
    .fold {
      width: 60px;
      height: 60px;
      background: #0381e9;
      color: white;
    }

    .logo {
      margin-left: 23px;
      margin-top: 10px;
      width: 394px;
      height: 60px;

      .img {
        width: 51px;
        height: 34px;
      }
      .logo-title {
        font-weight: 700;
        font-size: 30px;
        color: #ffffff;
        margin-left: 13px;
      }
    }

    .text {
      height: 60px;
      line-height: 60px;
      // margin-left: 30px;
      font-size: 16px;
      font-family: Microsoft YaHei;
      font-weight: bold;
      color: #ffffff;
    }
  }
  .user-box {
    .right-menu {
      float: right;
      height: 100%;
      line-height: 50px;

      &:focus {
        outline: none;
      }

      .right-menu-item {
        display: inline-block;
        padding: 0 8px;
        height: 100%;
        font-size: 18px;
        color: #5a5e66;
        vertical-align: text-bottom;

        &.hover-effect {
          cursor: pointer;
          transition: background 0.3s;

          &:hover {
            background: rgba(0, 0, 0, 0.025);
          }
        }
      }

      .avatar-container {
        margin-right: 30px;

        .avatar-wrapper {
          margin-top: 5px;
          position: relative;

          .user-avatar {
            cursor: pointer;
            width: 40px;
            height: 40px;
            border-radius: 10px;
          }

          .el-icon-caret-bottom {
            cursor: pointer;
            position: absolute;
            right: -20px;
            top: 25px;
            font-size: 12px;
          }
        }
      }
    }
  }
}
</style>
