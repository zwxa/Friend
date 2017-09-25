<template>
  <div class="login">
  	<div class="box">
  		<h1>叫啥玩意？</h1>
		<input type="text" v-model.trim="userid" @keyup.enter="login">
	</div>
  </div>
</template>

<script>
export default {
	data () {
		return {
			userid:''
		}
	},
	created(){
		
	},
	methods:{
		login(){
			this.$router.push({path:'Hello',query:{userid:this.userid}});
		}
	}
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
}
.login{
position: fixed;
top:0;
bottom: 0;
left: 0;
right: 0;
background-color: #000;
opacity: .8;
}
.login .box{
	position: absolute;
	top: 50%;
	left: 50%;
	transform: translate(-50%,-50%);
	color:#fff;
}
.login .box input{
	height: 24px;
	line-height: 24px;
	border-radius: 4px;
	margin-top: 6px;
	padding: 0 10px;
}
</style>
