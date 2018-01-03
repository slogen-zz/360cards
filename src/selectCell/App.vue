<template>
	<div id="app">
	    <div class="adress-banner">
	        <a href="javaScript:;" @click="chooseCity()" class="city"><span v-text="city.cityName"/></a>
	        <div class="input-city-box">
	            <div style="display: table; width: 100%">
	                <a href="javascript:;" @click="goSearchCell" class="input-city"><span>请输入小区或大厦、街道名称</span></a>
	            </div>
	        </div>
	    </div>
	    <div id="map"></div>
	    <div >
	        <ul style="background-color: #fff">
	            <li v-for="item in items" @click="confirmAdress($event,item)" class="info-item">
	                <h2 v-text="item.title"></h2>
	                <span v-text="item.adress"></span>
	            </li>
	        </ul>
	    </div>
	    <loading v-show="showMask" :msg="alertMessage"></loading>
	</div>
</template>

<script type="text/ecmascript-6">
    import '../common/base-1.3.css'
    import loading from 'common/loading/loading'
    import publicMethod from '../common/publicMethod.js'
	export default{
		name: 'App',
		data(){
			return{
				items:[],
	            city:{"cityName":""},
	            showMask: false, // 是否显示蒙版
	            alertMessage:""
			}
		},
		created:function () {
            if(sessionStorage.creatAddrCity!=''&&sessionStorage.creatAddrCity!='undefined'&sessionStorage.creatAddrCity!=undefined){
                var currentCity=sessionStorage.creatAddrCity;
            }else{
                var currentCity=localStorage.currentCity;
            }
            var _this=this;
            if(currentCity){
                this.city = JSON.parse(currentCity);
                console.log(this.city)
            }else{
                //加载当前city
                //正式用此动态的
                //this.city={"cityName":[[${chooseCityInfo.cityName}]],"cityId":[[${chooseCityInfo.cityId}]],"longitude":[[${chooseCityInfo.longitude}]],"latitude":[[${chooseCityInfo.latitude}]]};

                var localCity=new BMap.LocalCity();
                localCity.get(function(res){
                    if(res){
                        _this.city = {"cityName":res.name,"longitude":res.center.lng,"latitude":res.center.lat}
                    }
                })


//                this.city = {"cityName":'廊坊',"cityId":'2',"longitude":116.690340000000000,"latitude":39.543520000000000}
//                localStorage.currentCity = JSON.stringify(this.city);
            }

        },
        mounted: function(){

        },
        watch:{
            city(){
                this.getLocation();
            }
        },
        components:{
            loading
        },
        methods: {
        		goSearchCell () {
							location.href = 'searchCell.html?token='+publicMethod.getToken()+'&cityId='+publicMethod.getCity();
        		},
            confirmAdress: function (e,item) {    // 点击地址，更新本地存储的地址信息
                var t = e.target;
                var _this = this;
                console.log(t)
                if(!t.className.match(/info-item/)){
                    t = t.parentNode;
                }
                _this.showMask=true;

                // 创建地理编码实例
                var myGeoClick = new BMap.Geocoder();
                myGeoClick.getLocation(item.point, function(result){
                    sessionStorage.setItem('districtName', result.addressComponents.district);
                    sessionStorage.setItem('province', result.addressComponents.province);
                    sessionStorage.setItem('cityName', result.addressComponents.city);

                    sessionStorage.setItem('community', t.querySelectorAll('h2')[0].innerHTML);

                    //存储经纬度
                    sessionStorage.setItem("longitude",item.point.lng);
                    sessionStorage.setItem("latitude",item.point.lat);

                    _this.showMask=false;
                    location.href = 'newAddress.html?token='+publicMethod.getToken()+'&cityId='+publicMethod.getCity();

                })

            },
            chooseCity:function () {
                sessionStorage.setItem('skipUrl', true);
                location.href = 'searchCity.html?token='+publicMethod.getToken()+'&cityId='+publicMethod.getCity();
            },
            getLocation: function(){
            	var that = this;
            	var map = new BMap.Map("map");          // 创建地图实例
			    var longitude=that.city.longitude;
			    var latitude=that.city.latitude;
			    var point = new BMap.Point(longitude,latitude);  // 创建点坐标

			    map.centerAndZoom(point, 15);
			    var opts = {
			        anchor: BMAP_ANCHOR_BOTTOM_RIGHT,
			        showAddressBar:false,
			        enableAutoLocation: true
			    };
			    var geolocationControl = new BMap.GeolocationControl(opts);

			    geolocationControl.location();
			    geolocationControl.addEventListener("locationSuccess", function(e){
			        map.clearOverlays();        //清除地图覆盖物
			        map.panTo(point);
			        var marker = new BMap.Marker(point);  // 创建标注
			        map.addOverlay(marker);

			        // 定位信息e.addressComponent

			        // 搜索附近小区
			        var options = {
			            onSearchComplete: function(results){
			                // 判断状态是否正确
			                if (local.getStatus() == BMAP_STATUS_SUCCESS){
                                that.items = [];
			                    for (var i = 0; i < results.getCurrentNumPois(); i ++){
                                    that.items.push({
			                            title:results.getPoi(i).title,
			                            city:results.getPoi(i).city,
			                            adress:results.getPoi(i).address,
			                            point:results.getPoi(i).point
			                        });
			                    }
			                }
			            }
			        };
			        var local = new BMap.LocalSearch(point, options);
			        local.searchNearby("小区", point, 1000);

//			        // 创建地理编码实例
//			        var myGeo = new BMap.Geocoder();
//			        // 根据坐标得到地址描述
//			        myGeo.getLocation(point, function(result){
//			            if (result){
//			                sessionStorage.setItem('districtName', result.addressComponents.district);
//                            sessionStorage.setItem('province', result.addressComponents.province);
//                            sessionStorage.setItem('cityName', result.addressComponents.city);
//			            }
//			        });

			    });
			    geolocationControl.addEventListener("locationError",function(e){
			        // 定位失败事件
			        alert(e.message);
			    });
			    map.addControl(geolocationControl);

			    map.addEventListener('dragend', function(){
			        map.clearOverlays();        //清除地图覆盖物
			        var marker = new BMap.Marker(map.getCenter());  // 创建标注
			        map.addOverlay(marker);
			        var point = map.getCenter();
			        var options = {
			            onSearchComplete: function(results){
			                // 判断状态是否正确
			                if (local.getStatus() == BMAP_STATUS_SUCCESS){
                                that.items = [];
			                    for (var i = 0; i < results.getCurrentNumPois(); i ++){
                                    that.items.push({
			                            title:results.getPoi(i).title,
			                            city:results.getPoi(i).city,
			                            adress:results.getPoi(i).address,
			                            point:results.getPoi(i).point
			                        });
			                    }
			                }
			            }
			        };
			        var local = new BMap.LocalSearch(point, options);
			        local.searchNearby("小区", point, 1000);
			    })
            }
        },

	}

