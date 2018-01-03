<template>
	<div class="cardlist" 
		@click="goto"
		:class="{card:message.serviceCardType==1,goldcard:message.serviceCardType==2,placard:message.serviceCardType==3}"
		>
		<div class="iconimg">
			<img src="./img/360icon.png" alt="" class="cardicon" v-if="message.serviceCardType==1"/>
			<img src="./img/goldicon.png" alt="" class="cardicon" v-if="message.serviceCardType==2"/>
			<img src="./img/plaicon.png" alt="" class="cardicon" v-if="message.serviceCardType==3"/>
		</div>
		<div class="carddetail">
			<p class="cardtitle" v-cloak>{{message.serviceCardTypeStr}}</p>
			<p class="cardnum">
				<span>卡号：</span><span v-cloak>{{message.serviceCardNo}}</span>
			</p>
			<p>
				<span>服务有效截止：</span><span v-cloak>{{message.availableTimeStr}}</span>
			</p>
		</div>
		<div class="status" :class="{okstatus:message.serviceCardStatus==1,overtime:message.serviceCardStatus==2 || message.serviceCardStatus==4}" v-cloak><span>{{message.serviceCardStatusStr}}</span></div>
	</div>
</template>

<script>
	export default {
		props:['message','userId'],
	    data(){
	        return {
	        	
	        }
	    },
	    methods:{
	    	goto(){
        		window.location.href='detail.html?serviceCardNo='+this.message.serviceCardNo+'&'+'serviceCardType='+this.message.serviceCardType+'&'+'userId='+this.userId
        	}
	    },
	    mounted(){
	    	this.message.availableTimeStr = this.message.availableTimeStr.split('-').join('.')
	    }
	}
</script>

<style>
	.cardlist{
		width: 9.375rem;
		height:2.734375rem;
		border-radius: 5px;
		-webkit-border-radius: 5px;
		margin-bottom: 0.3125rem;
		font-size: 0;
		padding: 0.453125rem 0;
		box-sizing: border-box;
		position: relative;
		top: 0;
		left: 0;
		overflow: hidden;
	}
	.card{
		background: linear-gradient(to right, #252525 , #3f3f40); 
		background: -webkit-linear-gradient(left, #252525 , #3f3f40);
	}
	.goldcard{
		background: linear-gradient(to right, #cab16b , #d3c195); 
		background: -webkit-linear-gradient(left, #cab16b , #d3c195);
	}
	.placard{
		background: linear-gradient(to right, #979798 , #d2d2d5); 
		background: -webkit-linear-gradient(left, #979798 , #d2d2d5);
	}
	.cardlist>div{
		display: inline-block;
		vertical-align: middle;
	}
	.iconimg{
		margin-left:0.3125rem ;
	}
	.cardicon{
		width: 2.34375rem;
	}
	.carddetail{
		margin-left:0.46875rem ;
	}
	.cardtitle{
		font-size: 0.625rem;
		color: #fff;
		line-height: 100%;
		margin-bottom: 0.546875rem;
	}
	.carddetail p:not(.cardtitle){
		font-size: 0.3125rem;
		line-height: 100%;
		color: #fff;
	}
	.cardnum{
		margin-bottom:0.109375rem ;
	}
	.status{
		width:2.8rem;
		height:18px;
		position: absolute;
		top:0.28125rem;
		right:-0.75rem;
		line-height: 20px;
		font-size:10px;
		color: #fff;
		text-align: center;
		transform:rotate(40deg);
		-webkit-transform:rotate(40deg); 
		box-sizing: border-box;
	}
	.okstatus{
		background: #db5a15;
	}
	.overtime{
		background: #aaaaaa;
	}
</style>