import Vue from 'vue';
import VueResource from 'vue-resource';
import apiUrl from './commonurl';
let publicMethod = {}
//获取token
publicMethod.getToken = () => {
	//if (process.env.NODE_ENV === 'development') {
		//return '85262d56537441958a3c269f215d7da7'
	//} else {
		return publicMethod.getQuery('token')
	//}
}
//获取city
publicMethod.getCity = () => {
	//if (process.env.NODE_ENV === 'development') {
		//return '2077'
	//} else {
		return publicMethod.getQuery('cityId')
	//}
}

//获取地址信息所需要的数据
publicMethod.setAddrData = () => {
	let actToken = publicMethod.getToken()
    let getCity = publicMethod.getCity()
    if (getCity === null) {
    	let usedData = {"cityNo":"LF","cityName":"廊坊","cityPinyin":"LF","latitude":"39.543520000000000","cityId":2068,"longitude":"116.690340000000000"}
		usedData.latitude = parseFloat(usedData.latitude)
		usedData.longitude = parseFloat(usedData.longitude)
		localStorage.setItem('currentCity', JSON.stringify(usedData))
    	return
    }
    Vue.http.get(apiUrl.searchCity.listAllCity).then((response) => {
  	response = response.body
  	if (response.resultCode === 200) {
  		let data = response.data
  		let usedData = data.filter((value) => {
  			return value.cityId === parseInt(getCity)
  		})
  		if (usedData.length === 0) {
  			let usedData = {"cityNo":"LF","cityName":"廊坊","cityPinyin":"LF","latitude":"39.543520000000000","cityId":2068,"longitude":"116.690340000000000"}
			usedData.latitude = parseFloat(usedData.latitude)
			usedData.longitude = parseFloat(usedData.longitude)
			localStorage.setItem('currentCity', JSON.stringify(usedData))
	    	return
  		} else {
  			usedData = usedData[0]
	  		usedData.latitude = parseFloat(usedData.latitude)
	  		usedData.longitude = parseFloat(usedData.longitude)
	  		localStorage.setItem('currentCity', JSON.stringify(usedData))
  		}	
  	} else {
  		let usedData = {"cityNo":"LF","cityName":"廊坊","cityPinyin":"LF","latitude":"39.543520000000000","cityId":2068,"longitude":"116.690340000000000"}
		usedData.latitude = parseFloat(usedData.latitude)
		usedData.longitude = parseFloat(usedData.longitude)
		localStorage.setItem('currentCity', JSON.stringify(usedData))
    	return
  	}
  })
  if (actToken === null) {
    return
  }
  sessionStorage.setItem('actToken', actToken)
}
publicMethod.login = () => {
  if (!publicMethod.getQuery('token')) {
    try{
      ecej.login();
    }catch(er){
      alert("调取登录失败")
    }
  }
}
publicMethod.getQuery = (key) => {
	let query = location.search.substr(1);
	let reg = new RegExp('(^|&)'+key+'=([^&]+)(&|$)');
  if (!reg.exec(query)) {
    return null
  }
	return decodeURIComponent(reg.exec(query)[2]);
}

publicMethod.getCookie = (name) => {
  var arr,reg=new RegExp("(^| )"+name+"=([^;]*)(;|$)");
  if (arr=document.cookie.match(reg)) {
    return unescape(arr[2]);
  } else {
    return null;
  }
}
export default publicMethod