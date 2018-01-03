<template>
  <div id="app">
    <div class="cardused"  
    	v-for="(rights,indexs) in rightsInfo.firstRightsPackages"
    	v-if="rights.usedCount != 0"
    	:ref="'go'+indexs">
    	<p class="title">{{rights.rightsName}}</p>
    	<div class="describe-title">
    		<span class="red-code">*</span>
    		<span class="describe">
    			<span v-if="rights.limitCount == -1">无限次,</span><span v-else>可任选{{rights.limitCount}}次服务，</span>已使用{{rights.usedCount}}次
    		</span>
    	</div>
    	<ul class="maindetail">
    		<li class="detail" 
    			v-for="(usedRecord,index) in rights.usedRecords"
    			:class="index > 1 && mark[indexs] ? 'cardhide' : ''"
    			>
    			<p>
	    			<span class="sleft">订单编号</span>
	    			<span class="sright">{{usedRecord.orderNo}}</span>
	    		</p>
	    		<p>
	    			<span class="sleft">服务时间</span>
	    			<span class="sright">{{usedRecord.orderCompletedTime}}</span>
	    		</p>
	    		<p v-for="recordItem in usedRecord.recordItems">
	    			<span class="sleft">{{recordItem.itemName}}</span>
	    			<span class="sright red">*{{recordItem.usedCount}}</span>
	    		</p>
    		</li>
    	</ul>
    	<div class="getdetail" v-if="rights.usedRecords.length >= 3" >
	    	<div class="open-close"  @click="getmore('go'+indexs)">
	    		<span v-if="mark[indexs]">查看更多</span>
	    		<span v-else>收起更多</span>
	    		<img src="./img/up.png" alt=""
	    			class="upordown"
	    			:class="mark[indexs] ? 'down':''"
	    			 v-if="rights.usedRecords.length >= 3"/>
	    	</div>
    	</div>
    </div>
    <div class="nodetail" v-if="!bol">
    	您还没有使用记录，抓紧下单吧~
    </div>
  </div>
</template>

<script>
		import '../common/base-1.3.css'
		import publicMethod from '../common/publicMethod.js'
    export default {
        data () {
	        	let serviceCardNo = publicMethod.getQuery('serviceCardNo')
	        	let userId = publicMethod.getQuery('userId')
	        	return {
	        		serviceCardNo: serviceCardNo,
	        		userId: userId,
	        		rightsInfo:{},
	        		useRecords: {},
	        		bol:false,
	        		mark:[],
	        		usedRecords: [
	        			{
	                "orderNo": "2",
	                "orderCreateTime": "2017-11-13 10:42:33",
	                "orderCompletedTime": "2017-11-29 10:42:37",
	                "recordItems": [
	                  {
	                    "itemId": 0,
	                    "itemName": "项目1",
	                    "usedCount": 2
	                	}, 
	                	{
	                    "itemId": 0,
	                    "itemName": "想买23",
	                    "usedCount": 1
	                	}
	                ]
            	},
            	{
	                "orderNo": "2",
	                "orderCreateTime": "2017-11-13 10:42:33",
	                "orderCompletedTime": "2017-11-29 10:42:37",
	                "recordItems": [
	                  {
	                    "itemId": 0,
	                    "itemName": "项目1",
	                    "usedCount": 2
	                	}, 
	                	{
	                    "itemId": 0,
	                    "itemName": "想买23",
	                    "usedCount": 1
	                	}
	                ]
            	},
            	{
	                "orderNo": "2",
	                "orderCreateTime": "2017-11-13 10:42:33",
	                "orderCompletedTime": "2017-11-29 10:42:37",
	                "recordItems": [
	                  {
	                    "itemId": 0,
	                    "itemName": "项目1",
	                    "usedCount": 2
	                	}, 
	                	{
	                    "itemId": 0,
	                    "itemName": "想买23",
	                    "usedCount": 1
	                	}
	                ]
            		}
	        		]
	        	}
        	},
        mounted () {
          this.$http.get(this.apiUrl.detail.detailServiceCard, {params: {userId: this.userId, serviceCardNo:this.serviceCardNo}}).then((response) => {
              response = response.body
              if (response.resultCode === 200) {
              	this.rightsInfo = response.data.rightsInfo
              	for(let i = 0;i<this.rightsInfo.firstRightsPackages.length;i++){
              		this.mark.push(true)
              	}
              	for(let i = 0;i<this.rightsInfo.firstRightsPackages.length;i++){
              		console.log(this.rightsInfo.firstRightsPackages[i].usedRecords)
              		if(this.rightsInfo.firstRightsPackages[i].usedRecords){
              			this.bol = true
              		}
              	}
              } else {
              	alert (response.message)
              }
          })
          //地址添加
          publicMethod.setAddrData()
        },
        methods: {
        	getmore (cell){
//      		if(this.$refs[cell][0].querySelector('.open-close span').innerHTML =='查看更多'){
//      			for(let i=0;i<this.rightsInfo.firstRightsPackages.length;i++){
//      				this.$refs['go'+i][0].querySelector('.open-close span').innerHTML ='查看更多'
//      				this.$refs['go'+i][0].querySelector('.open-close img').className = 'down'
//      			}
//      			this.$refs[cell][0].querySelector('.open-close span').innerHTML ='收起更多'
//      			this.$refs[cell][0].querySelector('.open-close img').className = 'upordown'
//      		}else{
//      			this.$refs[cell][0].querySelector('.open-close span').innerHTML ='查看更多'
//      			this.$refs[cell][0].querySelector('.open-close img').className = 'down'
//      		}
        		let oindex = parseInt(cell.substr(2))
        		if(this.mark[oindex]==false){
        			this.$set(this.mark,oindex,true)
        		}else{
        			for(let i = 0 ;i<this.mark.length;i++){
        				this.mark[i] = true
        			}
        			this.$set(this.mark,oindex,false)
        		}
        	}
        }
    }
