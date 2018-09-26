<template>
	<div id="search-panel">
		<logo @getindex="getIndex"></logo>
		<div class="search-input">
			<input type="text" maxlength="40" v-model="keyword" @keyup="get($event)" @keydown.enter="search()" @keydown.down="selectDown()" @keydown.up.prevent="selectUp()" name="">
			<span class="search-reset" @click="clearInput()">&times;</span>
			<button class="search-btn" @click="search()">{{btnname}}</button>
			<div class="search-select">
				<transition-group name="itemfade" tag="ul">
					<li v-for="(val,index) in myData" class="search-select-option search-select-list" :class="{selectback:index==now}" :key="val" @mouseover="selectHover(index)" @click="selectClick(index)" @dblclick="search()">
						{{val}}
					</li>
				</transition-group>
			</div>
		</div>
	</div>
	
</template>
<script type="text/javascript">
	import logo from '@/components/search-logo/search-logo.vue'
	export default{
		name:'search-panel',
		data:function(){
			return{
				myData:[],
				keyword:'',
				searchIndex:0,
				now:-1,
				btnname:'360搜索',
				logoData:[
				{
					name: '360搜索',
					searchSrc: 'https://www.so.com/s?ie=utf-8&shb=1&src=360sou_newhome&q='
				}, {
					name: '百度搜索',
					searchSrc: 'https://www.baidu.com/s?ie=utf-8&f=8&rsv_bp=0&rsv_idx=1&tn=baidu&wd='
				}, {
					name: '搜狗搜索',
					searchSrc: 'https://www.sogou.com/web?query='
				}
				]

			}
		},
		methods:{
			get(ev){
				//如果按键时间是向上与向下则跳出ajax
				if(ev.keyCode == 38||ev.keyCode == 40){
					return
				}
				this.$http.jsonp('https://sug.so.360.cn/suggest?word=' + this.keyword + '&encodein=utf-8&encodeout=utf-8').then(function(res) {
					this.myData = res.data.s;
				})
				//ajax get回搜索数据
			},
			search(){
                //打开对应的搜索页面
                window.open(this.logoData[this.searchIndex].searchSrc+this.keyword)
            },
            selectDown(){
            	this.now++;
				//用户向下选择搜索的词条 当到最后一条时，在按下就返回第一个词条
				if (this.now==this.myData.length) {
					this.now=0
				}
				//搜索栏中的信息同步用户选择的搜索词条
				this.keyword=this.myData[this.now]
			},
			selectUp(){
				this.now--;
				if(this.now < 0){
					this.now=this.myData.length-1;
				}
				this.keyword=this.myData[this.now]
			},
			clearInput(){
				this.keyword='';
				//清空搜索栏中的字符
				this.$http.jsonp('https://sug.so.360.cn/suggest?word=' + this.keyword + '&encodein=utf-8&encodeout=utf-8').then(function(res) {
					this.myData = res.data.s;
				});
				//更新搜索词条返回的数据
				//this.myData=[]
			},
			selectHover(index){
				this.now=index;
			},
			selectClick(index){
				this.keyword= this.myData[index];
			},
			getIndex(index){
				//选择搜索引擎
				this.searchIndex=index;
				this.btnname=this.logoData[index].name;
			}
		},
		components: {
			logo
		},
	}

</script>
<style type="text/css">
.search-input{
	height: 45px;
	width: 600px;
	margin: 0 auto;
	margin-top: 10px;
	position: relative;
}
.search-input input{
	border: solid 1px #e4e4e4;
	width: 500px;
	height: 45px;
	font-size: 18px;
	float: left;
	padding-left: 10px;
	padding-right: 10px;
	overflow: hidden; 
}
.search-input .search-btn,.search-input .search-reset{
	user-select: none;
}
.search-btn{
	width: 100px;
	height: 45px;
	float: left;
	border:solid 1px mediumseagreen;
	background-color: mediumseagreen;
	color: #fff;
	font-size: 16px;
	font-weight: bold;
	cursor: pointer;
}
.search-reset{
	width: 21px;
	height: 21px;
	position: absolute;
	display: block;
	line-height: 1;
	text-align: center;
	cursor: pointer;
	font-size: 20px;
	right: 110px;
	top: 12px;
}
.search-select{
	position: absolute;
	top: 45px;
	width: 500px;
	z-index: 99999;
}
.search-select ul{
	list-style: none;
	text-align: left;
	margin: 0;
}
.search-select li{
	border: solid 1px #d4d4d4;
	border-top: none;
	border-bottom: none;
	background-color:#fff;
	width: 100%;
	 user-select: none;
}
.search-select-option{
	padding: 7px 10px;
}
.search-select-list{
	transition: all .3s;
}
.itemfade-enter,
.itemfade-leave-to{
	opacity: 0;
}
.itemfade-leave-active{
	position: absolute;
}
.selectback{
	background-color: #eee !important;
	cursor: pointer;
}
</style>