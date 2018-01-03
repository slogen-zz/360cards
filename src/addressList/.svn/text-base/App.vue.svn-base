<template>
    <div id="app">
        <!--当有常用地址时，显示此选项-->
        <div class="address-lists" v-if="addressData.length>0">

            <div class="address-list" v-for="address in addressData"
                 :class="{'clickable-false' : address.clickable}">
                <p class="clearfix">
                    <span class="a-name" v-text="address.userName"></span>
                    <span class="a-number" v-text="address.iphone"></span>
                </p>
                <p @click="selectAddress(address)"><i v-text="address.detailAddress"></i></p>
            </div>

        </div>
        <!--当无常用地址时，显示此选项-->
        <div class="com-address-null" v-else="addressData.length==0">
            <img src="./img/com-address.png" alt=""/>
            <p>您还未添加新地址</p>
        </div>
        <div style="height: 45px;"></div>
        <div class="new-address"><a href="javaScript:;" @click="addAddress()"><img src="./img/com-add.png" alt=""/>新增地址</a>
        </div>
        <!--<alert-window :header-msg="headerMsg" :btn-text="btnText" v-if="isAlert"></alert-window>-->
        <!--<alertText :header-msg="changeCityMsg" :btn-text="changeCityText" v-if="chooseDisableAddress"></alertText>-->
        <!--<test :Show="testShow" :successFn="ajax"></test>-->
        <bubble :moudel="BShow" :msg="msg"></bubble>
    </div>
</template>

<script type="text/ecmascript-6">
    import '../common/base-1.3.css'
    import alertText from 'common/alertText/alertText'
    //import test from 'common/test/test'
    import bubble from 'common/bubble/bubble'
    import publicMethod from '../common/publicMethod.js'
    export default{
        name: 'App',
        data(){
            return {
                addressData: [],
                testShow: {
                    show: false
                },
                currentCity : JSON.parse(localStorage.currentCity),
                BShow:{
                    show:false
                },
                msg: ''
            }
        },
        components: {
//            alertText,
            //test,
            bubble
        },
        watch: {
            username: function (val, oldVal) {
                sessionStorage.setItem('username', val);
            },
            iphone: function (val, oldVal) {
                sessionStorage.setItem('iphone', val);
            },
            province: function (val, oldVal) {
                sessionStorage.setItem('province', val);
            },
            cityname: function (val, oldVal) {
                sessionStorage.setItem('cityname', val);
            },
            districtName: function (val, oldVal) {
                sessionStorage.setItem('districtName', val);
            },
            community: function (val, oldVal) {
                sessionStorage.setItem('community', val);
            },
            detailaddress: function (val, oldVal) {
                sessionStorage.setItem('detailaddress', val);
            },
        },
        mounted: function () {
            this.getAddress();
        },
        methods: {
            //接口数据
            ajax(){
                var currentCity = JSON.parse(localStorage.currentCity);
                var params = {
                    "token": publicMethod.getToken(),
                };
                this.$http.get(this.apiUrl.addressList.addressList, {params}).then(function (res) {
                    if (res.body.resultCode == 200) {
                        this.addressData = res.body.data;
                        for(let i=0;i<this.addressData.length;i++){
                            this.addressData[i].clickable=(this.addressData[i].cityId!=this.currentCity.cityId);
                        }
                    }
                }, function (res) {
                    console.log(res.status, res.statusText);
                });
            },
            getAddress: function () {
                if (sessionStorage.actToken) {
                    this.ajax();
                } else {
//                  this.BShow.show = true;
//                  this.msg = '参数不能为空'
					console.log('token不能为空')
                }
            },
            addAddress: function () {
                sessionStorage.removeItem('username');
                sessionStorage.removeItem('telnum');
                sessionStorage.removeItem('serveplace');
                sessionStorage.removeItem('detailplace');
                sessionStorage.removeItem('districtName');
                sessionStorage.removeItem('province');
                sessionStorage.removeItem('cityName') ;
                sessionStorage.removeItem('community');

                //localStorage.setItem("currentCity","");

                location.href = 'newAddress.html?token='+publicMethod.getToken()+'&cityId='+publicMethod.getCity();
            },
            //选中地址
            selectAddress: function (address) {
                if(address.clickable) return;
                var isAreas = {
                    'cityId': this.currentCity.cityId,
                    'longitude': address.longitude,
                    'latitude': address.latitude,
                    'token': publicMethod.getToken(),
                };
                this.$http.get(this.apiUrl.addressList.isServiceArea, {params: isAreas}).then(function (res) {
                    sessionStorage.setItem("selectedAddress", JSON.stringify(address));
                    if (res.body.resultCode == 200) {
                        if (res.body.data == 0) {
                            this.msg = resp.body.message;
                            this.BShow.show = true;
                            return false;
                        }else if(res.body.data == 1){
                            sessionStorage.setItem("selectedAddress", JSON.stringify(address));
                            location.href = 'addcard.html?token='+publicMethod.getToken()+'&cityId='+publicMethod.getCity();
                        }
                    }
                }, function (res) {

                });
            }
        }
    }
</script>

<style>
    /*常用地址start*/
    .com-address-null {
        text-align: center;
        font-size: 0.46875rem;
        margin-top: 4.53125rem;
        color: #acacac;
    }

    .com-address-null img {
        width: 71px;
        margin-bottom: 15px;
    }

    .new-address {
        width: 100%;
        height: 45px;
        line-height: 45px;
        position: fixed;
        bottom: 0;
        left: 0;
        text-align: center;
        background: #fff;
        border-top: 1px solid #ddd;
        font-size: 18px;
        margin-top: 45px;
    }

    .new-address a {
        display: block;
        color: #d81300;
    }

    .new-address img {
        width: 0.46875rem;
        padding-top: 13px;
        padding-right: 3px;
        /*vertical-align: middle;*/
    }

    .address-list {
        background: #fff;
        padding: 0 10px;
        margin-bottom: 10px;
    }

    .address-list p {
        /*line-height: 1.25rem;*/
        padding: 12px 0;
        padding-bottom: 0;
    }

    .address-list p:nth-child(1) {
        padding-top: 20px;
    }

    .address-list p:nth-child(2), .address-list p:nth-child(3) {
        padding-top: 12px;
        padding-bottom: 15px;
    }

    .address-list p:nth-child(1) span {
        font-size: 14px;
        color: #222;
    }

    .a-name {
        float: left;
    }

    .a-number {
        float: right;
    }

    .address-list p:nth-child(2) {
        font-size: 13px;
        color: #666;
        /*border-bottom: 1px solid #eee;*/
    }

    .address-list p:nth-child(3) span {
        font-size: 12px;
    }

    .address-state {
        float: left;
        color: #666;
    }

    .red-color {
        color: #d81300;
    }

    .com-address {
        color: #d81300;
    }

    .a-delete {
        float: right;
        color: #acacac;
    }

    .address-state img {
        width: 17px;
        height: 17px;
        margin-right: 10px;
    }

    .a-delete img {
        width: 17px;
        height: 17px;
        margin-right: 8px;
    }
    .clickable-false{
        background-color: #fafbfa;
    }
    div.clickable-false p span.a-name,
    div.clickable-false p span.a-number,
    div.clickable-false p i {
        color: #ddd !important;
    }
    /*常用地址end*/
</style>
