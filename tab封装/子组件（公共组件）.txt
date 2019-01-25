<template>
  <el-tabs type="border-card" :tabData="data" v-model="tabsActive" @tab-click="onTabsClick">
    <el-tab-pane v-for="item in data" :label="item.label" :name="item.name" :key="item.name">
      <component :is="item.component" :ref="item.name" :v-permission="item.permission" />
    </el-tab-pane>
  </el-tabs>
</template>

<script>
export default {
  props: {
    data: Array,
    active: String
  },
  data() {
    return {
      tabData: [],
      tabsActive: this.active,
      state: {}
    };
  },
  mounted() {
    this.initRecursion();
  },
  created() {
    this.setStatusValue();
  },
  methods: {
    setStatusValue() {
      this.$set(this.state, this.active, true);
    },
    onTabsClick(tab) {
      this.tabsActive = tab.name;
      if (!this.state[this.tabsActive]) {
        this.state[this.tabsActive] = true;
        this.$refs[this.tabsActive][0].init &&
          this.$refs[this.tabsActive][0].init();
      }
    },
    initRecursion() {
      this.$nextTick(() => {
        if (this.$refs[this.tabsActive]) {
          this.$refs[this.tabsActive][0].init &&
            this.$refs[this.tabsActive][0].init();
        } else {
          setTimeout(() => {
            this.initRecursion();
          }, 200);
        }
      });
    }
  }
};
</script>

<style lang="scss" scoped>
.el-tabs {
  height: 100%;

  /deep/ {
    .el-tabs__content {
      height: calc(100% - 43px);
      padding: 0;
      overflow: auto;
    }
    .el-tabs__header {
      background: #fafbfd;
    }
    .el-tabs__item {
      width: 120px;
      height: 48px;
      line-height: 48x;
      padding: 0 !important;
      border-right: 1px solid #e4e5e6;
      text-align: center;
      font-size: 14px;
      color: #666;
      &.is-active {
        background: #eaeaea;
        font-weight: bold;
        color: #353535;
      }
    }
    .el-tab-pane {
      height: 100%;
    }
    .el-tabs__active-bar {
      display: none;
    }
  }
}
</style>