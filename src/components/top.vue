<template>
	<div class="top-container">
		<div class="left-box">
			<div class="icon-wrapper">
				<span @click="$router.replace('/')" class="iconfont icon-home"></span>
				<span class="iconfont icon-sami-select"></span>
				<span class="iconfont icon-full-screen"></span>
			</div>
			<div class="history-wrapper">
				<span @click="$router.go(-1)" class="iconfont icon-arrow-lift"></span>
				<span @click="$router.go(1)" class="iconfont icon-arrow-right"></span>
			</div>
		</div>
		<div class="right-box">
			<el-input size="medium" @keyup.enter.native="toResult" placeholder="搜索" v-model.trim="query">
				<i slot="prefix" class="el-input__icon el-icon-search"></i>
			</el-input>
			<img src="@/assets/pic-1.jpg" alt="图片加载失败!" id="touxiang" @click="change">
			<el-card>
				<span id="float"> Open &nbsp;&nbsp;Up &nbsp;&nbsp; A  &nbsp;&nbsp;World &nbsp;&nbsp; Of &nbsp;&nbsp; Music&nbsp; &nbsp; With&nbsp;&nbsp;  Me . </span>
			</el-card>
		</div>
	</div>
</template>

<script>
	export default {
		name: 'top',
		data() {
			return {
				query: '',
				count:1
			}
		},
		methods: {
			toResult() {
				if (!this.query) {
					this.$message.warning('请输入查询内容')
				} else {
					this.$router.push(`/result?keywords=${this.query}`)
				}
			},
			change(){
				this.count++;
				if(this.count % 2){
					document.querySelector('.el-card').style.display = 'block';
				}else{
					document.querySelector('.el-card').style.display = 'none';
				}
			}
		},
	};
</script>

<style lang="scss" scoped>
	.el-card{
		width:45%;
		height:50px;
		position:absolute;
		top:0px;
		right:430px;
		background-color:#666;
		z-index:1999;
		opacity: .8;
		border:0;
		margin-top:5px;
	}
	#float{
		display:block;
		height:auto;
		color:lightsteelblue;
		font-size:18px;
		display:flex;
		justify-content: center;
		font-family:elephant;
		transition: all 2s;
	}
	.el-card__body{
		padding:0px;
	}
	.top-container{
		opacity: .9;
	}
</style>
