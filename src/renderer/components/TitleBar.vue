<template>
  <div id="title_bar" class="title_bar">
    <div class="title">
      Eclipse Script Generator
    </div>
    <div class="btn_section">
      <el-row :gutter="10">
        <el-col :span="12">
          <el-button
            type="warning"
            icon="el-icon-minus"
            circle
            size="small"
          ></el-button>
        </el-col>
        <el-col :span="12">
          <el-button
            type="danger"
            icon="el-icon-close"
            circle
            size="small"
            @click="quitApp()"
          ></el-button>
        </el-col>
      </el-row>
    </div>
  </div>
</template>

<script>
const { remote } = require("electron");

export default {
  name: "title-bar",
  methods: {
    quitApp() {
      remote.app.quit();
    }
  }
};
</script>

<style>
.title_bar {
  height: 40px;
  background-color: black;
  -webkit-app-region: drag;
}
.title_bar:hover {
  cursor: pointer;
}
.title_bar .title {
  float: left;
  color: white;
  padding: 10px;
}
.title_bar .btn_section {
  -webkit-app-region: no-drag;
  float: right;
  margin: 8px 15px 0 0;
}
.title_bar .el-button--small.is-circle {
  padding: 5px;
}
</style>
