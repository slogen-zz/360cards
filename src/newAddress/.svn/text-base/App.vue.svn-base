<template>
    <div id="app">
        <div class="adress-items">
            <ul>
                <li class="item clearfix">
                    <span>联系人&nbsp;&nbsp;&nbsp;&nbsp;</span>
                    <input type="text" v-model="username" placeholder="请输入姓名">
                </li>
                <li class="item clearfix">
                    <span>联系电话</span>
                    <input type="tel" v-model="telnum" @blur="checkTelOk" placeholder="请输入您的联系电话">
                </li>
                <li class="item clearfix">
                    <span>服务地址</span>
                    <div style="display: inline-block" @click.stop.prevent="redirect">
                        <input class="disabled-color" type="text" v-model="serveplace" disabled="disabled"
                               placeholder="请选择服务地址">
                    </div>

                    <i></i>
                </li>
                <li class="item clearfix">
                    <span>详细地址</span>
                    <input type="text" v-model="detailplace" placeholder="楼栋、单元、门牌号">
                </li>
            </ul>
        </div>
        <a href="javascript:;" class="confirm" :class="{activeBtn: btnClick}" @click="confirm">确定</a>

        <div class="font-num-shadow" v-if="alertFontNum">
            <div class="font-num">
                <div class="num-window">手机号格式有误</div>
            </div>
        </div>
        <!--<alertText :header-msg="changeCityMsg" :btn-text="changeCityText" v-if="chooseDisableAddress"></alertText>-->
       
        <bubble :moudel="BShow" :msg="msg"></bubble>
    </div>
</template>

<script type="text/ecmascript-6">
    import '../common/base-1.3.css'
    //    import alertText from 'common/alertText/alertText'
    import bubble from 'common/bubble/bubble'
    import publicMethod from '../common/publicMethod.js'
    export default{
        name: 'App',
        data(){
            return {
                username: '',
                telnum: '',
                serveplace: '',
                detailplace: '',
                btnClick: false,
                telOk: false,
                inputOk: false,
                alertFontNum: false,
                chooseDisableAddress: false,
                BShow: {
                    show: false
                },
                msg: ''
            }
        },

        components: {
//            alertText,
            bubble
        },
        methods: {
            redirect: function () {
                window.location.href = 'selectCell.html?token='+publicMethod.getToken()+'&cityId='+publicMethod.getCity();
            },
            // 验证输入框是否为空
            testVal: function () {
                if (this.username.length * this.telnum.length * this.serveplace.length * this.detailplace.length > 0) {
                    this.inputOk = true;
                } else {
                    this.inputOk = false;
                }
            },
            // 验证手机号
            checkTel: function () {
                (/^1[34578]\d{9}$/.test(this.telnum)) ? this.telOk = true : this.telOk = false;
            },
            // 验证按钮是否可点击
            checkClick: function () {
                this.checkTel();
                this.testVal();
                (this.telOk && this.inputOk) ? this.btnClick = true : this.btnClick = false;
            },
            // 手机号格式有误弹窗
            alertTel: function () {
                //检测手机号格式，调接口请干掉注释
                this.alertFontNum = true;
                var _this = this;
                setTimeout(function () {
                    _this.alertFontNum = false;
                }, 1500);
            },
            checkTelOk: function () {
                this.checkTel();
                this.telOk || this.alertTel();
            },
            ajax: function () {
                var address = {};
                address["token"] = publicMethod.getToken();
                address["userName"] = this.$data.username;
                address["userTel"] = this.$data.telnum;
                address["community"] = sessionStorage.getItem('community');
                address["detailAddress"] = this.$data.detailplace;
                address["province"] = sessionStorage.province;
                address["districtName"] = sessionStorage.districtName;

                if (sessionStorage.creatAddrCity) {
                    var currentCity = JSON.parse(sessionStorage.creatAddrCity);
                } else {
                    var currentCity = JSON.parse(localStorage.currentCity);
                }

                address["cityName"] = currentCity.cityName;
                address["cityId"] = currentCity.cityId;
                address["serverAddress"] = this.serveplace;
                address["longitude"] = sessionStorage.longitude;
                address["latitude"] = sessionStorage.latitude;


                this.$http.post(this.apiUrl.newAddress.addAddress, address).then(function (resp) {
                        if (resp.body.resultCode == 200) {
                            sessionStorage.removeItem('creatAddrCity');
                            sessionStorage.setItem('districtName', '');
                            sessionStorage.setItem('province', '');
                            sessionStorage.setItem('cityName', '');

                            sessionStorage.setItem('community', '');

                            //存储经纬度
                            sessionStorage.setItem("longitude", '');
                            sessionStorage.setItem("latitude", '');

                            window.location.href = 'addressList.html?token='+publicMethod.getToken()+'&cityId='+publicMethod.getCity();
                        } else{
                            this.msg = resp.body.message;
                            this.BShow.show = true;
                        }

                    }, function (resp) {
                        console.log("error");
                    }
                );
            },
            confirm: function () {
                if (this.btnClick) {
                    if (publicMethod.getToken()) {
                        var currentCity = JSON.parse(localStorage.currentCity);
                        var isAreas = {
                            'cityId': currentCity.cityId,
                            'longitude': sessionStorage.longitude,
                            'latitude': sessionStorage.latitude,
                            'token': publicMethod.getToken()
                        };
                        this.$http.get(this.apiUrl.addressList.isServiceArea, {params: isAreas}).then(function (res) {
                            if (res.body.resultCode == 200) {
                                if (res.body.data == 0) {
                                    this.msg = '您输入的地址不在服务区内';
                                    this.BShow.show = true;
                                    return false;
                                } else if (res.body.data == 1) {
                                    this.ajax();
                                }
                            } else {
                                this.msg = res.body.message;
                                this.BShow.show = true;
                            }
                        }, function (res) {

                        });
                    } else {
                        this.testShow.show = true;
                    }

                }
            }
        },
        watch: {
            username: function (val, oldVal) {
                sessionStorage.setItem('username', val);
                this.testVal();
                this.checkClick();
            }
            ,
            telnum: function (val, oldVal) {
                sessionStorage.setItem('telnum', val);
                this.testVal();
                this.checkTel();
                this.checkClick();
            }
            ,
            serveplace: function (val, oldVal) {
                sessionStorage.setItem('serveplace', val);
                this.testVal();
                this.checkClick();
            }
            ,
            detailplace: function (val, oldVal) {
                sessionStorage.setItem('detailplace', val);
                this.testVal();
                this.checkClick();
            }
        }
        ,
        created: function () {
            this.username = sessionStorage.getItem('username') ? sessionStorage.getItem('username') : '';
            this.telnum = sessionStorage.getItem('telnum') ? sessionStorage.getItem('telnum') : '';
            if(sessionStorage.getItem('cityName')&&sessionStorage.getItem('districtName')&&sessionStorage.getItem('community')){
                this.serveplace = sessionStorage.getItem('cityName') + sessionStorage.getItem('districtName') + sessionStorage.getItem('community');
            }
            this.detailplace = sessionStorage.getItem('detailplace') ? sessionStorage.getItem('detailplace') : '';

            this.testVal();
        }
        ,
        mounted: function () {
            this.checkTel();
            this.testVal();
            this.checkClick();
        }
    }
