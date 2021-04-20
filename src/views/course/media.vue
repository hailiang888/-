<template>
  <div>
    <el-row class="head">
      <el-col :span="24">
        <el-button
          type="primary"
          class="el-icon-edit el-input__icon"
          style="width: 10%; margin-left: 1%"
        >
          新增图文</el-button
        >
        <!-- </el-col> -->
        <!-- <el-col :span="24"> -->
        <el-select
          v-model="listQuery.publishStatus"
          placeholder="商品状"
          style="margin-left: 50%; width: 5%"
        >
          <el-option
            v-for="item in publishStatusOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          >
          </el-option>
        </el-select>
        <el-input
          v-model="listQuery.username"
          placeholder="标题"
          style="width: 15%; margin-left: 1%"
        ></el-input>
        <el-button
          type="primary"
          icon="el-icon-search"
          @click="handleSearchList"
          >搜索</el-button
        >
      </el-col>
    </el-row>
    <el-row>
      <el-col>
        <el-table :data="tableData" border style="width: 100%">
          <el-table-column prop="id" label="ID" width="150" align="center">
          </el-table-column>
          <el-table-column label="图文内容" min-width="180px">
            <template slot-scope="{ row }">
              <div style="display: flex">
                <img
                  :src="row.cover"
                  style="width: 100px; height: 50px; margin-right: 10px"
                />
                <div
                  style="
                    display: flex;
                    flex-direction: column;
                  "
                >
                  <span>{{ row.title }}</span>
                  <span style="color: red">￥{{ row.price }}</span>
                </div>
              </div>
            </template>
          </el-table-column>
          <el-table-column
            prop="sub_count"
            label="存量"
            width="100"
            align="center"
          >
            <!-- <template slot-scope="scope">{{ scope.row.store }}</template> -->
          </el-table-column>
          <el-table-column
            prop="status"
            label="状态"
            width="200"
            align="center"
          >
          </el-table-column>
          <el-table-column
            prop="created_time"
            label="创建时间"
            width="180"
            align="center"
          >
            <!-- <template slot-scope="scope">
              <i class="el-icon-time"></i>
              <span style="margin-left: 10px">{{ scope.row.date }}</span>
            </template> -->
          </el-table-column>
          <el-table-column label="操作" width="400" align="center">
            <template slot-scope="scope">
              <el-button
                size="mini"
                type="primary"
                @click="handleEdit(scope.$index, scope.row)"
                >编辑</el-button
              >
              <el-button
                size="mini"
                type="primary"
                @click="handleEdit(scope.$index, scope.row)"
                >编辑</el-button
              >

              <el-button
                size="mini"
                type="danger"
                @click="handleDelete(scope.$index, scope.row)"
                >删除</el-button
              >
            </template>
          </el-table-column>
        </el-table>
        <!-- 分类列表的分页区域 -->
        <div class="pagination-container">
          <el-pagination
            @size-change="handleSizeChange"
            @current-change="handleCurrentChange"
            :current-page="listQuery.pageNum"
            :page-sizes="[5, 10, 15]"
            :page-size="listQuery.pageSize"
            layout="total, sizes, prev, pager, next, jumper"
            :total="total"
          >
          </el-pagination>
        </div>
      </el-col>
    </el-row>
  </div>
</template>
<script>
import { fetchList } from "../../api/media";

let defautlQuery = {
  pageNum: 1, // 默认显示第几页面
  pageSize: 5, // 每页显示多少个
  publishStatus: null, // 上架状态
  username: null,
};
export default {
  data() {
    return {
      listQuery: Object.assign({}, defautlQuery),
      publishStatusOptions: [
        { value: 1, label: "上架" },
        { value: 0, label: "下架" },
      ],
      tableData: [],
      total: 0,
      listLoading: false,
      options: [
        {
          value: "选项1",
          label: "已上架",
        },
        {
          value: "选项2",
          label: "已下架",
          disabled: true,
        },
      ],

      currentPage1: 5,
      currentPage2: 5,
      currentPage3: 5,
      currentPage4: 4,
    };
  },
  created() {
    this.getfetchList();
  },
  methods: {
    //获取列表数据
    getfetchList() {
      this.listLoading = true;
      fetchList(this.listQuery).then((response) => {
        console.log(response);
        if (response.code === 20000) {
          this.listLoading = false;
          this.tableData = response.data.items;
          this.total = response.data.total;
        }
      });
    },

    // 搜索
    handleSearchList() {
      this.listQuery.pageNum = 1;
      this.getfetchList();
    },
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`);
      this.listQuery.pageNum = 1;
      this.listQuery.pageSize = val;
      this.getfetchList(); // 重新加载数据
    },
    handleCurrentChange(val) {
      console.log(`当前页: ${val}`);
      this.listQuery.pageNum = val;
      this.getfetchList(); // 重新加载数据
    },
    handleEdit(index, row) {
      console.log(index, row);
    },
    handleDelete(index, row) {
      console.log(index, row);
    },
  },
};
</script>
<style scoped>
.head {
  width: 100%;
  height: 100px;
  line-height: 100px;
}
</style>