</script>

<style>
	/* 选择小区 */

	/* 选择小区地图 */

	.adress-banner {
	  overflow: hidden;
	  padding-left: 15px;
	  padding-right: 15px;
	  height: 1.40625rem;
	  background-color: #fff;
	  border-bottom: 1px solid #ddd;
	}


	.adress-banner .city {
	  float: left;
	  display: block;
	  margin-right: 15px;
	  padding-right: 16px;
	  line-height: 1.40625rem;
	  background: url("./img/arrow.png") no-repeat right center;
	  background-size: 10.5px;
	  font-size: 15px;
	}
	#app .adress-banner .city span{
	  font-size: 0.4375rem;
	  color: #222;

	}
	#app .adress-banner .input-city span{
	  color: #acacac;
	}
	.input-city-box {
	  overflow: hidden;
	  background-color: #f3f3f3;
	  margin-top: 0.28125rem;
	  height: 0.875rem;
	  border-radius: 0.0625rem;
	}
	.adress-banner .input-city {
	  display: table-cell;
	  text-align: center;
	  vertical-align: middle;
	  font-size: 0.40625rem;
	  width: 100%;
	  height: 0.875rem;
	  color: #acacac;
	  border-radius: 0.078125rem;
	}

	.input-city span {
	  display: inline-block;
	  padding-left: 0.5625rem;
	  padding-top: 0.03125rem;
	  background: url("./img/search.png") no-repeat left center;
	  background-size: 0.4375rem;
	  vertical-align: middle;
	}

	#map {
	  height: 7.125rem;
	}

	#app li {
	  padding-top: 15px;
	  padding-left: 10px;
	  padding-bottom: 13px;
	  border-bottom: 1px solid #eee;
	}

	#app li:first-child h2:before {
	  content: '[当前]';
	  color: #d81300;
	  margin-right: 0.3125rem;
	}

	#app h2 {
	  margin-bottom: 12px;
	  font-size: 14px;
	  color: #222;
	  line-height: 100%;

	}
	::-webkit-input-placeholder {
	  /* WebKit browsers */
	  color: #ACACAC;
	  font-size: 0.40625rem;
	}

	#app span {
	  font-size: 0.375rem;
	  color: #666;
	  vertical-align: middle;

	}

	/* --选择小区地图 */
</style>
