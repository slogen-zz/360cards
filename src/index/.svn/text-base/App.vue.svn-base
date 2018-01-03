<template>
    <div class="addcardlist">
       	<div class="addcard" v-if="mes.length==0">
       		<img src="./img/morecard.png" class="morecard"/>
       		<p class="nocard">您还未添加任何保修卡</p>
       		<p class="moredis">快速激活，享受更多优惠</p>
       	</div>
       	<div class="allcardlist" v-else>
       		<cardlist 
       			:message="message" 
       			v-for="(message,index) in mes"
       			:userId='result.userId'
       			:key="index"
       			></cardlist>
       			<!--<p class="usedescri" @click="moredetail" v-if="mes.length!=0">使用说明 >></p>-->
       			<p class="usedescri" @click="goSurvey">问题反馈 >></p>
       	</div>
       	<button class="newcard" @click="activation">新卡激活</button>
       
       	<carddetail :Show="Show"></carddetail>
       	<bubble :moudel="BShow" :msg="msg"></bubble>
    </div>
</template>

<script>
	import '../common/base-1.3.css'
	import cardlist from 'index/cardlist/cardlist'
	import carddetail from 'index/carddetail/carddetail'
	import bubble from '../components/common/bubble/bubble'
	import publicMethod from '../common/publicMethod.js'
	
    export default {
        name: 'index',
        data(){
            return {
            	Show:{
            		show:false
            	},
            	result:'',
            	mes:[
//			      	{
//			        	"serviceCardId": 11100,
//				        "serviceCardNo": "e1708011099",
//				        "serviceCardType": 1,
//				        "serviceCardTypeStr": "e家无忧·360卡",
//				        "serviceCardStatus": 1,
//				        "serviceCardStatusStr": "使用中",
//				        "availableTimeStr": "2017-08-28"
//			      	},
//			      	{
//			        	"serviceCardId": 11100,
//				        "serviceCardNo": "e1708011099",
//				        "serviceCardType": 2,
//				        "serviceCardTypeStr": "e家无忧·金卡",
//				        "serviceCardStatus": 1,
//				        "serviceCardStatusStr": "使用中",
//				        "availableTimeStr": "2017-08-30"
//			      	},
//			      	{
//			        	"serviceCardId": 11100,
//				        "serviceCardNo": "e1708011099",
//				        "serviceCardType": 3,
//				        "serviceCardTypeStr": "e家无忧·铂金卡",
//				        "serviceCardStatus": 2,
//				        "serviceCardStatusStr": "已过期",
//				        "availableTimeStr": "2017-08-28"
//			      	},
//			      	{
//			        	"serviceCardId": 11100,
//				        "serviceCardNo": "e1708011099",
//				        "serviceCardType": 1,
//				        "serviceCardTypeStr": "e家无忧·360卡",
//				        "serviceCardStatus": 1,
//				        "serviceCardStatusStr": "使用中",
//				        "availableTimeStr": "2017-08-28"
//			      	}
		    	],
		    	BShow: {
                    show: false
                },
                msg: ''
            }
        },
        components: {
            cardlist,
            carddetail,
            bubble
        },
        methods:{
        	moredetail(){
        		this.Show.show = true
        	},
        	activation(){
        		let token = publicMethod.getCookie('appToken')
        		if(!token){
        			try{
				      	ecej.login();
					    }catch(er){
					      	this.BShow.show = true
					        this.msg = '请登录'
					    }
        		}else{
        			window.location.href='addcard.html?token='+token+'&cityId='+publicMethod.getCity()
        		}
        		
        	},
        	goSurvey(){
        		window.location.href='https://www.wjx.top/jq/18710076.aspx'
        	}
        	
        },
        mounted () {
        	let token = publicMethod.getCookie('appToken')
	    		if(!token){
	    			try{
				      	ecej.login();
					}catch(er){
					    this.BShow.show = true
						  this.msg = '请登录'
					}
	    		}
        	let timer = setInterval(() => {
        		let token = publicMethod.getCookie('appToken')
        		if(token){
	        		this.$http.get(this.apiUrl.index.listServiceCard, {params: {token: token}}).then((res) => {
		                if(res.body.resultCode==200){
		                	this.result = res.body.data
		                	this.mes = res.body.data.pageList
		                }else{
		                	this.BShow.show = true
					        this.msg = res.body.message
		                }
		           })
	        		clearInterval(timer)
	        	}
        	},200)
        }
    }
</script>

<style>
	body,html{
		width: 100%;
		height: 100%;
		background:#F3F3F3 ;
	}
	.addcardlist{
		width: 100%;
		height: 100%;
		padding: 0.3125rem;
		box-sizing: border-box;
		overflow-y: auto;
	}
	.addcard{
		text-align: center;
		margin-top: 2.03125rem;
		margin-bottom: 0.9375rem;
		font-size: 0;
	}
	.morecard{
		width:2.1875rem;
	}
	.nocard{
		font-size: 0.46875rem;
		margin: 0.546875rem 0 0.46875rem 0;
	}
	.moredis{
		font-size: 0.40625rem;
		color: #acacac;
	}
	.newcard{
		width: 100%;
		height: 50px;
		background: #fff;
		border: none;
		outline: none;
		color: #d40711;
		font-size: 18px;
		line-height: 50px;
		position: fixed;
		bottom: 0;
		left: 0;
		z-index: 10;
		border-top: 1px solid #dddddd;
		box-sizing: border-box;
	}
	.allcardlist{
		padding-bottom:1.8rem ;
	}
	.usedescri{
		font-size: 12px;
		/*font-size: 0.375rem;*/
		color:#666666;
		text-align: right;
		line-height:100% ;
	}
</style>
