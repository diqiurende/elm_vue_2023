<script setup>
import {onMounted, ref} from "vue";
import {getSessionStorage} from "@/common.js";
import axios from "axios";
import Footer from "@/components/Footer.vue";
import Header from "@/components/Header.vue";
const orderArr = ref([])
const user = ref({})

onMounted(()=>{
  // user.value = getSessionStorage('user')
  user.value.userId = 555;
  axios.get("OrdersController/listOrdersByUserId",{params:{userId:user.value.userId}}).then(response=>{
    let orders = response.data
    console.log('Orders:', orders);
    for(let order of orders){
      order.isShowDetaillet = false;
    }
    orderArr.value = orders;
  }).catch(error=>{
    console.log(error)
  })
})
</script>

<template>
<!--  <div>{{orderArr}}</div>-->


    <!--header部分-->
    <header class="w-full h-12 bg-[#0097FFFF] text-white text-2xl fixed top-0  flex justify-center orders-center">
      <p >我的订单</p>
    </header>


  <div class="w-full h-full">

    <div  style="margin-top: 15vw" v-for="(order, index) in orderArr" :key="order.orderId">
      <p>Order ID: {{ order.orderId }}</p>
      <p>Order State: {{ order.orderState }}</p>
      <p>Order Details: {{ order }}</p>
    </div>

    <!--订单列表部分-->
    <h3 style="margin-top: 10vw; box-sizing: border-box; padding: 4vw; font-size: 1rem; font-weight: 300; color: #999999FF;">未支付订单信息：</h3>
        <ul class="w-full">
      <li  v-for="(order, index) in orderArr" :key="order.orderId"  class="w-full  text-3xl">
        <div v-if="order.orderState===0">
          <div class="box-border py-[2vw] px-[4vw]  text-[#666666FF] flex justify-between orders-center">
            <p class="flex  justify-between orders-center">{{ order.business }}
              <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 32 32" @click="showDetail"><path fill="currentColor" d="m24 12l-8 10l-8-10z"/></svg></p>
            <div class="flex">
              <p class="">&#165;{{order.orderTotal}}</p>
              <div class="bg-[#FF9900FF] text-white rounded-[3px] ml-4 select-none cursor-pointer">去支付</div>
            </div>
          </div>
<!--          <ul class="w-full" v-show="order.isShowDetaillet">-->
<!--            <li class=" w-full box-border py-[1vw] px-[4vw] text-[#666666FF] text-xl  flex justify-between orders-center" v-for="(detail,key) in order.list">-->
<!--              <p class="">{{detail.food.foodName}} x {{detail.quantity}}</p>-->
<!--              <p class="">&#165;{{detail.food.foodPrice * detail.quantity}} </p>-->
<!--            </li>-->
<!--            <li class=" w-full box-border py-[1vw] px-[4vw] text-[#666666FF] text-xl  flex justify-between orders-center">-->
<!--              <p class="">配送费</p>-->
<!--              <p class="">&#165;{{order.business.deliveryPrice}}</p>-->
<!--            </li>-->

<!--          </ul>-->
        </div>
      </li>
    </ul>

    <h3 style="margin-top: 10vw; box-sizing: border-box; padding: 4vw; font-size: 1rem; font-weight: 300; color: #999999FF;">已支付订单信息：</h3>
<!--    <ul class="w-full">-->
<!--      <li class="w-full text-3xl" v-for="order in orderArr" v-if="order.orderState===1">-->
<!--        <div class="box-border py-[2vw] px-[4vw]  text-[#666666FF] flex justify-between orders-center">-->
<!--          <p class="flex flex-row justify-between orders-center">-->
<!--            {{order.business.businessName}}-->
<!--            <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 32 32" @click="showdetail"><path fill="currentColor" d="m24 12l-8 10l-8-10z"/></svg>-->
<!--          </p>-->
<!--          <div class="flex">-->
<!--            <p class="">&#165;{{order.orderTotal}}</p>-->
<!--            <div class="bg-[#FF9900FF] text-white rounded-[3px] ml-4 select-none cursor-pointer">去支付</div>-->
<!--          </div>-->
<!--        </div>-->
<!--        <ul class="w-full" v-show="order.isShowDetaillet">-->
<!--          <li class="w-full box-border py-[1vw] px-[4vw] text-[#666666FF] text-xl flex justify-between orders-center" v-for="(detail,key) in order.list">-->
<!--            <p class="">{{detail.food.foodName}} x {{detail.quantity}}</p>-->
<!--            <p class="">&#165;{{detail.food.foodPrice * detail.quantity}}</p>-->
<!--          </li>-->
<!--          <li class="w-full box-border py-[1vw] px-[4vw] text-[#666666FF] text-xl flex justify-between orders-center">-->
<!--            <p class="">配送费</p>-->
<!--            <p class="">&#165;{{order.business.deliveryPrice}}</p>-->
<!--          </li>-->
<!--        </ul>-->
<!--      </li>-->
<!--    </ul>-->

    <!--底部菜单部分-->
    <Footer></Footer>
  </div>
</template>

<style scoped>

</style>
