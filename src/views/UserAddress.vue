<template>
  <div class="wrapper">
    <!-- header部分 -->
    <header>
      <p>地址管理</p>
    </header>

    <!-- 地址列表部分 -->
    <ul class="addresslist">
      <li v-for="item in deliveryAddressArr">
        <div class="addresslist-left" @click="setDeliveryAddress(item)">
          <h3>{{item.contactName}} {{getsex(item.contactSex)}} {{item.contactTel}}</h3>
          <p>{{item.address}}</p>
        </div>
        <div class="addresslist-right">
          <i class="fa fa-edit" @click="editUserAddress(item.daId)"></i>
          <i class="fa fa-remove" @click="removeUserAddress(item.daId)"></i>
        </div>
      </li>
    </ul>

    <!-- 新增地址部分 -->
    <div class="addbtn" @click="toAddUserAddress">
      <i class="fa fa-plus-circle"></i>
      <p>新增收货地址</p>
    </div>

    <!-- 底部菜单部分 -->
    <Footer></Footer>
  </div>
</template>

<script setup>
import Footer from '../components/Footer.vue';
import {onMounted, ref} from "vue";
import {useRoute, useRouter} from "vue-router";
import {getLocalStorage, getSessionStorage, removeLocalStorage, setLocalStorage} from "@/common.js";
import axios from "axios";
import qs from "qs";
const route = useRoute()
const router = useRouter()
const businessId = ref(route.query.businessId)
const user =ref({})
const deliveryAddressArr = ref([])


function listDeliveryAddressByUserId() {
  //查询送货地址
  axios.get('DeliveryAddressController/listDeliveryAddressByUserId',{params:{
    userId: user.value.userId}}
  ).then(response => {
    deliveryAddressArr.value = response.data;
  }).catch(error => {
    console.error(error);
  });
}
onMounted(()=> {
    user.value = getSessionStorage('user');
    listDeliveryAddressByUserId();
  })

function getsex(sex) {
  if (sex === 1) {
    return "先生";
  } else if (sex === 0) {
    return "女士"
  } else {return " "}
}
  function setDeliveryAddress(deliveryAddress){
      //把用户选择的默认送货地址存储到localStorage中
      setLocalStorage(user.value.userId,deliveryAddress);
      router.push({path:'/tooder',query:{businessId:businessId.value}});
    }
    function toAddUserAddress(){
      router.push({path:'/addUserAddress',query:{businessId:businessId.value}});
    }
    function editUserAddress(daId){
      router.push({path:'/editUserAddress',query:{businessId:businessId.value,daId:daId}});
    }
    function removeUserAddress(daId){
      if(!confirm('确认要删除此送货地址吗？')){
        return;
      }

      axios.get('DeliveryAddressController/removeDeliveryAddress',{params:{
        daId:daId}
      }).then(response=>{
        if(response.data>0){
          let deliveryAddress = getLocalStorage(user.value.userId);
          if(deliveryAddress!=null&&deliveryAddress.daId===daId){
            removeLocalStorage(user.value.userId);
          }
          this.listDeliveryAddressByUserId();
        }else{
          alert('删除地址失败！');
        }
      }).catch(error=>{
        console.error(error);
      });
}
</script>

<style scoped>
/*************** 总容器 ***************/
.wrapper {
  width: 100%;
  height: 100%;
  background-color: #F5F5F5;
}

/*************** header ***************/
.wrapper header {
  width: 100%;
  height: 12vw;
  background-color: #0097FF;
  display: flex;
  justify-content: space-around;
  align-items: center;
  color: #fff;
  font-size: 4.8vw;
  position: fixed;
  left: 0;
  top: 0;
  /*保证在最上层*/
  z-index: 1000;
}

/*************** addresslist ***************/
.wrapper .addresslist {
  width: 100%;
  margin-top: 12vw;
  background-color: #fff;
}

.wrapper .addresslist li {
  width: 100%;
  box-sizing: border-box;
  border-bottom: solid 1px #DDD;
  padding: 3vw;

  display: flex;
}

.wrapper .addresslist li .addresslist-left {
  flex: 5;
  /*左边这块区域是可以点击的*/
  user-select: none;
  cursor: pointer;
}

.wrapper .addresslist li .addresslist-left h3 {
  font-size: 4.6vw;
  font-weight: 300;
  color: #666;
}

.wrapper .addresslist li .addresslist-left p {
  font-size: 4vw;
  color: #666;
}

.wrapper .addresslist li .addresslist-right {
  flex: 1;
  font-size: 5.6vw;
  color: #999;
  cursor: pointer;

  display: flex;
  justify-content: space-around;
  align-items: center;
}

/*************** 新增地址部分 ***************/
.wrapper .addbtn {
  width: 100%;
  height: 14vw;
  border-top: solid 1px #DDD;
  border-bottom: solid 1px #DDD;
  background-color: #fff;
  margin-top: 4vw;

  display: flex;
  justify-content: center;
  align-items: center;

  font-size: 4.5vw;
  color: #0097FF;
  user-select: none;
  cursor: pointer;
}

.wrapper .addbtn p {
  margin-left: 2vw;
}
</style>
