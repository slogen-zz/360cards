<template>
	<div class="search-city" id="app">
		<div class="search-banner">
			<div id="search" class="clearfix">
				<div class="search-box">
					<input v-model="msg" type="text" @input="searchCityResult" class="search-input" placeholder="请输入城市／行政区／拼音">
					<a href="javascript:;" class="search-close" @click="clearInput"></a>
				</div>
			</div>
			<div class="locatCity" v-show="showLocate">
				<span>定位城市：</span>
				<i v-text="locatCity"></i><i v-if="cctb">（暂未开通）</i>
				<span class="re-locate" @click="doLocation"><img src="./img/re-locate.png" alt="" />重新定位</span>
			</div>

		</div>
		<div class="cityList" v-if="msg.length == 0">
			<div class="city-group" v-for="item in cityArrBind">
				<h2 v-text="item.cityIndex.toUpperCase()" v-bind:id="item.cityIndex.toUpperCase()"></h2>
				<ul>
					<li v-for="city in item.cityList" v-text="city.cityName" @click="clickCity(city)"></li>
				</ul>
			</div>
		</div>
		<div class="search-city-result" v-else-if="msg.length>0 && cityResults.length>0" :class="{'sm-padding-top': !showLocate}">
			<ul>
				<li v-for="item in cityResults" v-text="item" @click="abc(item)"></li>
			</ul>
		</div>
		<div class="no-results" v-else="msg.length>0 && cityResults.length==0">
			<p>抱歉，没有找到您要查找的城市哦～</p>
		</div>
		<ul class="index" v-show="isShowIndex">
			<li v-for="item in cityArrBind" v-text="item.cityIndex.toUpperCase()" @click="scroll"></li>
		</ul>
        <loading v-show="showMask" :msg="alertMessage" class="showMask"></loading>
	</div>
</template>

