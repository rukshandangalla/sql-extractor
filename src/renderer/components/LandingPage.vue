<template>
  <div id="wrapper" class="wrapper">
    <el-row :gutter="20">
      <el-col :span="6">
        <el-divider>
          <b>Connection</b>
        </el-divider>
        <el-row :gutter="20">
          <el-col>
            <el-input placeholder="Server Name With Port" v-model="serverName"></el-input>
          </el-col>
        </el-row>

        <el-row :gutter="20">
          <el-col>
            <el-input placeholder="SQL User Name" v-model="username"></el-input>
          </el-col>
        </el-row>

        <el-row :gutter="20">
          <el-col>
            <el-input placeholder="Sql Password" v-model="password" show-password></el-input>
          </el-col>
        </el-row>

        <el-row :gutter="20">
          <el-col>
            <el-button type="primary" @click="connect">Connect</el-button>
          </el-col>
        </el-row>

        <el-row :gutter="20" v-if="dbList.length > 0">
          <el-col>
            <el-divider>
              <b>Databases</b>
            </el-divider>
            <el-select
              v-model="selectedDB"
              placeholder="Select Database"
              @change="onDBSelect($event)"
            >
              <el-option v-for="db in dbList" :key="db.value" :label="db.label" :value="db"></el-option>
            </el-select>
          </el-col>
        </el-row>
      </el-col>
      <el-col :span="1">
        <div class="main-seperator"></div>
      </el-col>
      <el-col :span="17">
        <el-input
          placeholder="Search SP.."
          prefix-icon="el-icon-search"
          v-model="spSearchText"
          @change="onSearchTextChange()"
        ></el-input>
        <el-table
          :data="filteredSPList"
          stripe
          style="width: 100%"
          height="400"
          ref="multipleTable"
          @selection-change="handleSelectionChange"
        >
          <el-table-column type="selection" width="55"></el-table-column>
          <el-table-column prop="name" label="SP Name"></el-table-column>
        </el-table>
      </el-col>
    </el-row>
  </div>
</template>

<script>
const sql = require("mssql");

export default {
  name: "landing-page",
  components: {},
  data() {
    return {
      serverName: "devdrdbnode01,7947",
      username: "sa",
      password: "qaz@123",
      dbList: [],
      selectedDB: {},
      spList: [],
      filteredSPList: [],
      spSearchText: ''
    };
  },
  mounted() {
    
  },
  methods: {
    async connect() {
      try {
        const connection = `mssql://${this.username}:${this.password}@${this.serverName}`;
        await sql.connect(connection);

        const result = await sql.query(`SELECT database_id,name FROM sys.databases`);

        this.$notify({
          title: "Connected",
          message: "Connected to server",
          position: "bottom-right",
          type: "success"
        });

        this.dbList = [];
        result.recordset.forEach(r => {
          this.dbList.push({ value: r.database_id, label: r.name });
        });

        sql.close();
      } catch (err) {
        console.log(err);
        this.$notify.error({ title: "Error Occured", message: err, position: "bottom-right" });
      }
    },
    async onDBSelect(evt) {
      try {
        const connection = `mssql://${this.username}:${this.password}@${this.serverName}/${this.selectedDB.label}`;
        await sql.connect(connection);

        const result = await sql.query(`SELECT ROUTINE_SCHEMA, ROUTINE_NAME FROM INFORMATION_SCHEMA.ROUTINES WHERE ROUTINE_TYPE = 'PROCEDURE';`);

        this.spList = [];
        result.recordset.forEach(r => {
          this.spList.push({ name: `[${r.ROUTINE_SCHEMA}].[${r.ROUTINE_NAME}]` });
        });

        this.filteredSPList = this.spList;

        sql.close();

      } catch (err) {
        console.log(err);
        this.$notify.error({ title: "Error Occured", message: err, position: "bottom-right" });
      }
    },
    handleSelectionChange(val) {
      this.multipleSelection = val;
    },
    onSearchTextChange() {
      
    }
  }
};
</script>

<style scoped>
.wrapper {
  padding: 10px;
}
.el-row {
  margin-bottom: 20px;
}
.el-row:last-child {
  margin-bottom: 0;
}
.main-seperator {
  height: 30vh;
  width: 1px;
  margin-top: 50px;
  background-color: #dcdfe6;
}
</style>
