<template>
  <div class="pos">
    <el-row>
      <el-col :span="7" class="pos-order" id="pos-order">
        <el-tabs>
          <el-tab-pane label="点餐">
            <el-table :data="tableData" stripe border style="width: 100%">
              <el-table-column
                prop="goodsName"
                label="商品名称"
              ></el-table-column>
              <el-table-column
                prop="count"
                label="数量"
                width="50"
              ></el-table-column>
              <el-table-column
                prop="price"
                label="金额"
                width="70"
              ></el-table-column>
              <el-table-column label="操作" width="100" fixed="right">
                <template slot-scope="scope">
                  <el-button
                    type="text"
                    size="small"
                    @click="deleteGoods(scope.row)"
                    >删除</el-button
                  >
                  <el-button
                    type="text"
                    size="small"
                    @click="addGoods(scope.row)"
                    >增加</el-button
                  >
                </template>
              </el-table-column>
            </el-table>
            <div class="totalDiv">
              <small>数量：</small> {{ totalCount }} &nbsp;&nbsp;&nbsp;&nbsp;
              <small>金额：</small> {{ totalMoney }} 元
            </div>
            <div class="btn-box">
              <el-button type="warning">挂单</el-button>
              <el-button type="danger" @click="deleteAllGoods">删除</el-button>
              <el-button type="success" @click="mockBill">结账</el-button>
            </div>
          </el-tab-pane>
          <el-tab-pane label="挂单">挂单</el-tab-pane>
          <el-tab-pane label="外卖">外卖</el-tab-pane>
        </el-tabs>
      </el-col>
      <el-col :span="17">
        <div class="often-used">
          <div class="title">常用商品</div>
          <div class="often-goods-list">
            <ul>
              <li
                v-for="item in oftenGoods"
                :key="item.goodsId"
                @click="addGoods(item)"
              >
                <span>{{ item.goodsName }}</span>
                <span class="price">￥ {{ item.price }}元</span>
              </li>
            </ul>
          </div>
        </div>
        <div class="type-goods-list">
          <el-tabs>
            <el-tab-pane label="汉堡">
              <div class="cook-list">
                <ul>
                  <li
                    v-for="item in typeOfHambur"
                    :key="item.id"
                    @click="addGoods(item)"
                  >
                    <span class="food-image"
                      ><img :src="item.goodsImg" width="100%"
                    /></span>
                    <span class="food-name">{{ item.goodsName }}</span>
                    <span class="food-price">￥ {{ item.price }}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="小食">
              <div class="cook-list">
                <ul>
                  <li
                    v-for="item in typeOfSnacks"
                    :key="item.id"
                    @click="addGoods(item)"
                  >
                    <span class="food-image"
                      ><img :src="item.goodsImg" width="100%"
                    /></span>
                    <span class="food-name">{{ item.goodsName }}</span>
                    <span class="food-price">￥ {{ item.price }}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="饮料">
              <div class="cook-list">
                <ul>
                  <li
                    v-for="item in typeOfDrink"
                    :key="item.id"
                    @click="addGoods(item)"
                  >
                    <span class="food-image"
                      ><img :src="item.goodsImg" width="100%"
                    /></span>
                    <span class="food-name">{{ item.goodsName }}</span>
                    <span class="food-price">￥ {{ item.price }}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="套餐">
              <div class="cook-list">
                <ul>
                  <li
                    v-for="item in typeOfMeals"
                    :key="item.id"
                    @click="addGoods(item)"
                  >
                    <span class="food-image"
                      ><img :src="item.goodsImg" width="100%"
                    /></span>
                    <span class="food-name">{{ item.goodsName }}</span>
                    <span class="food-price">￥ {{ item.price }}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
          </el-tabs>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "pos",
  data() {
    return {
      // 商品列表
      tableData: [],
      // 常用商品
      oftenGoods: [],
      // 分类商品
      typeOfHambur: [],
      typeOfSnacks: [],
      typeOfDrink: [],
      typeOfMeals: [],
      totalMoney: 0,
      totalCount: 0,
    };
  },
  created() {
    // 调用获取常用商品列表方法
    this.getOftenGoods();
    // 调用获取分类商品列表方法
    this.getTypeGoods();
  },
  mounted() {
    // 获得屏幕高度
    let orderHeight = document.body.clientHeight;
    // 设置pos-order 高度占据整个屏幕
    document.getElementById("pos-order").style.height = orderHeight + "px";
  },
  methods: {
    // 获取常用商品列表
    getOftenGoods() {
      this.$axios
        .get("/oftenGoods")
        .then((res) => {
          this.oftenGoods = res.data.data;
        })
        .catch((err) => {
          console.log(err);
        });
    },
    // 获取分类商品数据
    getTypeGoods() {
      this.$axios
        .get("/typeGoods")
        .then((res) => {
          this.typeOfHambur = res.data.data[0];
          this.typeOfSnacks = res.data.data[1];
          this.typeOfDrink = res.data.data[2];
          this.typeOfMeals = res.data.data[3];
        })
        .catch((err) => {
          console.log(err);
        });
    },
    // 添加商品
    addGoods(goods) {
      this.totalMoney = 0;
      this.totalCount = 0;
      let ifHave = false;
      for (let i = 0; i < this.tableData.length; i++) {
        if (this.tableData[i].goodsId == goods.goodsId) {
          ifHave = true;
        }
      }
      if (ifHave) {
        let arr = this.tableData.filter((o) => o.goodsId == goods.goodsId);
        arr[0].count++;
      } else {
        this.tableData.push({
          goodsId: goods.goodsId,
          goodsName: goods.goodsName,
          price: goods.price,
          count: 1,
        });
      }
      this.getAllCountAMoney();
    },
    // 删除商品
    deleteGoods(goods) {
      this.tableData.forEach((e) => {
        if (e.goodsId == goods.goodsId && e.count == 1) {
          this.tableData = this.tableData.filter(
            (o) => o.goodsId != goods.goodsId
          );
        } else if (e.goodsId == goods.goodsId && e.count > 1) {
          e.count--;
        }
      });
      this.getAllCountAMoney();
    },
    // 获得总数量和总金额
    getAllCountAMoney() {
      this.totalMoney = 0;
      this.totalCount = 0;
      this.tableData.forEach((e) => {
        this.totalCount += e.count;
        this.totalMoney += e.count * e.price;
      });
    },
    // 删除全部商品
    deleteAllGoods() {
      this.tableData = [];
      this.totalCount = 0;
      this.totalMoney = 0;
    },
    // 模拟结账
    mockBill() {
      if (this.totalCount != 0) {
        this.tableData = [];
        this.totalCount = 0;
        this.totalMoney = 0;
        this.$message({
          message: "结账成功",
          type: "success",
        })
      }else {
        this.$message({
          message: '请选择商品',
          type: 'error'
        })
      }
    },
  },
};
</script>

