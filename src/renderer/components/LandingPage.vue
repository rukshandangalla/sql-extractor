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
            <el-select v-model="selectedDB" placeholder="Select Database">
              <el-option v-for="db in dbList" :key="db.value" :label="db.label" :value="db"></el-option>
            </el-select>
          </el-col>
        </el-row>
      </el-col>
      <el-col :span="18"></el-col>
    </el-row>
    <el-table :data="tableData" stripe style="width: 100%">
      <el-table-column prop="date" label="Date" width="180"></el-table-column>
      <el-table-column prop="name" label="Name" width="180"></el-table-column>
      <el-table-column prop="address" label="Address"></el-table-column>
    </el-table>
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
      tableData: [
        {
          date: '2016-05-03',
          name: 'Tom',
          address: 'No. 189, Grove St, Los Angeles'
        },
        {
          date: '2016-05-02',
          name: 'Tom',
          address: 'No. 189, Grove St, Los Angeles'
        },
        {
          date: '2016-05-04',
          name: 'Tom',
          address: 'No. 189, Grove St, Los Angeles'
        },
        {
          date: '2016-05-01',
          name: 'Tom',
          address: 'No. 189, Grove St, Los Angeles'
        }]
    };
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

      } catch (err) {
        console.log(err);
        this.$notify.error({
          title: "Error Occured",
          message: err,
          position: "bottom-right"
        });
      }
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
</style>
