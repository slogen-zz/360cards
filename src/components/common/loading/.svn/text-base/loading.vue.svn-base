<template>
	 <div class="font-num-shadow">
            <div class="font-num">
                <div class="num-window" v-show="msg.length>0">{{msg}}</div>
                <div class="num-window loading" v-show="msg.length==0">
                    <img src="./img/loading-img.png"/>
                </div> 
            </div>
        </div>
</template>

<script  type="text/ecmascript-6">
	export default{
		 props:['msg'],
		 methods: {
		 	
		 }
	}
</script>

<style>
	/* 蒙版 */
	div.loading {
	    padding: 0;
	    width: 1.25rem;
	    height: 1.25rem;
	    border-radius: 0.125rem;
	    background-color: rgba(0,0,0,0.5);
	}
	.loading img {
	    margin: 0 auto;
	  margin-top: 0.3125rem;
	  width: 0.609375rem;
	  height: 0.609375rem;
	  -webkit-animation:rotate 1.2s linear infinite;
	  animation:rotate 1.2s linear infinite;
	
	}
	@-webkit-keyframes rotate {
	  0% {
	    -webkit-transform:rotate(0deg);
	  }
	  50%{
	    -webkit-transform:rotate(180deg);
	  }
	  100%{
	    -webkit-transform:rotate(360deg);
	  }
	}
	@keyframes rotate {
	  0% {
	    transform:rotate(0deg);
	  }
	  50%{
	    transform:rotate(180deg);
	  }
	  100%{
	    transform:rotate(360deg);
	  }
	}
	/* --蒙版 */
	/* 字数超过40个 */

	.font-num-shadow {
	  position: fixed;
	  left: 0;
	  top: 0;
	  width: 100%;
	  height: 100%;
	  display: table;
	  background-color: rgba(50,50,50,0.6);
	  z-index: 1000;
	}
	
	.font-num {
	  display: table-cell;
	  vertical-align: middle;
	  text-align: center;
	}
	
	.num-window {
	  margin: 0 auto;
	  display: inline-block;
	  padding-left: 0.25rem;
	  padding-right: 0.25rem;
	  font-size: 0.4375rem;
	  text-align: center;
	  line-height: 1.875rem;
	  background-color: rgba(51,51,51,0.7);
	  border-radius: 0.078125rem;
	  color: #fff;
	}
	
	/* --字数超过40个 */
</style>