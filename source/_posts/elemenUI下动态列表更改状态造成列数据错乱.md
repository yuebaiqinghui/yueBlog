---
date: 2019/12/6 16:20:44
categories:
- Vue
tags:
- Vue
- elementUI
---
## 问题
需要显示已审核和待审核两个列表，两个列表大多数列是相同的，少数列并不相同，用v-if控制列的显示，但切换状态显示不同列表时，列显示错乱（顺序改变，内容对不上等）

## 解决方法
给所有列头加上**key**
`key="1"`这种
```html
<el-table 
:data="list" 
v-loading="listloading" 
fit
border
size="medium">
  <el-table-column key="1" label="序号" type="index" align="center"></el-table-column>
  <el-table-column key="2" label="订单编号" prop="orderId" align="center"></el-table-column>
  <el-table-column key="3" label="用户名称" prop="userName" align="center"></el-table-column>
  <el-table-column key="4" label="联系电话" prop="userPhone" align="center"></el-table-column>
  <el-table-column key="5" label="申请理由" prop="remark" align="center"></el-table-column>
  <el-table-column key="6" label="证明照片" align="center">
    <template slot-scope="scope">
      <el-image 
        style="width: 100px; height: 100px"
        :src="handleToString(scope.row.reworkPicUrl)" 
        :preview-src-list="handleToArray(scope.row.reworkPicUrl)">
      </el-image>
    </template>
  </el-table-column>
  <el-table-column key="7" label="上门时间" prop="serviceTime" align="center"></el-table-column>
  <el-table-column key="8" label="申请时间" prop="createdTime" align="center"></el-table-column>
  
  <template v-if="state===''">
    <el-table-column key="9" label="审核时间" prop="updatedTime" align="center"></el-table-column>
      <el-table-column key="10" label="申请结果" align="center">
        <template slot-scope="scope">
          <span v-if="scope.row.reworkState === 2">通过</span>
          <span v-if="scope.row.reworkState === 4">通过</span>
          <span v-if="scope.row.reworkState === 5">通过</span>
          <span v-if="scope.row.reworkState === 3">已拒绝</span>
        </template>
      </el-table-column>
      <el-table-column key="11" label="审核人" prop="auditorName" align="center"></el-table-column>
  </template>
  
  <template v-if="state===1">
    <el-table-column key="12" label="维修员" prop="workerName" align="center"></el-table-column>
    <el-table-column key="13" label="联系电话" prop="workerPhone" align="center"></el-table-column>
    <el-table-column key="14" label="操作" align="center" width="100vw">
      <template slot-scope="scope">
        <el-button v-if="scope.row.reworkState === 1" type="text" size="medium" @click="handleVerify(scope.row)">审核</el-button>
      </template>
    </el-table-column>
  </template>
  
</el-table>
```