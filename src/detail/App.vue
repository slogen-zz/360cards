<template>
  <div id="app">
    <div class="cardpic">
    	<div class="box">
    		<span v-if="serviceCardType === '1'"><img src="./card-1-left.png"></span>
    		<span v-if="serviceCardType === '1'"><img src="./card-1-right.png"></span>
    		<span v-if="serviceCardType === '2'"><img src="./card-2-left.png"></span>
    		<span v-if="serviceCardType === '2'"><img src="./card-2-right.png"></span>
    		<span v-if="serviceCardType === '3'"><img src="./card-3-left.png"></span>
    		<span v-if="serviceCardType === '3'"><img src="./card-3-right.png"></span>
    	</div>
    </div>
    <div class="cardadd">
    	<h2 class="title">服务地址</h2>
    	<div class="add">
    		<p><span>姓名：</span><span>{{addressInfo.userName}}</span></p>
    		<p><span>联系电话：</span><span>{{addressInfo.iphone}}</span></p>
    		<p class="add-add"><span>联系地址：</span><span>{{addressInfo.province + addressInfo.detailAddress}}</span></p>
    	</div>
    </div>
    <div class="cardrights">
    	<div class="rights-top">
    		<div class="carddetail" @click="goDiscrip">
    			<span>卡说明</span>
    			<img src="./arrow.png" alt="" />
    		</div>
    		<h2 class="title">保修卡权益</h2>
    	</div>
    	<cardright :rightsInfo = "rightsInfo"></cardright>
    </div>
    <div class="usage" @click="goUsage">
    	<span class="usage-left">使用情况</span>
    	<span class="usage-right"></span>
    </div>
  </div>
</template>

<script>
		import '../common/base-1.3.css'
		import publicMethod from '../common/publicMethod.js'
		import cardright from './cardright.vue'
    export default {
        name: 'detail',
        data () {
	        	let serviceCardNo = publicMethod.getQuery('serviceCardNo')
	        	let serviceCardType = publicMethod.getQuery('serviceCardType')
	        	let userId = publicMethod.getQuery('userId')
	        	return {
	        		serviceCardNo: serviceCardNo,
	        		serviceCardType: serviceCardType,
	        		userId: userId,
	        		addressInfo: {},
	        		rightsInfo:{}
	        	}
        },
        mounted () {
//      	{userId=1354999&serviceCardNo=e1711000051}
          this.$http.get(this.apiUrl.detail.detailServiceCard, {params: {userId: this.userId, serviceCardNo: this.serviceCardNo}}).then((response) => {
              response = response.body
              if (response.resultCode === 200) {
              	this.addressInfo = response.data.addressInfo
              	this.rightsInfo= response.data.rightsInfo
              } else {
              	alert (response.message)
              }
          })
          //地址添加
          publicMethod.setAddrData()
        },
        methods: {
        	goDiscrip (){
        		window.location.href = 'descrip.html?serviceCardNo='+this.serviceCardNo+'&'+'userId='+this.userId
        	},
        	goUsage(){
        		window.location.href = 'usage.html?serviceCardNo='+this.serviceCardNo+'&'+'userId='+this.userId
        	}
        },
        components: {
        	cardright
        }
    }
</script>

<style>
	body {
		font-family: "helvetica","microsoft yahei";
		background: #f3f3f3;
	}
	.title {
		font-size: 16px;
		padding: 10px 0;
		border-bottom: 1px solid #eeeeee;
	}
	.cardpic {
		  padding: 15px 1.2rem 25px;
		  background: #fff;
	}
	.cardpic .box {
		display: webkit-flex;
		display: flex;
		justify-content: space-between;
		font-size: 0;
	}
	.cardpic .box span {
		  display: inline-block;
		  width: 3.46rem;
		  height: 5.46rem;
		  border-radius: 5px;
		  overflow: hidden;
	}
	.cardpic .box span img {
		width: 100%;
		height: 100%;
	}
	.cardadd {
		padding: 0 0.3125rem;
		margin-top: 10px;
		background: #fff;
	}
	.cardadd .add {
		padding: 10px 0;
		font-size: 15px;
	}
	.cardadd .add p {
		display: webkit-flex;
		display: flex;
		justify-content: space-between;
		padding: 5px 0;
		color: #666;
	}
	.cardadd .add p.add-add span:nth-of-type(1) {
		width: 130px;
	}
	.cardadd .add p.add-add span:nth-of-type(2) {
		text-align: right;
	}
	.cardrights {
		/*padding: 0.3125rem;*/
		padding: 0 0.3125rem;
		margin-top: 10px;
		background: #fff;
	}
	.rights-top{
		overflow: hidden;
	}
	.carddetail{
		float: right;
		line-height: 50px;
		font-size: 0;
		vertical-align: middle;
	}
	.carddetail span{
		font-size: 15px;
		display: inline-block;
		margin-right: 5px;
		vertical-align: middle;
		color: #666666;
	}
	.carddetail img,.usage img{
		width: 10px;
		display: inline-block;
		vertical-align: middle;
	}
	.cardrights .intro h3 {
		padding: 15px 0;
		font-size: 15px;
	}
	.cardrights .intro .introcell {
		padding-bottom: 10px;
		border-bottom: 1px solid #eeeeee;
	}
	.cardrights .intro .introcell h4 {
		display: webkit-flex;
		display: flex;
		justify-content: space-between;
		padding-bottom: 10px;
		color: #666666;
		font-size: 14px;
	}
	.cardrights .intro .introcell h4 span:nth-of-type(2) {
		padding-right: 5px;
	}
	.cardrights .intro .introcell h4 span:nth-of-type(2) i {
		position: relative;
		left: 5px;
	}
	.cardrights .title,.cardused .title{
		padding:14px 0;
	}
	.triangle-up {
		top: -12px;
    width: 0;
    height: 0;
    border-left: 3px solid transparent;
    border-right: 3px solid transparent;
    border-bottom: 6px solid #666666;
	}
	.triangle-down {
		top: 12px;
    width: 0;
    height: 0;
    border-left: 3px solid transparent;
    border-right: 3px solid transparent;
    border-top: 6px solid #666666;
	}
	.show {
		display: block;
	}
	.hide {
		display: none;
	}
	.cardrights .intro .introcell ul {
		padding-bottom: 10px;
		padding-left: 10px;
		padding-right: 10px;
		/*border: 1px solid #eeeeee;*/
		margin-bottom: 10px;
		border-radius: 4px;
		background: #fafbfb;
	}
	.cardrights .intro .introcell li {
		padding-top: 10px;
		font-size: 14px;
		color: #acacac;
	}
	.cardused {
		padding: 0 0.3125rem;
		margin-top: 10px;
		background: #fff;
	}
	.cardused .usedcell {
		font-size: 15px;
		color: #666666;
	}
	.cardused .usedcell li {
		display: webkit-flex;
		display: flex;
		justify-content: space-between;
		padding-top: 12px;
	}
	.cardused .usedcell li:last-child {
		padding-bottom: 12px;
		border-bottom: 1px solid #eeeeee;
	}
	.cardused .usedcell li span.red {
		color: #d40711;
	}
	.usage{
		margin-top: 10px;
		background: #fff;
		height: 50px;
		padding: 0 0.3125rem;
		line-height: 50px;
		font-size: 16px;
		overflow: hidden;
	}
	.usage-left{
		float: left;
	}
	.usage-right{
		width: 10px;
		height: 50px;
		background: url(arrow.png) no-repeat center center;
		background-size: 10px 17px;
		float: right;
	}
</style>