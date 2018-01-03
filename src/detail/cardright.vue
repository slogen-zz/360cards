<template>
	<div class="360card">
		<div class="intro" v-for="(rights,indexs) in rightsInfo.firstRightsPackages">
	  		<h3>
	  			<span>{{rights.rightsName}}</span> 
	  			<span class="rights-time">
	  			(<span v-if="rights.limitCount == -1">不限次,</span>
	  			<span v-else>可任选{{rights.limitCount}}次服务,</span>
	  			<span>已使用{{rights.usedCount}}次</span>)
	  			</span>
	  		</h3>
	    	<div class="introcell">
	    		<div :ref="'cell'+secondRight.rightsId" v-for="(secondRight,index) in rights.secondRightsPackages">
		    		<h4 @click="introCellDetail ('cell'+secondRight.rightsId,indexs,index)">
		    			<span>{{secondRight.rightsName}}</span>
		    			<span>
		    				<span v-if="secondRight.limitCount == -1">不限次</span><span v-else>任选{{secondRight.limitCount}}次</span><i class="triangle-down"></i>
		    			</span>
		    		</h4>
		    		<ul class="hide">
		    			<li v-for="rightsItem in secondRight.rightsItems">
		    				<span>{{rightsItem.itemName}}</span>
		    				<span>({{rightsItem.usedCount}}次)</span>
		    			</li>
		    		</ul>
	    		</div>
	    	</div>
  		</div>
	</div>
</template>
<script>
	var arr = []
	export default {
		props:['rightsInfo'],
		methods: {
			getarr (){
				var _this = this
	  			let length = this.rightsInfo.firstRightsPackages.length
	  			for(let i = 0;i < length;i++){
	  				for(let j = 0;j<_this.rightsInfo.firstRightsPackages[i].secondRightsPackages.length;j++){
	  					arr.push(_this.rightsInfo.firstRightsPackages[i].secondRightsPackages[j].rightsId)
	  				}
	  			}
	  			return arr
			},
			introCellDetail (cell,indexs,index) {
	  		  	let elI = this.$refs[cell][0].querySelector('h4 i')
	  			if (elI.className === 'triangle-down') {
	  				elI.className = 'triangle-up'
	  			} else {
	  				elI.className = 'triangle-down'
	  			}
	  			let elUl = this.$refs[cell][0].querySelector('ul')
	  			if (elUl.className === 'hide') {
	  				elUl.className = 'show'
	  			} else {
	  				elUl.className = 'hide'
	  			}
	  			let oindex = parseInt(cell.substr(4))
	  			var c = arr.filter(function(item){
					return item != oindex
				})
	  			for (let i = 0; i < c.length; i++) {
	  				this.$refs['cell' + c[i]][0].querySelector('h4 i').className = 'triangle-down'
	  				this.$refs['cell' + c[i]][0].querySelector('ul').className = 'hide'
	  				
	  			}
	    	}
	    	
		},
		updated () {
    		this.getarr()
//  		console.log(arr)
    	}
	}
</script>
<style>
	.rights-time{
		font-size: 13px;
	}
</style>