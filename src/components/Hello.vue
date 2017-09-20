<template>
  <div class="hello"> 
    <div class="top"><h1>AI</h1></div>
    <div class="warpper">
      <div class="content">
        <div class="chat-box" v-for="msg in reply">
          <li :class="{'robot':msg.type==='r','me':msg.type==='m'}">
          <span v-show="msg.type==='m'">{{address}}</span><p>{{msg.content}}</p>
          <a v-show="msg.url" :href="msg.url">{{msg.url}}</a>
          </li>
        </div>
      </div>
    </div>
   <div class="bottom">
     <input type="text" @keyup.enter="send" v-model.trim="inputmsg">  
   </div>
  </div>
</template>
<script>
import axios from 'axios';
export default {
  name: 'hello',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      reply:[],
      sendmsg:[],
      userid:this.$route.query.userid,
      inputmsg:'',
      address:'',
       post: null,
      error: null
    }
  },
  created(){  
      axios.post('/baidu')
        .then((response)=>{
          this.address = response.data.content.address_detail.city
        })
        .catch(function(err){
          console.log(err)
        })
      this.reply.push({type:'r',content:'hi,'+this.userid})
  },
 
  methods:{
    send:function(){
      this.reply.push({type:'m',content:this.inputmsg});
      axios.post('/api', {
        key: '82a2db25a9b5408a97e096619139236f',
        info: this.inputmsg,
        loc:this.address,
        userid:this.userid
      })
      .then((response) =>{
        console.log(response);
        this.reply.push({type:'r',content:response.data.text,url:response.data.url});
        this.inputmsg=''
      })
      .catch((error) =>{
        console.log(error);
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}
a {
  color: #42b983;
}
.top{
  display: flex;
  top: 0;
  width: 100%;
  height: 50px;
  line-height: 50px;
  border-bottom: 1px solid #aaa;
  background-color: rgba(2, 12, 22, 0.5);
}
.top>h1{
  font-size: 18px;
  color: #045858;
  margin: 0 auto;
  text-align: center;
}
.warpper{
  width: 100%;
  height:100vh;
  margin: 0 auto;
  position: relative;
  overflow: auto;
} 
.warpper .content{
  max-width: 80%;
}
.warpper .content .chat-box{
  display: block;
  margin: 5px ;
}
.warpper .content p{
  display: inline-block;
}
.warpper .content span{
  display: block;
  font-size: 12px;
  color: #050505;
}
.warpper .content li{
  display: inline-block;
  position: relative;
  left: 40px;
  padding: 4px;
  background-color: #A6DADC;
  border: none;
  border-radius: 4px;
}
.warpper .robot{
  text-align: left;
  border: 1px solid #aaa;
  border-radius: 5px;
}
.warpper .me{
  float: right;
  position: relative;
  left: -20px;

}

.chat-box{
  margin: 4px;
}
.robot:before{  
    position:absolute;  
    content:"\00a0";  
    width:0px;  
    height:0px;  
    border-width:8px 18px 8px 0;  
    border-style:solid;  
    border-color:transparent #A6DADC transparent transparent;  
    top:8px;  
    left:-18px;  
}  
.me:before{  
    position:absolute;  
    content:"\00a0";  
    display:inline-block;  
    width:0px;  
    height:0px;  
    border-width:8px 0px 8px 18px;  
    border-style:solid;  
    border-color:transparent transparent transparent #A6DADC;  
    right:-18px;  
    top:8px;  
                      
}  

.chat-box::after {
    content:" ";
    display:block;
    clear:both;
}

.bottom{
  width: 100%;
  height: 40px;
  position: fixed;
  bottom: -1px;
  background-color: rgba(0,0,0,.2);
}
.bottom>input{
  width: 80%;
  height: 24px;
  line-height: 24px;
  border-radius: 5px;
  border: none;
  padding: 2px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%,-50%); 
  transition: ease;
}


</style>