</script>

<style>
    .adress-items {
        padding: 0 10px;
        border-top: 1px solid #ddd;
        background-color: #fff;
    }

    .adress-items li {
        border-bottom: 1px solid #eee;
        position: relative;
    }

    .adress-items li:last-child {
        border-bottom: none;
    }

    .adress-items span {
        display: inline-block;
        margin-right: 8px;
        font-size: 14px;
        color: #222;
        line-height: 44px;
        vertical-align: middle;
    }

    .adress-items input {
        width: 7.03125rem;
        font-size: 14px;
        color: #222;
        outline: none;
        border: none;
        vertical-align: middle;
        line-height: 1.2em;
    }

    .confirm {
        display: block;
        margin: 30px 15px 0;
        /*width: 9.0625rem;*/
        height: 45px;
        background-color: #ddd;
        font-size: 19px;
        color: #acacac;
        line-height: 45px;
        border-radius: 2px;
        text-align: center;
    }

    .adress-items i {
        position: absolute;
        right: 0;
        top: 13px;
        width: 8px;
        height: 14px;
        background: url("./img/arrow-horizental.png") no-repeat left center;
        background-size: 100%;
    }

    a.activeBtn {
        background-color: #d81300;
        color: #fff;
    }

    /* --新建地址汇总 */
    /* 字数超过40个 */

    .font-num-shadow {
        position: fixed;
        left: 0;
        top: 0;
        width: 100%;
        height: 100%;
        display: table;
        background-color: rgba(50, 50, 50, 0.6);
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
        background-color: rgba(51, 51, 51, 0.7);
        border-radius: 0.078125rem;
        color: #fff;
    }

    .disabled-color:disabled {
        color: #222;
        opacity: 1;
        background: transparent;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
    }

    /* --字数超过40个 */
</style>
