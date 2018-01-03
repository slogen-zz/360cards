<template>
	<div class="addcard">
		<div class="address" @click="goaddreslist">
			<div class="noaddres" v-if="address==null || this.address==''">
				<div class="addicon">
					<img src="./img/addadres.png"/>
				</div>
				<div class="newaddres">新建地址</div>
				<div class="arrow">
					<img src="./img/arrow.png" alt="" />
				</div>
			</div>
			<div class="hasaddres" v-else>
				<div class="positioning">
					<img src="./img/positioning.png"/>
				</div>
				<div class="detailaddress">
					<p>
						<span class="addresname"  v-cloak>{{detail.userName}}</span>
						<span  v-cloak>{{detail.iphone}}</span>
					</p>
					<p class="addresdetail"  v-cloak>{{detail.detailAddress}}</p>	
				</div>
				<div class="arrow">
					<img src="./img/arrow.png" alt="" />
				</div>
			</div>
		</div>
		<div class="cardmain">
			<div class="cardpass">
				<input
					type="text" 
					placeholder="请输入保修卡密码" 
					v-model='password'
					class="pass"
					ref="input"
					@focus="getFoucus"
					@input="getFoucus"
					@blur="loseBlur"
					/>
				<button class="activation" @click="active">立即激活</button>
				<div class="cardid" v-show="bol" >
					{{passwordCopy}}
				</div>
			</div>
			<div class="list">
				<p class="precautions">卡激活注意事项</p>
				<p>1.卡不记名，激活后才可绑定到您个人账户；</p>
				<p>2.激活前，请确认您登陆的是正确账户；</p>
				<p>3.每张卡对应激活地址唯一，激活前请确认卡绑定的是您所需地址；</p>
				<p>4.如无可用地址，请通过地址管理新增地址后，再绑定激活；</p>
				<p>5.激活后，卡权益即开通，自激活日起，一年内有效；</p>
				<p>6.同一地址不能同时用多张会员卡激活；</p>
				<p>7.卡有最迟激活时限，截止期见卡背面，请在此日期前激活此卡。</p>
			</div>
		</div>
		<bubble :moudel="BShow" :msg="msg"></bubble>
	</div>
</template>

<script>
	import '../common/base-1.3.css'
	import bubble from '../components/common/bubble/bubble'
	import publicMethod from '../common/publicMethod.js'
	export default {
		data(){
			return{
				address:'',
				detail:'',
				password:'',
				BShow: {
                    show: false
                },
                msg: '',
                passwordCopy:'',
                bol:false
			}
		},
		components:{
			bubble
		},
		methods:{
			active(){
				if(this.password && this.address){
//					if(this.password.length==19){
						let addressId = JSON.parse(sessionStorage.getItem('selectedAddress')).addressId
						let params={token: publicMethod.getToken(),servcieCardSecret:this.password,addressId:addressId}
						this.$http.get(this.apiUrl.addcard.activateServiceCard, {params}).then((res) => {
			              console.log(res.body)
			              if(res.body.resultCode==200){
//			               		window.location.href = 'index.html?token='+publicMethod.getToken()+'&cityId='+publicMethod.getCity()
			               		window.history.go(0-history.length+1)
			               }else{
			               		this.BShow.show = true
			               		this.msg = res.body.message
			               }
			           })
//					}else{
//						this.BShow.show = true
//			            this.msg = '卡密码错误，请重新输入。'
//					}
				}else if(this.password==''){
					this.BShow.show = true
			        this.msg = '保修卡密码不能为空哟~'
				}else if(!this.address){
					if(this.password){
						sessionStorage.setItem('password',this.password)
					}
					this.BShow.show = true
			        this.msg = '请输入地址'
				}else if(!this.address && !this.password){
					this.BShow.show = true
			        this.msg = '请输入地址'
				}
				
			},
			goaddreslist(){
				window.location.href ='addressList.html?token='+publicMethod.getToken()+'&cityId='+publicMethod.getCity()
			},
			getFoucus(){
				if(this.password.length){
					this.bol = true
				}else{
					this.bol = false
				}
			},
			loseBlur(){
				this.bol = false
			}
		},
		mounted(){
			//地址添加
          	publicMethod.setAddrData()
          	this.address = sessionStorage.getItem("selectedAddress")
          	this.detail = JSON.parse(this.address)
			if(sessionStorage.getItem('password')){
				this.password = sessionStorage.getItem('password')
			}
		},
		watch:{
			password(val,oldVal){
				this.password = val.split(' ').join('').substr(0,16)
//				let arr = val.replace(/\s/g,'').match(/(.{1,4})/g)
				if(val){
					let arr = val.replace(/\s/g,'').match(/(.{1,4})/g).join(' ').substr(0,19);
					this.passwordCopy = arr
				}
//				this.password = val.split(' ').join('').substr(0,16)
//				let arr = val.replace(/\s/g,'').match(/(.{1,4})/g);
//				if(val){
//					this.firPass = arr[0]
//        			this.secPass = arr[1]
//        			this.thirPass = arr[2]
//        			this.fourPass = arr[3]
//				}else{
//					this.firPass = ''
//        			this.secPass = ''
//        			this.thirPass = ''
//        			this.fourPass = ''
//				}
				
				//this.password = val.replace(/\s/g,'').replace(/(.{4})/g,"$1 ");
//				console.log(12)
//				this.password = val.replace(/\s/g,'').match(/(.{1,4})/g).join(' ').substr(0,19);
//				this.$refs.input.setSelectionRange(1,1)
			}
		}
	}
