<template>
    <div class="description" v-html="cardInstructionInfo">
       	
    </div>
</template>

<script>
	import '../common/base-1.3.css'
	import publicMethod from '../common/publicMethod.js'
    export default {
        data(){
            let serviceCardNo = publicMethod.getQuery('serviceCardNo')
	        let userId = publicMethod.getQuery('userId')
	        return {
	        	serviceCardNo: serviceCardNo,
	        	userId: userId,
	        	cardInstructionInfo:''
	        }
        },
        methods:{
        	
        },
        mounted () {
        	this.$http.get(this.apiUrl.detail.detailServiceCard, {params: {userId: this.userId, serviceCardNo:this.serviceCardNo}}).then((response) => {
              response = response.body
              if (response.resultCode === 200) {
              	console.log(response.data)
              	this.cardInstructionInfo = response.data.cardInstructionInfo
              } else {
              	alert (response.message)
              }
          })
          //地址添加
          publicMethod.setAddrData()
        }
    }
</script>

<style>
	body,html{
		width: 100%;
		height: 100%;
		background:#F3F3F3 ;
	}
	.description{
		width: 100%;
		height: 100%;
		padding: 0.3125rem;
		box-sizing: border-box;
		font-size: 16px;
	}
	
</style>
