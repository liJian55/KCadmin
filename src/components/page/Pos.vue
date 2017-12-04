<template>
  <div class="pos">
    <div>
        <el-row >
            <el-col :span='7' class="pos-order" id="order-list">
              <el-tabs>
               <el-tab-pane label="点餐" style="padding-left：500px">
                 <el-table :data="tabledata" border style="width:100%">
                  <el-table-column prop="goodsName" label="商品名称" width="90"></el-table-column>
                  <el-table-column prop="count" label="数量"></el-table-column>
                  <el-table-column prop="price" label="价格"></el-table-column>
                  <el-table-column  label="操作" fixed="right" width="100">
                    <template scope="scope">
                      <el-button type="text" size="small">删除</el-button>
                      <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
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
              <div class="totalDiv">
                <small>数量：</small>{{this.totalCount}} &nbsp;&nbsp;&nbsp;<small>金额：</small> {{this.totalMoney}}元
              </div>
               </el-tabs> 




            </el-col>
            <!--商品展示-->
            <el-col :span="17">
              <div class="often-goods">
              <div class="title">常用商品</div>
              <div class="often-goods-list">
                <ul>
                  <li v-for="goods in oftenGoods" @click="addOrderList(goods)">
                    <span>{{goods.goodsName}}</span>
                    <span class="o-price">￥{{goods.price}}元</span>
                  </li>
                </ul>
              </div>
              </div>
              <div class="goods-type">
                <el-tabs>
                  <el-tab-pane label="汉堡">
                    <div>
                      <ul class='cookList'>
                        <li v-for="goods in type0Goods" @click="addOrderList(goods)">
                            <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                            <span class="foodName">{{goods.goodsName}}</span>
                            <span class="foodPrice">￥{{goods.price}}元</span>
                        </li>
                      </ul>
                    </div>
                  </el-tab-pane>
                  <el-tab-pane label="小食">
                        <div>
                          <ul class='cookList'>
                            <li v-for="goods in type1Goods" @click="addOrderList(goods)">
                              <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                              <span class="foodName">{{goods.goodsName}}</span>
                              <span class="foodPrice">￥{{goods.price}}元</span>
                        </li>
                      </ul>
                    </div>
                  </el-tab-pane>
                  <el-tab-pane label="饮料">
                       <div>
                          <ul class='cookList'>
                            <li v-for="goods in type2Goods" @click="addOrderList(goods)">
                              <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                              <span class="foodName">{{goods.goodsName}}</span>
                              <span class="foodPrice">￥{{goods.price}}元</span>
                        </li>
                      </ul>
                    </div>
                  </el-tab-pane>
                  <el-tab-pane label="汉堡">
                       <div>
                          <ul class='cookList'>
                            <li v-for="goods in type3Goods" @click="addOrderList(goods)">
                              <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                              <span class="foodName">{{goods.goodsName}}</span>
                              <span class="foodPrice">￥{{goods.price}}元</span>
                        </li>
                      </ul>
                    </div>
                  </el-tab-pane>
                </el-tabs>
              </div>


            </el-col>
        </el-row>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Pos',
  data(){
    return{
      tabledata: [],
      oftenGoods:[],
      type0Goods:[],
      type1Goods:[],
      type2Goods:[],
      type3Goods:[],
      totalMoney:0,
      totalCount:0
    }
  },
  methods:{
    addOrderList(goods){
      this.totalMoney = 0;
      this.totalCount = 0;

      //商品是否已经存在于订单列表中
      let isHave = false;
      for(let i =0;i<this.tabledata.length;i++){
        if(this.tabledata[i].goodsId == goods.goodsId){
            isHave = true;
        }

      }
      //根据判断的值编写业务逻辑
        if(isHave==true){
          //改变列表中商品的数量
          let arr = this.tabledata.filter(o=>o.goodsId ==goods.goodsId);
          arr[0].count++;
        
        }else{
          let newGoods = {goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1};
          this.tabledata.push(newGoods)

        }
        //计算汇总价格和数量
        this.tabledata.forEach((element)=>{
          this.totalCount +=element.count;
          this.totalMoney =this.totalMoney+(element.price*element.count);

        })

    }
  }
  ,
  created:function(){
    axios.get('http://jspang.com/DemoApi/oftenGoods.php')
    .then(Response=>{
      // console.log(Response)
      this.oftenGoods = Response.data;
    })
    .catch(Error=>{
      // console.log(Error)
      alert('网络异常，请检查网络');
    })


     axios.get('http://jspang.com/DemoApi/typeGoods.php')
    .then(Response=>{
      // console.log(Response)
      this.type0Goods = Response.data[0];
      this.type1Goods = Response.data[1];
      this.type2Goods = Response.data[2];
      this.type3Goods = Response.data[3];
    })
    .catch(Error=>{
      console.log(Error)
      alert('网络异常，请检查网络');
    })
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
.title{
  height: 20px;
  border-bottom: 1px solid #d3dce6;
  background-color:#F9FAFC;
  padding: 10px;
  text-align: left;
}
.often-goods-list ul li{
  list-style: none;
  float: left;
  border:1px solid #E5e9f2;
  padding: 10px;
  margin: 10px;
  background-color: #fff;
  cursor: pointer;
}
.o-price{
  color: #58b7ff;
}
.goods-type{
  clear: both;
}
.cookList li{
       list-style: none;
       width:23%;
       border:1px solid #E5E9F2;
       height: auot;
       overflow: hidden;
       background-color:#fff;
       padding: 2px;
       float:left;
       margin: 2px;
       cursor: pointer;
 
   }
   .cookList li span{
       
        display: block;
        float:left;
   }
   .foodImg{
       width: 40%;
   }
   .foodName{
       font-size: 18px;
       padding-left: 10px;
       color:brown;
 
   }
   .foodPrice{
       font-size: 16px;
       padding-left: 10px;
       padding-top:10px;
   }
   .totalDiv{
     padding: 10px;
     background-color: #fff;
     border-bottom: 1px solid #d3dce6;
   }
</style>