</script>

<style>
body,html{
	width: 100%;
	height: 100%;
}
.addcard{
	height: 100%;
	box-sizing: border-box;
	padding-bottom:1.515625rem; ;
}
.address{
	width: 10rem;
	height: 2.5625rem;
	background: url(img/address.jpg) no-repeat;
	background-position: center;
	background-size: cover;
	padding: 0.390625rem 0.34375rem 0.3125rem 0.34375rem;
	box-sizing: border-box;
	font-size: 0;
}
.address>div{
	height: 100%;
	align-items:center;
	display: -webkit-flex;
	position: relative;
	top: 0;
	left: 0;
}
.addicon{
	margin-right: 8px;
}
.detailaddress{
	margin:0 14px 0 12px;
}
.addresname{
	margin-right: 34px;
}
.addicon>img,.positioning>img{
	width:19px;
}
.newaddres{
	font-size: 15px;
	line-height: 100%;
}
.arrow{
	position: absolute;
	top: 50%;
	right: 0;
	margin-top: -8px;
}
.arrow>img{
	height: 16px;
}
.detailaddress{
	font-size: 14px;
	line-height: 100%;
}
.addresdetail{
	margin-top: 12px;
	font-size: 13px;
	color: #666666;
}
.cardmain{
	padding: 35px 0.3125rem 0;
	/*padding: 0.765625rem 0.3125rem 0;*/
}

.cardpass{
	position: relative;
	top: 0;
	left: 0;
	font-size: 0;
}
.cardid{
	position: absolute;
	bottom: 35px;
	left: 0;
	width:5.78125rem;
	margin-right: 0.15625rem;
	box-sizing: border-box;
	height:35px;
	line-height: 35px;
	/*border: 1px solid #f00;*/
	font-size: 18px;
	background:  #fffdef;
	border: 1px solid #f6eed0;
	color: #bf853c;
	border-radius: 4px;
	-webkit-border-radius: 4px;
	white-space:nowrap;
	overflow:hidden;
	text-overflow:ellipsis;
	padding-left: 0.1rem;
}
.cardid span{
	box-sizing: border-box;
	margin-left: 0.1rem;
	/*width: 1.2rem;*/
	height:35px;
	line-height: 35px;
	display: inline-block;
	vertical-align: middle;
}
.pass{
	border: 1px solid #cccccc;
	border-radius: 5px;
	-webkit-border-radius: 5px;
	height: 35px;
	line-height: 35px;
	width:5.78125rem;
	outline: none;
	margin-right: 0.15625rem;
	box-sizing: border-box;
	padding-left: 0.15625rem;
	font-size: 16px;
	z-index: 10;
}
.activation{
	outline: none;
	border: 1px solid #d40711;
	border-radius: 5px;
	-webkit-border-radius: 5px;
	height: 35px;
	color: #d40711;
	width: 3.4375rem;
	background: #fff;
	font-size: 15px;
}
::-webkit-input-placeholder {
    color: #acacac;
    font-size: 13px;
}
.list{
	background: #fafbfb;
	border: 1px dashed #ebebeb;
	border-radius: 5px;
	-webkit-border-radius: 5px;
	margin-top: 1.09375rem;
	padding:0.5rem 0.25rem ;
}
.precautions{
	font-size: 14px;
	color: #666666;
	text-align: center;
	margin-bottom: 10px;
}
.list>p{
	line-height: 20px;
}
.list>p:not(.precautions){
	font-size: 13px;
	color: #acacac;
	margin-top: 6px;
}
</style>