</script>

<style>
	body {
		font-family: "helvetica","microsoft yahei";
		background: #f3f3f3;
	}
	.cardused {
		padding: 0 0.4rem 11px;
		background: #fff;
		margin-bottom: 10px;
		/*display: none;*/
	}
	.title {
		font-size:15px;
  	color: #272727;
		padding:17px 0;
		line-height: 100%;
		border-bottom: 1px solid #eeeeee;
	}
	.describe-title{
		padding: 15px 0;
		font-size: 0;
	}
	.red-code{
		color: #d40711;
		margin-right: 6px;
		font-size: 14px;
		padding-top: 3px;
		box-sizing: border-box;
		height: 19px;
		display: inline-block;
		vertical-align: middle;
	}
	.describe{
		font-size: 14px;
		color: #272727;
		display: inline-block;
		vertical-align: middle;
	}
	.maindetail{
		
	}
	.detail{
		width: 100%;
		background-color: #fafbfb;
		padding: 14px 0.266666rem 13px;
		box-sizing: border-box;
	}
	.detail{
		margin-bottom: 10px;
	}
	.detail p{
		display: webkit-flex;
		display: flex;
		justify-content: space-between;
		margin-bottom: 14px;
	}
	.detail p:last-child{
		margin-bottom: 0;
	}
	.detail .red{
		color: #d40711;
	}
	.detail span{
		color:#666666 ;
		font-size: 15px;
	}
	.sleft{
		min-width: 60px;
		max-width:7.64rem;
	}
	.sright{
		max-width:6.666666rem;
		word-wrap:break-word;
		text-align: right;
	}
	.getdetail{
		text-align: center;
		margin-top: 7px;
		font-size: 0;
	}
	.open-close{
		display: inline-block;
		font-size: 0;
	}
	.open-close span{
		/*display: inline-block;*/
		vertical-align: middle;
		font-size: 13px;
		color: #666666;
		margin-right: 10px;
	}
	.upordown{
		vertical-align: middle;
		width: 11px;
	}
	.down{
		transform:rotate(180deg);
		-webkit-transform:rotate(180deg);
	}
	.open-close .dis{
		display: none;
	}
	.nodetail{
		/*display: none;*/
		font-size:15px ;
		margin:150px auto;
		color: #acacac;
		text-align: center;
	}
	.cardhide{
		display: none;
	}
	.cardplay{
		display: block;
	}
	

</style>