<style scoped>
.pos-order {
  background-color: #f0fcf2;
  border-right: 1px solid rgb(160, 160, 233);
}
.totalDiv {
  height: 50px;
  line-height: 50px;
  background-color: #f0fcf2;
  border-bottom: 1px solid rgb(160, 160, 233);
}
.btn-box {
  margin-top: 10px;
}
.title {
  height: 38px;
  line-height: 38px;
  background-color: rgb(228, 236, 247);
  text-align: left;
  border-bottom: 1px solid rgb(95, 94, 94);
}
.often-goods-list ul li {
  list-style: none;
  float: left;
  padding: 10px;
  margin: 10px;
  background-color: #fff;
  border: 1px solid rgb(75, 75, 214);
  cursor: pointer;
}
.price {
  color: rgb(96, 96, 214);
}
.type-goods-list {
  clear: both;
}
.cook-list ul li {
  list-style: none;
  float: left;
  border: 1px solid rgb(107, 107, 218);
  padding: 10px;
  margin: 2px;
  width: 23%;
  background-color: #fff;
  cursor: pointer;
}
.cook-list ul li span {
  display: block;
  float: left;
}
.food-image {
  float: left;
  width: 40%;
  margin-right: 10px;
  height: 60px;
}
.food-image > img {
  width: 100%;
  height: 100%;
}
.food-name {
  font-size: 18px;
  padding-left: 10px;
  color: rgb(28, 20, 148);
  margin-top: 5px;
  margin-bottom: 2px;
}
.food-price {
  font-size: 16px;
  padding-left: 10px;
}
</style>
