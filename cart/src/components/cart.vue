<template>
  <div id="cart">
    <div class="title">&nbsp;点餐平台</div>
    <div class="little_title">
      <p style="font-size: 20px;padding-bottom: 1px"><</p>
      <p @click="removeDis()">编辑</p>
      <span>购物车</span>
    </div>

    <div class="container" v-for="(item,index) in list">
      <div class="content">
        <div class="rant">
          <p class="rant_name">{{item.rant_name}}</p>
          <input type="checkbox" name="item.name" id="item.id" :checked="item.checkAll" @click="check(index)"/>全选
        </div>
        <div class="rant_detail">
          <div class="d_item" v-for="(d,i) in item.rant_detail">
            <p><input type="checkbox" :checked="d.checked" @click="single_check(index,i)"/></p>
            <p><img :src="d.img" alt=""></p>
            <p>
              <span>{{d.name}}</span>
              <span>￥{{d.price}}</span>
            </p>
            <p>
              <span @click="reduce(index,i)">-</span><span>{{d.count}}</span><span @click="add(index,i)">+</span>
            </p>
            <p v-if="removeBtn" @click="remove(index,i)">
              删除
            </p>
          </div>
        </div>
      </div>


    </div>

    <div class="total">
      <p><input type="checkbox"  :checked="total_check"  @click="totalCheck()"/>&nbsp;&nbsp;全选</p>
      <p>合计:￥<span>{{totalMon}}</span></p>
      <p class="js">结算</p>
    </div>

  </div>
</template>

<script>
    export default {
      data(){
        return {
          list:[
            {
              id: 1000,
              checkAll: true,
              rant_name: "餐厅1",
              rant_detail:[
                {
                  id: 11,
                  checked: true,
                  img:"./static/images/01.png",
                  name:"商品1-1",
                  price:30,
                  count: 1
                },
                {
                  id: 12,
                  checked: true,
                  img:"./static/images/02.png",
                  name:"商品1-2",
                  price:30,
                  count: 1
                }
              ],
            },
            {
              id:2000,
              checkAll: true,
              rant_name: "餐厅2",
              rant_detail:[
                {
                  id:22,
                  checked: true,
                  img:"./static/images/03.png",
                  name:"商品2-1",
                  price:30,
                  count: 1
                }
              ]
            }
          ],
          total_check:true,
          removeBtn: false
        }
      },
      methods:{
       // 每个餐厅全选
        check: function(index){
            for(var i = 0; i<this.list[index].rant_detail.length;i++){
              this.list[index].rant_detail[i].checked = !this.list[index].checkAll;
            }
            if(this.list[index].checkAll){
              this.list[index].checkAll = false;
            }else{
              this.list[index].checkAll = true;
            }
        },

       // 单选
        single_check: function (index,i) {
          this.list[index].rant_detail[i].checked = !this.list[index].rant_detail[i].checked;
          if(!this.list[index].rant_detail[i].checked){
            this.list[index].checkAll = false;
            this.total_check = false;
          }else{
            this.list[index].checkAll = true;
            this.total_check = true;
//            for(var a=0;a<this.list.length;i++){
//              for(var b=0;b<this.list[a].rant_detail.length;b++){
//                if(!this.list[a].checkAll && !this.list[a.rant_detail[b].checked]){
//                  this.total_check = false;
//                }else{
//                  this.total_check = true;
//                }
//              }
//            }
          }
        },
       // 增加数量
        add: function (index,i) {
            this.list[index].rant_detail[i].count++;
//          this.totalMon();

        },
       // 减少数量
        reduce: function(index,i){
          if(this.list[index].rant_detail[i].count <= 1){
            return;
          }else{
            this.list[index].rant_detail[i].count--;
          }
        },

       // 订单全选
       totalCheck: function () {
          var _this = this;
        // 订单全选及取消订单全选
         if(_this.total_check){
           _this.total_check = false;
           for(var i = 0;i<_this.list.length;i++){
             _this.list[i].checkAll = false;
             for(var j = 0;j < _this.list[i].rant_detail.length ;j++){
                 _this.list[i].rant_detail[j].checked = false;
             }
           }
         }else{
           _this.total_check = true;
           for(var i = 0;i<_this.list.length;i++){
             _this.list[i].checkAll = true;
             for(var j = 0;j < _this.list[i].rant_detail.length ;j ++){
               _this.list[i].rant_detail[j].checked = true;
             }
           }
         }
       },
       //删除按钮展示
        removeDis: function(){
          this.removeBtn = !this.removeBtn;
        },
      //删除某一条
        remove: function(index,i){
          this.list[index].rant_detail.splice(i,1);
          console.log(this.list[index].rant_detail.length)
          if(this.list[index].rant_detail.length == 0){
//            this.list[index].splice(index,1);
          }
        }
      },
      mounted: function () {
      },
      computed: {
        totalMon: function () {
          var t = 0;
          var me = this;
          for(var i = 0; i < me.list.length; i++) {
            for(var j = 0;j < me.list[i].rant_detail.length;j++){
              if(me.list[i].rant_detail[j].checked){
                t += me.list[i].rant_detail[j].price * me.list[i].rant_detail[j].count;
              }
            }
          }
          return t;
        }
      }
    }
