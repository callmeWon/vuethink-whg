<template>
	<el-dialog ref="dialog" :visible="opDialog" custom-class="w-800 h-500 ovf-auto" title="节点列表">
		<div class="pos-rel h-60">
			<el-input placeholder="请输入内容" v-model="keyword" class="search-btn w-300">
				<el-button slot="append" icon="el-icon-search" @click="searchMsg()"></el-button>
			</el-input>
		</div>
		<div>
			<el-table :data="tableData" stripe style="width: 100%">
				<el-table-column prop="level" label="级别"	width="100"></el-table-column>
        <el-table-column prop="name" label="规则标识" width="180"></el-table-column>
				<el-table-column prop="title" label="规则名称"></el-table-column>
				<el-table-column label="操作" width="100">
          <template slot-scope="scope">
            <el-button size="small" @click="selectRule(scope.row)">选择</el-button>
          </template>
				</el-table-column>
			</el-table>
		</div>
    <span slot="footer" class="dialog-footer">
      <el-button @click="closeDialog()">取 消</el-button>
      <!-- <el-button type="primary" @click="centerDialogVisible = false">确 定</el-button> -->
  </span>
	</el-dialog>
</template>

<script>
  import http from '../../../../assets/js/http'

  export default {
    data() {
      return {
        keyword: '',
        tableData: [],
        opDialog:false,
      }
    },
    methods: {
      open() {
        this.$refs.dialog.open();
        this.opDialog = true;
        // console.log(this.tableData)
      },
      closeDialog() {
        this.$refs.dialog.close();
        this.opDialog = false;
      },
      selectRule(item) {
        setTimeout(() => {
          this.$parent.form.rule_name = item.title
          this.$parent.form.rule_id = item.id
        }, 0)
        this.closeDialog()
      },
      getRules() {
        this.apiGet('admin/rules').then((res) => {
          this.handelResponse(res, (data) => {
            this.tableDataShow = _(data).forEach((ret) => {
              ret.showInput = false
            })
            this.tableData = this.tableDataShow
          })
        })
      }
    },
    created() {
      let data = store.state.rules
      if (data && data.length) {
        this.tableDataShow = _(data).forEach((res) => {
          res.showInput = false
        })
        this.tableData = this.tableDataShow
      } else {
        this.getRules()
      }
    },
    mixins: [http]
  }
</script>

<style>
.search-btn {
	position: absolute;
	top: 0px;
	right: 0px;
}
</style>