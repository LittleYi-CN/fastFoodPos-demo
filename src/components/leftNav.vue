<template>
  <div class="left-nav">
    <ul>
      <li>
        <i class="icon iconfont icon-hanbao"></i>
        <div>商品</div>
      </li>
      <li>
        <i class="icon iconfont icon-jinrudianpu"></i>
        <div>店铺</div>
      </li>
      <li>
        <i class="icon iconfont icon-wodezichan"></i>
        <div>收银</div>
      </li>
      <li>
        <i class="icon iconfont icon-31huiyuanqia"></i>
        <div>会员</div>
      </li>
      <li>
        <i class="icon iconfont icon-chanpincanshu"></i>
        <div>统计</div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'leftNav'
}
</script>

<style>
  .left-nav {
    height: 100%;
    width: 5%;
    color: #fff;
    background-color: rgb(87, 87, 240);
    font-size: 10px;
    float: left;
  }

  .left-nav > ul {
    padding: 0;
    margin: 0;
  }

  .left-nav > ul > li {
    list-style: none;
    text-align: center;
    border-bottom: 1px solid rgb(92, 91, 91);
    padding: 10px;
  }
  .iconfont {
    font-size: 24px;
  }
</style>