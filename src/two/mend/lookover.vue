<template>
  <div class="look" id="lookHei">
    <div class="back" @click="show">
        <img src="../../assets/img/back.png">
    </div>
    <!-- 查看信息列表 -->
    <ul class="list" v-for="(item,index) in lookList" :key="index">
      <li>
        <label for="userName">客户名称：</label>
        <input type="text" id="userName" :value="item.owner">
      </li>
      <li>
        <label for="carNum">车牌号：</label>
        <input type="text" id="carNum" :value="item.license">
      </li>
      <li>
        <label for="phoneNum">手机号：</label>
        <input type="text" id="phoneNum" :value="item.phone">
      </li>
      <li>
        <label for="cardNum">证件号：</label>
        <input type="text" id="cardNum" :value="item.identity">
      </li>
      <li>
        <label >下单时间：</label>
        <input type="text" :value="item.orderstimes"> ——— <input type="text" :value="item.createtime">
      </li>
      <li>
        <label for="payment">支付金额：</label>
        <input type="text" id="payment" :value="item.indentmoney">
      </li>
      <li>
        <label for="orderState">订单状况：</label>
        <input type="text" id="orderState" :value="item.orderStatue">
      </li>
      <li>
        <label for="shop">门店：</label>
        <input type="text" id="shop" :value="item.returnStore">
      </li>
    </ul>
    <!-- 正在加载…… -->
    <div class="load" v-show="accShow">
      <i class="el-icon-loading"></i>正在加载中……
    </div>
  </div>
</template>

<script>

  export default {
    data() {
      return {
        lookList:[],
        // 数据加载慢时 显示正在加载
        accShow:true
      }
    },
    methods: {
      // 控制显示隐藏
        show(){
            this.$store.commit("isshow");
        }
    },
    mounted(){
      // 改变ul的高度 
      console.log("宽："+document.body.clientWidth);
      if(document.body.clientWidth<=1366){
        document.getElementsByClassName("look")[0].style.height = '483px';
        }else{
        document.getElementsByClassName("look")[0].style.height = '690px';
      }

       // 请求数据
      this.$axios.post('http://wlz.in.8866.org:30167/sumfind/getsumfind',this.$qs.stringify({
        license:this.$store.state.carIndex
      })).then((res)=>{
        // 请求数据  显示 正在加载
        this.accShow = false;
        this.lookList.push(res.data.sumfind[0]);
      }).catch((err)=>{
        // 请求数据  显示 正在加载
        this.accShow = true;
        throw err;
      });
    }
  }

</script>

<style scoped lang="less">
.look{
    height: 100%;
    background: #fff;
    border-radius: 13px;
    font-size: 14px;
    color: #333;
    position: relative;
    .back{
        width: 100%;
        height: 70px;
        display: flex;
        justify-content: flex-end;
        align-items: center;
        padding: 0 33px;
    }
    a{
        display: block;
    }
    .list{
        width: 37%;
        margin: 0 auto 120px auto;
        li{
            margin-bottom: 20px;
            &:nth-child(5) input{
                width: 150px;
                height: 40px;
            }
        }
        input{
            width: 251px;
            height: 40px;
            border-radius: 5px;
            border: solid 1px #d2d2d2;
            text-indent: 14px;
            &:focus{
                border: 1px solid #00a0e9;
                border-shadow: 0 0 3px #00a0e9;
            }
        }
        label{
            display: inline-block;
            width: 80px;
            text-align: right;
            margin-right: 13px;
        }
    }
}
.load{
   font-size: 18px;
   color: #888;
   height: 60px;
   display: flex;
   align-items: center;
   justify-content: center;

   position: absolute;
   top: 90px;
   left: 50%;
   transform: translateX(-50%);
   i{
     margin-right: 3px;
   }
}
@media all and (max-width: 1366px) {
  .look{
    border-radius: 9px;
    font-size: 10px;
    -webkit-transform-origin-x: 0;
    .back{
        height: 49px;
        padding: 0 23px;
    }
    .list{
        width: 37%;
        margin: 0 auto 84px auto;
        li{
            margin-bottom: 14px;
            &:nth-child(5) input{
                width: 105px;
                height: 28px;
            }
        }
        input{
            width: 176px;
            height: 28px;
            border-radius: 3px;
            text-indent: 10px;
            &:focus{
                border-shadow: 0 0 2px #00a0e9;
            }
        }
        label{
            width: 60px;
            margin-right: 9px;
        }
    }
  }
}
</style>
