<template>
  <div id="wrapper" class="wrapper">
    <el-button type="success" @click="connect">Connect</el-button>
  </div>
</template>

<script>
const sql = require('mssql')
import SystemInformation from "./LandingPage/SystemInformation";

export default {
  name: "landing-page",
  components: { SystemInformation },
  methods: {
    async connect() {
      try {
        // make sure that any items are correctly URL encoded in the connection string
        //await sql.connect("Driver=SQL Server;Server=devdrdbnode01,7947;Database=EclipseDev;Trusted_Connection=true;");
        await sql.connect('mssql://sa:qaz@123@devdrdbnode01,7947/Master')
        const result = await sql.query(`SELECT name FROM sys.databases`);
        console.log(result.recordset);
      } catch (err) {
       console.log(err);
      }
    }
  }
};
</script>

<style scoped>
.wrapper {
  padding: 10px;
}
</style>