<script type="text/ecmascript-6">
    import '../common/base-1.3.css'
    import loading from 'common/loading/loading'
    import publicMethod from '../common/publicMethod.js'
	export default {
		name: 'App',
		data() {
			return {
				msg: '',
				items: [],
				resultLength: 0,
				locatCity: '',
				cityArrBind: [],
				cityResults: [],
				isShowIndex: true,
				showLocate: true,
                showMask: true, // 是否显示蒙版
                alertMessage:'',
                OcityList:[],
                cctb:false
			}
		},
        components:{
            loading
        },
		methods: {
			abc: function(item) {
				var cityId;
				var cityItem;
				for(var i = 0; i < this.cityArrBind.length; i++) {
					for(var j = 0; j < this.cityArrBind[i].cityList.length; j++) {
						if(item == this.cityArrBind[i].cityList[j].cityName) {
							cityId = this.cityArrBind[i].cityList[j].cityId;
							cityItem = this.cityArrBind[i].cityList[j];
						}
					}
				}
				if(cityItem) {
					this.clickCity(cityItem);
					return;
				}
				console.debug("选择城市为空");
			},
			clearInput: function(event) {
				this.msg = '';
			},
			clickCity: function(city) {
				//选择城市缓存，在添加地址的时候使用
				sessionStorage.creatAddrCity=JSON.stringify(city);
				localStorage.setItem("currentCity", JSON.stringify(city));
				setTimeout(function(){
					window.location.href='selectCell.html?token='+publicMethod.getToken()+'&cityId='+publicMethod.getCity()
				}, 200)
			},
			// 处理城市列表数据
			dealCityData: function(data) {
//				                var data = JSON.parse(data);
				if(data.resultCode == 200) {
					var cityArr = data.data,
						len = data.data.length;

					var firstCharArr = []; // 定义首字母数组
					for(var i = 0; i < len; i++) {
						var firstChar = cityArr[i].cityPinyin.toLowerCase()[0];
						firstCharArr.push(firstChar);
					}
					firstCharArr = this.delRepeat(firstCharArr); // 去重

					// 拼装cityArrBind
					for(var j = 0; j < firstCharArr.length; j++) {
						this.cityArrBind[j] = {
							cityIndex: firstCharArr[j],
							cityList: []
						}
						for(var k = 0; k < cityArr.length; k++) {
							if(cityArr[k].cityPinyin.toLowerCase()[0] == firstCharArr[j]) {
								this.cityArrBind[j].cityList.push({
									cityName: cityArr[k].cityName,
									cityPinyin: cityArr[k].cityPinyin,
									cityId: cityArr[k].cityId,
									cityNo:cityArr[k].cityNo,
									latitude: cityArr[k].latitude,
									longitude: cityArr[k].longitude
								})
							}
						}
					}
				} else {
					alert('请求数据失败，请刷新重试')
				}
			},
			// 数组去重
			delRepeat: function(ar) {
				var ret = [];
				for(var i = 0, j = ar.length; i < j; i++) {
					if(ret.indexOf(ar[i]) === -1) {
						ret.push(ar[i]);
					}
				}
				return ret.sort();
			},
			// 检索城市
			searchCityResult: function() {
				/**
				 * 根据汉字或拼音搜索词，返回在arr中的索引数组
				 * @param {string} [word] [搜索词，必须是汉字或拼音]
				 * @param {array} [arr] [搜索范围数组]
				 * @return {array} [word在arr中的索引数组]
				 * @example1
				 * word '湖'
				 * arr ['安徽','蚌埠','莱阳','湖南','湖北']
				 * 返回 [3,4]
				 * @example2
				 * word 'la'
				 * arr ['langfang','liushugou','laiyang']
				 * 返回 [0,2]
				 */
				var _this  = this;

				function searchCity(word, arr, b) {
					// 将word拆分成数组
					var arrWord = word.split('');
					if(arrWord.length == 0) { //判断是否为空数组
						return [];
					}
					// 生成正则表达式
					if(b == true) {
						var regStr = '^';
						for(var j = 0; j < arrWord.length; j++) {
							regStr += arrWord[j] + '\\S*';
						}
						regStr += '';
					} else {
						var regStr = '\\S*';
						for(var j = 0; j < arrWord.length; j++) {
							regStr += arrWord[j] + '\\S*';
						}
						regStr += '';
					}
					// 查找arr符合条件的数组元素索引
					var arrRet = [];
					for(var m = 0; m < arr.length; m++) {
						var reg = new RegExp(regStr);
						if(reg.test(arr[m])) {
							arrRet.push(m);
						}
					}
					return arrRet;
				}
				/**
				 * 将字符串的汉字部分和拼音部分分开,拼接为两个字符串中,并存储为数组
				 * @param {string} [str] [需要处理的字符串]
				 * @return {array} [长度为3，第一个数组元素为汉字字符串，第二个为字母,第三个表示首字符是否为拼音]
				 * @example
				 * 参数为 '乌Lu木qi',返回结果['乌木','luqi'](大写字母转换为小写)
				 */
				function sliStr(str) {
					var strArr = str.split('');
					var strHan = '';
					var strSpell = '';
					var m = 0;
					for(var i = 0; i < str.length; i++) {
						if(str.charCodeAt(i) > 64 && str.charCodeAt(i) < 91 || str.charCodeAt(i) > 96 && str.charCodeAt(i) < 123) {
							strSpell += strArr[i].toLowerCase();
						} else if(/^([\u4E00-\u9FA5]|[\uFE30-\uFFA0])*$/.test(strArr[i])) {
							strHan += strArr[i];
						} else {
							strSpell += strArr[i];
						}
					}
					while(m < str.length && !(str.charCodeAt(m) > 64 && str.charCodeAt(m) < 91 || str.charCodeAt(m) > 96 && str.charCodeAt(m) < 123) && !(/^([\u4E00-\u9FA5]|[\uFE30-\uFFA0])*$/.test(strArr[m]))) {
						m++;
					}
					var b = (str.charCodeAt(m) > 64 && str.charCodeAt(m) < 91 || str.charCodeAt(m) > 96 && str.charCodeAt(m) < 123) ? true : false;
					var arr = [strHan, strSpell, b];
					return arr;
				}
				/**
				 * 获取搜索词相关的所有城市，返回数组
				 * @param {string} [inpWord] [输入框文本]
				 */
				function retCity(inpWord) {
					var arr = sliStr(inpWord);
					// 获取城市的汉字和拼音全集
					var cityArr = [];
					var citySpellArr = [];
					(function() {
						for(var i = 0; i < _this.cityArrBind.length; i++) {
							for(var j = 0; j < _this.cityArrBind[i].cityList.length; j++) {
								cityArr.push(_this.cityArrBind[i].cityList[j].cityName);
								citySpellArr.push(_this.cityArrBind[i].cityList[j].cityPinyin.toLowerCase());
							}
						}
					})();
					var cityIndArr = searchCity(arr[0], cityArr);
					var spellIndArr = searchCity(arr[1], citySpellArr, arr[2]);
					// 取索引交集
					var allIndexArr = []; //存储索引交集数组
					(function(arr1, arr2) {
						if(arr1.length > 0 && arr2.length > 0) {
							var ai = 0,
								bi = 0;
							while(ai < arr1.length && bi < arr2.length) {
								if(arr1[ai] < arr2[bi]) {
									ai++;
								} else if(arr1[ai] > arr2[bi]) {
									bi++;
								} else /* they're equal */ {
									allIndexArr.push(arr1[ai]);
									ai++;
									bi++;
								}
							}
						} else {
							allIndexArr = allIndexArr.concat(arr1.length > 0 ? arr1 : arr2);
						}
					})(cityIndArr, spellIndArr);
					// 获取搜索词相关城市列表
					var retCityArr = []; //根据搜索词获取的结果数组
					for(var j = 0; j < allIndexArr.length; j++) {
						retCityArr.push(cityArr[allIndexArr[j]]);
					}
					return retCityArr;
				}
				this.cityResults = retCity(this.msg);
			},
			// 滚动列表
			scroll: function(event) {
				var t = event.target;
				var txt = t.innerHTML;
				var top = document.getElementById(txt).offsetTop;
				var h = document.querySelector('.search-banner').offsetHeight;
				document.getElementsByClassName('search-city')[0].scrollTop = top - h;
			},
			// 定位
			doLocation: function() {
				var me = this;
				// 百度地图API功能
				var map = new BMap.Map("allmap");
				//                var point = new BMap.Point(116.331398,39.897445);
				//                map.centerAndZoom(point,12);
				var BMAP_STATUS_SUCCESS = 0; //	检索成功。对应数值“0”。
				//                BMAP_STATUS_CITY_LIST	城市列表。对应数值“1”。
				//                BMAP_STATUS_UNKNOWN_LOCATION	位置结果未知。对应数值“2”。
				//                BMAP_STATUS_UNKNOWN_ROUTE	导航结果未知。对应数值“3”。
				//                BMAP_STATUS_INVALID_KEY	非法密钥。对应数值“4”。
				//                BMAP_STATUS_INVALID_REQUEST	非法请求。对应数值“5”。
				//                BMAP_STATUS_PERMISSION_DENIED	没有权限。对应数值“6”。(自 1.1 新增)
				//                BMAP_STATUS_SERVICE_UNAVAILABLE	服务不可用。对应数值“7”。(自 1.1 新增)
				//                BMAP_STATUS_TIMEOUT	超时。对应数值“8”。(自 1.1 新增)新增
				var geolocation = new BMap.Geolocation();
				geolocation.getCurrentPosition(function(r) {
					var address = r.address;
					me.locatCity = address.city;
					if(me.OcityList.length>0){
                        me.cct()
                    }
//					if(this.getStatus() == BMAP_STATUS_SUCCESS) {
//						// 请求城市数据并处理--接口
//						var newVar = {
//							cityName: address.city,
//							district: address.district,
//							province: address.province
//						};
//						me.$http.post("/wechat/location/city", newVar).then(function(resp) {
//
//							parseResponse(resp, function(data, msg) {
//								me.locatCity = data.cityName;
//								localStorage.setItem('cityName', data.cityName);
//							}, function(code, msg) {
//								me.locatCity = msg;
//							});
//						}, function(resp) {
//							console.debug("出错啦");
//							console.debug(resp);
//							me.locatCity = "无法定位，请手动选择";
//						});
//					} else {
//						alert('failed' + this.getStatus());
//					}
				}, {
					enableHighAccuracy: true
				});
				//关于状态码
			},
			cct(){
                let cityN=this.locatCity.replace(/市/g,'');
                let fb=true;
                for(let i=0;i<this.OcityList.length;i++){
                    let reg=new RegExp(cityN,'g')
                    if(reg.test(this.OcityList[i].cityName)){
                        fb=false;
                    }
                }
                if(fb){
                    this.cctb=true
                }
            }
		},
		created: function() {
			this.doLocation();
		},
		mounted: function() {
			//            // 请求城市数据并处理--接口
			this.$http.get(this.apiUrl.searchCity.listAllCity).then(function(res) {
				if(res.body.resultCode == 200){
				    this.showMask = false;
				    this.OcityList=res.body.data;
					this.dealCityData(res.body);
//					var cityId,
//						cityName = localStorage.getItem('cityName');
//					for(var i = 0; i < res.body.data.length; i++) {
//						if(cityName == res.body.data[i].cityName) {
//							cityId = res.body.data[i].cityId;
//						}
//					}
//					localStorage.setItem('cityId', cityId);
				}else{
                    this.showMask = false;
                }
			}, function(res) {
                this.showMask = false;
				console.log(res.status, res.statusText);
			});
		},
		watch: {
			msg: function(val, oldVal) {
				this.isShowIndex = !(val.length > 0);
				val.length > 0 ? (this.showLocate = false) : (this.showLocate = true);
			},
            OcityList(val,old){
			    this.cct()
            }
		}
	}
