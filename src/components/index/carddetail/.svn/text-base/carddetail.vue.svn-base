<template>
	<div class="playrule vbase" style="display: none;">
		<transition name="fade" v-on:before-enter="beforeEnter" v-on:after-leave="afterLeave">
			<div class="prMain" v-if='Show.show'>
				<div class="prMainFir">
					<p>使用说明</p>
					<ul>
						<li>
							<p>
								<span>Q：</span>我的卡包含什么服务？
							</p>
							<p>
								<span>A：</span>请参看卡详细服务说明。
							</p>
						</li>
						<li>
							<p>
								<span>Q：</span>我如何查询服务剩余情况？
							</p>
							<p>
								<span>A：</span>卡详细说明下方，有会员权益使用统计。
							</p>
						</li>
						<li>
							<p>
								<span>Q：</span>我如何确认卡有效期？
							</p>
							<p>
								<span>A：</span>在卡包里，会显示您卡的服务有效期。
							</p>
						</li>
						<li>
							<p>
								<span>Q：</span>我如何下单及结算？
							</p>
							<p>
								<span>A：</span>您通过正常渠道下单即可，卡内权益会免费上门服务。
							</p>
						</li>
						<li>
							<p>
								<span>Q：</span>一张卡是否能用于多个地址？
							</p>
							<p>
								<span>A：</span>不能，如您有多个地址，可再绑定其他卡。
							</p>
						</li>
					</ul>
				</div>
				<div class="prMainSec">
					<img src="./img/playclose.png" alt="" @click='playclose'/>
				</div>
			</div>

		</transition>
	</div>
</template>

<script>
	export default {
	 	props:['Show'],
	 	methods:{
	 		beforeEnter(){
            	document.querySelector('.playrule').style.display='block';
	        },
	        afterLeave(){
	            document.querySelector('.playrule').style.display='none';
	        } ,
	 		playclose:function(){
	 			this.Show.show = false
	 		}
	 	}
	}
</script>

<style>
	.playrule{
		width: 100%;
		height: 100%;
		background: rgba(0,0,0,0.5);
		position: fixed;
		z-index: 10;
		top: 0;
		left: 0;
		text-align: center;
	}
	.prMain{
		display: inline-block;
		text-align: center;
		vertical-align: middle;
		width: 87.5%;
		font-size: 0;
	}
	.prMainFir{
		background: #fff;
		border-radius: 4px;
	}
	.prMainFir p{
		font-size: 15px;
        color: #666;
        line-height: 20px;
	}
	.prMainFir>p{
		height: 40px;
		line-height: 40px;
		color: #222;
		border-bottom: 1px solid #EEEEEE;
	}
	.prMainFir p span{
		color: #666666;
	}
	.prMainFir ul{
		text-align: left;
		margin: 0 10px;
		padding-bottom: 12px;
	}
	.prMainFir ul li{
		margin-top: 7px;
	}

	.prMainSec img{
		width: 31px;
		margin-top: 15px;
	}
	.fade-enter-active, .fade-leave-active {
        -webkit-transition: all .2s;
        transition: all .2s;
    }
    .fade-enter, .fade-leave-active {
        -webkit-transform: scale(0);
        transform: scale(0);
    }
</style>
