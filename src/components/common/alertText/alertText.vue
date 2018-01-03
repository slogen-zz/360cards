<template>
	<div class="shadow">
	    <div class="shadow-cell">
		    <div class="alert-window" v-show="showAlertWindow">
			    <div class="header" v-html="headerMsg"></div>
			    <div class="window-btn">
			    	<div class="alert-btn" @click="item.callback" v-for="item in btnText" v-text="item.text"></div>
			    </div>
		    </div>
	    </div>
    </div>
</template>

<script type="text/ecmascript-6">
	export default{
		 props: ['headerMsg', 'btnText', 'showBox'],
		 data(){
		 	return{
		 		changeCityMsg:"服务地址需要切换城市才可以选择哦~",
                chooseDisableAddress:true,
                changeCityText:[
                    {
                        text: '确认切换',  // 按钮文本
                        callback: function () {  // 点击按钮的回调函数
                            address.chooseDisableAddress = false;
                            //清缓存
                            localStorage.setItem('username','');
                            localStorage.setItem('telnum','');
                            localStorage.setItem('serveplace','');
                            localStorage.setItem('detailplace','');

                            localStorage.setItem('districtName','');
                            localStorage.setItem('province','');
                            localStorage.setItem('cityName','');
                            localStorage.setItem('community','');

                            localStorage.setItem("currentCity","");

                            location.href="/wechat/location/city/choose";
                        }
                    },{
                        text: '换个地址', // 按钮文本
                        callback: function () {  // 点击按钮的回调函数
                            address.chooseDisableAddress = false;
                        }
                    }
                ]
		 	}
		 },
		 computed: {
	        showAlertWindow: function () {
	            if(this.showBox == undefined || this.showBox){
	                return true;
	            }else {
	                return false;
	            }
	        }
	    }
	}
</script>

<style>
	/* 弹窗 */
	.shadow {
	  display: table;
	  position: fixed;
	  left: 0;
	  top: 0;
	  width: 100%;
	  height: 100%;
	  background-color: rgba(127,127,127,0.5);
	  text-align: center;
	}
	
	.shadow-cell {
	  display: table-cell;
	  vertical-align: middle;
	}
	
	.alert-window {
	  margin: 0 auto;
	  width: 7.8125rem;
	  background-color: #fff;
	  text-align: center;
	  border-radius: 0.15625rem;
	}
	
	.header {
	  padding-top: 1.09375rem;
	  padding-bottom: 0.9375rem;
		padding-left: 0.625rem;
		padding-right: 0.625rem;
		font-size: 0.5rem;
	  color: #222;
	}
	
	.window-btn {
	  display: -webkit-box;
	  display: -ms-flexbox;
	  display: flex;
	  border-top: 0.015625rem solid #e5e5e5;
	  line-height: 1.5625rem;
	  font-size: 0.5625rem;
	  color: #222;
	  background-color: #e5e5e5;
		border-bottom-left-radius: 0.15625rem;
		border-bottom-right-radius: 0.15625rem;
	}
	
	.alert-btn {
	  margin-left: 1px;
	  width: 0;
	  -webkit-box-flex: 1;
	  -ms-flex: 1;
	  flex: 1;
	  background-color: #fff;
	}
	.alert-window div:nth-child(1){
		border-bottom-left-radius: 0.15625rem;
	
	}
	.alert-window div:nth-child(2){
	 border-bottom-right-radius: 0.15625rem;
	
	}
	.alert-btn:first-child {
	  margin-left: 0;
	}
	
	.dialog-tel {
	  color: #d81300;
	}
	
	/* --弹窗 */
</style>