</script>

<style>
    html,body{
        height:100%;
    }
	/* 检索城市 */

	.search-city {
		position: fixed;
		left: 0;
		top: 0;
		width: 100%;
		height: 100%;
		z-index: 99;
		overflow-y: auto;
	}

	.locatCity {
		height: 45px;
		line-height: 45px;
		background-color: #fff;
		padding-left: 10px;
		font-size: 14px;
	}

	.locatCity span {
		font-size: 13px;
		color: #666;
		/*line-height: 45px;*/
	}

	.locatCity i {
		font-size: 14px;
		color: #222;
		/*line-height: 45px;*/
	}

	span.re-locate {
		float: right;
		margin-right: 10px;
		font-size: 12px;
		color: #d51200;
	}

	.re-locate img {
		width: 15px;
		height: 15px;
		vertical-align: middle;
		margin-right: 3px;
	}

	.index {
		position: fixed;
		right: 0;
		top: 100px;
		width: 0.5rem;
	}

	.city-group h2 {
		padding-left: 10px;
		line-height: 0.6875rem;
		font-size: 13px;
		color: #666;
	}

	.city-group ul {
		padding-left: 10px;
		background-color: #fff;
	}

	.city-group li {
		height: 45px;
		line-height: 45px;
		font-size: 14px;
		color: #222;
		border-bottom: 1px solid #eee;
	}

	.city-group li:last-child {
		border-bottom: none;
	}

	.index li {
		font-size: 10px;
		line-height: 20px;
		color: #040404;
	}

	.search-city-result {
		padding-top: 2.8125rem;
		padding-left: 10px;
		background-color: #fff;
	}

	div.sm-padding-top {
		padding-top: 44px;
	}

	.search-city-result li {
		height: 50px;
		line-height: 50px;
		font-size: 14px;
		color: #222;
		border-bottom: 1px solid #eee;
	}

	.search-city-result li:last-child {
		border-bottom: none;
	}

	.no-results p {
		padding-top: 2.8125rem;
		margin: 4.6875rem auto 0;
		font-size: 0.4375rem;
		color: #acacac;
		text-align: center;
	}

	.search-banner {
		position: fixed;
		left: 0;
		top: 0;
		width: 100%;
	}

	.cityList {
		padding-top: 95px;
	}

	#search {
		background-color: #fff;
		padding-left: 10px;
		padding-right: 10px;
		border-bottom: 1px solid #ddd;
		height: 48px;
		overflow: hidden;
	}

	.search-box {
		margin-top: 8px;
		position: relative;
		background: #f3f4f6 url("./img/search-adress.png") no-repeat 10px center;
		background-size: 13px;
		padding-left: 30px;
		height: 32px;
		line-height: 32px;
		border-radius: 0.078125rem;
	}

	.search-input {
		display: inline-block;
		vertical-align: top;
		outline: none;
		border: none;
		width: 100%;
		background-color: #f3f4f6;
		font-size: 13px;
		height: 100%;
		box-sizing: border-box;
		/* height: 26px; */
		line-height: 32px;
	}

	.search-input::-webkit-input-placeholder {
		font-size: 13px;
		color: #acacac;
	}

	.search-close {
		display: inline-block;
		position: absolute;
		top: 0;
		right: 0;
		height: 32px;
		width: 32px;
		background: url("./img/search-close.png") no-repeat center center;
		background-size: 13px;
	}
	/*.search-cancel {
	  float: right;
	  font-size: 0.46875rem;
	  color: #222;
	  line-height: 1.375rem;
	}*/
	/* --检索城市 */
</style>
