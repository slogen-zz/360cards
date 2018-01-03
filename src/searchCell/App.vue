<template>
    <div id="app">
        <div id="search" class="clearfix">
            <div class="search-box">
                <input v-model="msg" @input="searchPlot" type="text" class="search-input" placeholder="请输入小区或大厦、街道名称">
                <a href="javascript:;" class="search-close" @click="clearInput"></a>
            </div>
            <a href="/wechat/address/chooseAddr" class="search-cancel">取消</a>
        </div>
        <div id="result-list" :style="noBack">
            <ul v-if="resultLength > 0">
                <li v-for="item in items" @click="confirmAdress($event,item)" class="info-item">
                    <h2 v-text="item.title"></h2>
                    <span v-text="item.adress"></span>
                </li>
            </ul>
            <div v-else-if="resultLength == 0 && msg.length > 0">
                <p class="none-result">抱歉，当前城市未找到您要查到的地址哦～<br>
                    如果查找不到您的小区，可以选择附近小区。</p>
            </div>
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
            return {
                msg: '',
                items: [],
                resultLength: 0,
                showMask: false, // 是否显示蒙版
                alertMessage: ""
            }
        },
        components: {
            loading
        },
        computed: {
            noBack: function () {
                return {
                    backgroundColor: (this.resultLength == 0 && this.msg.length > 0) ? '#f3f3f3' : '#fff'
                }
            }
        },
        methods: {
            searchPlot: function (event) {   // 搜索相关小区
                var _this = this;
                var options = {
                    onSearchComplete: function (results) {

                        // 判断状态是否正确
                        if (local.getStatus() == BMAP_STATUS_SUCCESS) {
                            _this.resultLength = results.getCurrentNumPois();
                            _this.items = [];
                            for (var i = 0; i < results.getCurrentNumPois(); i++) {
                                _this.items.push({
                                    title: results.getPoi(i).title,
                                    adress: results.getPoi(i).address,
                                    point: results.getPoi(i).point
                                });
                            }
                        }
                    }
                };

                if (sessionStorage.creatAddrCity) {
                    var currentCity = JSON.parse(sessionStorage.creatAddrCity);
                } else {
                    var currentCity = JSON.parse(localStorage.currentCity);
                }

                var local = new BMap.LocalSearch(currentCity.cityName, options);
                local.search(this.msg);
            },
            clearInput: function (event) {
                this.msg = '';
            },
            confirmAdress: function (e, item) { // 点击地址，更新本地存储的地址信息
                var t = e.target;
                var that = this;
                console.log(t)
                if (!t.className.match(/info-item/)) {
                    t = t.parentNode;
                }
                that.showMask = true;

                // 创建地理编码实例
                var myGeoClick = new BMap.Geocoder();
                myGeoClick.getLocation(item.point, function (result) {
                    sessionStorage.setItem('districtName', result.addressComponents.district);
                    sessionStorage.setItem('province', result.addressComponents.province);
                    sessionStorage.setItem('cityName', result.addressComponents.city);


                    sessionStorage.setItem('community', t.querySelectorAll('h2')[0].innerHTML);

                    //存储经纬度
                    sessionStorage.setItem("longitude", item.point.lng);
                    sessionStorage.setItem("latitude", item.point.lat);

                    that.showMask = false;
                    location.href = 'newAddress.html?token='+publicMethod.getToken()+'&cityId='+publicMethod.getCity();
                })
            }
        }
    }
</script>

<style>
    /* 选择小区搜索框 */

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

    .search-cancel {
        float: right;
        font-size: 0.46875rem;
        color: #222;
        line-height: 1.375rem;
    }

    #result-list {
        padding-left: 0.3125rem;
        padding-right: 0.3125rem;
        /*background-color: #fff;*/
        margin-top: 0.3125rem;
    }

    #result-list li {
        padding-top: 0.46875rem;
        padding-left: 0.3125rem;
        padding-bottom: 0.40625rem;
        border-bottom: 1px solid #eee;
    }

    #result-list h2 {
        margin-bottom: 0.3125rem;
        font-size: 0.4375rem;
        color: #222;
        line-height: 100%;

    }

    #result-list span {
        vertical-align: middle;
        font-size: 0.375rem;
        color: #666;
        line-height: 100%;

    }

    .none-result {
        margin-top: 4.375rem;
        font-size: 0.4375rem;
        color: #acacac;
        line-height: 0.96875rem;
        text-align: center;
    }

    /* --选择小区搜索框 */
</style>
