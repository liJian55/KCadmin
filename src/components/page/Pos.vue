<template>
  <div class="pos">
    <div>
        <el-row >
            <el-col :span='7' class="pos-order" id="order-list">
              <el-tabs>
               <el-tab-pane label="点餐">
                 <el-table :data="tabledata" border style="width:100%">
                  <el-table-column prop="goodsName" label="商品名称" width="90"></el-table-column>
                  <el-table-column prop="count" label="数量"></el-table-column>
                  <el-table-column prop="price" label="价格"></el-table-column>
                  <el-table-column  label="操作" fixed="right" width="100">
                    <template scope="scope">
                      <el-button type="text" size="small">删除</el-button>
                      <el-button type="text" size="small">增加</el-button>
                    </template>
                  </el-table-column>
                          </el-table>
                          <div class="div-btn">
                            <el-button type="warning">挂单</el-button>
                            <el-button type="danger">删除</el-button>
                            <el-button type="success">结账</el-button>
                          </div>
                </el-tab-pane>
                <el-tab-pane label="挂单">
                  挂单
                </el-tab-pane>
                <el-tab-pane label="外卖">
                  挂单
                </el-tab-pane>
            
               </el-tabs> 




            </el-col>
            <!--商品展示-->
            <el-col :span="17">
             我是产品栏
            </el-col>
        </el-row>
    </div>
  </div>
</template>

<script>

export default {
  name: 'Pos',
  data(){
    return{
      tabledata: [{
          
          goodsName: '可口可乐',
          price: 8,
          count:1
        }, {
          
          goodsName: '香辣鸡腿堡',
          price: 15,
          count:1
        }, {
         
          goodsName: '爱心薯条',
          price: 8,
          count:1
        }, {
         
          goodsName: '甜筒',
          price: 8,
          count:1
        }]
    }
  },
  mounted:function(){
    var orderHeight = document.body.clientHeight;
    console.log(orderHeight);
    document.getElementById('order-list').style.height = orderHeight+'px';
  }
}
</script>
<style scoped>
.pos-order{
  background-color: #f9fafc;
  border-right: 1px solid #C0CCDA
}
.div-btn{
  padding-top: 10px;
  
}
</style>