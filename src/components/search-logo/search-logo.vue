<template>
	<div class="mian-logo">
		<img :src="logodata[selectedNow].img" @click="toggleFlag"/>
		<span class="logoList-arrow" @click="toggleFlag"></span>
		<transition name="logofade">
			<ul v-show="logoListFlag&&mouseLeaveFlag" class="logoList" @mouseleave="mouseLeaveList">
				<li v-for='(item,index) in logodata' class="logoItem" @mouseover="logoListHover(index)" :class="{selectback:index==logoNow}" @click="logoSelected(index)">
					<img :src="item.img"/>
				</li>
			</ul>
		</transition>


		
	</div>
</template>
<script type="text/javascript">
	export default{
		name:'search-logo',
		data:function(){
			return{
				logoListFlag:false,
				mouseLeaveFlag:false,
				logoNow:-1,
				selectedNow:0,
				logodata:[
				{
					img:require('@/assets/360_logo.png')
				},{
					img:require('@/assets/baidu_logo.png')
				},{
					img:require('@/assets/sougou_logo.png')
				}
				],
			}
		},
		methods:{
			toggleFlag(){
				this.logoListFlag=!this.logoListFlag;
				this.mouseLeaveFlag = !this.mouseLeaveFlag;
			},
			logoSelected(index){
				this.selectedNow = index;
				this.logoListFlag=false;
				this.$emit('getindex', this.selectedNow);
			},
			logoListHover(i){
				this.logoNow=i;
			},
			mouseLeaveList(){
				this.logoListFlag=false;
				this.mouseLeaveFlag=false;
			}
		}


	}
</script>
<style type="text/css">
.mian-logo{
	width: 600px;
	height: 140px;
	position: relative;
}
.mian-logo img{
	display: block;
	margin: 0 auto;
	user-select: none;
	cursor: pointer
}
.logoList-arrow{
	position: absolute;
	width: 0;
	height: 0;
	border: solid 8px; 
	border-color: #000 transparent transparent transparent;
	right: 100px;
	top: 66px;
	cursor: pointer;
}
.logoList{
	position: absolute;
	top: 100%;
	width: 200px;
	left: 50%;
	margin-left: -100px;
	z-index: 999999;
	border: solid 1px #d4d4d4;
	list-style: none;
}
.logoList li{
	width: 100%;
	height: 80px;
	background-color: #fefefe;
	line-height: 1;
	padding-top:1px;  
}
.logoList li img{
	width: 100%;
	margin-top: 10px;
}

.selectback{
	background-color:#eee !important;
	cursor: pointer;
}
.logofade-enter-active,
.logofade-leave-active{
	-webkit-transition: all .5s;
	-o-transition: all .5s;
	transition: all .5s;
}
.logofade-enter,
.logofade-leave-to{
	opacity: 0;
	transform: translateY(20px);
}

</style>