</script>

<style scoped>
  .title{
    height: 100px;
    line-height: 100px;
    padding: 0 3%;
    font-size: 36px;
    text-align: left;
    letter-spacing: 2px;
    color: #fff;
    background-color: #4F77AA;
  }
  .little_title{
    position: relative;
    top: 0;
    left: 0;
    display: flex;
    justify-content: space-between;
    padding: 0 3%;
    color: #737373;
    height: 100px;
    line-height:100px;
  }
  .little_title p{
    font-size: 30px;
    color: #737373;
  }
  .little_title span{
    position: absolute;
    height:100%;
    line-height:100px;
    width: 40%;
    left: 30%;
    font-size: 32px;
  }
  .container{
    padding:0 1%;
  }

  .rant{
    display: flex;
    align-items: center;
    padding: 0 3%;
    height: 80px;
    line-height: 80px;
    border-top: 1px solid #F2F2F2;
    border-bottom: 1px solid #F2F2F2;
  }
  .rant p{
    font-size: 30px;
    margin-right: 6%;
  }
  .d_item{
    display: flex;
    align-items: center;
    padding: 0 3%;
    height: 240px;
  }
  .d_item p:nth-child(1){
    width: 10%;
    text-align: left;
  }
  .d_item p:nth-child(2){
    width: 30%;
    height: 80%;
  }
  .d_item p:nth-child(2) img{
    width: 100%;
    height: 100%;
  }
  .d_item p:nth-child(3){
    display: flex;
    flex-flow: column;
    justify-content: space-between;
    text-align: left;
    width: 36%;
    height: 80%;
    margin-left: 2%;
  }
  .d_item p:nth-child(4){
    width: 30%;
  }
  .d_item p:nth-child(4) span{
    border: 1px solid #f2f2f2;
    padding: 5px 14px;
    color: #333;
  }
  .d_item p:nth-child(4) span:nth-child(1){
   margin-left: 20%;
  }
  .d_item p:nth-child(4) span:nth-child(2){
    padding: 5px 30px;
  }
  .d_item p:nth-child(5){
    color:#FD0200;
    padding: 5px ;
    white-space: nowrap;
  }
  .total{
    position: fixed;
    bottom:0;
    left:0;
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 100%;
    height: 100px;
    padding-left: 3%;
    line-height:100px;
  }
  .total span{
    color: #FD0200;
  }
  .total .js{
    color: #fff;
    width:30%;
    text-align: center;
    font-size: 34px;
    letter-spacing: 4px;
    background-color: #FD0200;
  }

</style>
