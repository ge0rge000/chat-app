<template>
  <div class="container clearfix">
   
 <div class="people-list" id="people-list">
      <div class="search">
        <input type="text" placeholder="search" />
        <i class="fa fa-search"></i>
      </div>
      <ul class="list">
          
        {{userList}}
       
         <li @click.prevent="selectUser(user.id)" class="clearfix" v-for="user in userList.names" :key="user.id">
          <div class="about">
            <div class="name">{{users.name}}</div>
            <div class="status">
              <i class="fa fa-circle online"></i> online
            </div>
          </div>
        </li>
        
        
        <li class="clearfix">
          <img src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/195612/chat_avatar_04.jpg" alt="avatar" />
          <div class="about">
            <div class="name">Erica Hughes</div>
            <div class="status">
              <i class="fa fa-circle online"></i> online
            </div>
          </div>
        </li>
        
        <li class="clearfix">
          <img src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/195612/chat_avatar_05.jpg" alt="avatar" />
          <div class="about">
            <div class="name">Ginger Johnston</div>
            <div class="status">
              <i class="fa fa-circle online"></i> online
            </div>
          </div>
        </li>
        
        <li class="clearfix">
          <img src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/195612/chat_avatar_06.jpg" alt="avatar" />
          <div class="about">
            <div class="name">Tracy Carpenter</div>
            <div class="status">
              <i class="fa fa-circle offline"></i> left 30 mins ago
            </div>
          </div>
        </li>
        
        <li class="clearfix">
          <img src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/195612/chat_avatar_07.jpg" alt="avatar" />
          <div class="about">
            <div class="name">Christian Kelly</div>
            <div class="status">
              <i class="fa fa-circle offline"></i> left 10 hours ago
            </div>
          </div>
        </li>
        
        <li class="clearfix">
          <img src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/195612/chat_avatar_08.jpg" alt="avatar" />
          <div class="about">
            <div class="name">Monica Ward</div>
            <div class="status">
              <i class="fa fa-circle online"></i> online
            </div>
          </div>
        </li>
        
        <li class="clearfix">
          <img src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/195612/chat_avatar_09.jpg" alt="avatar" />
          <div class="about">
            <div class="name">Dean Henry</div>
            <div class="status">
              <i class="fa fa-circle offline"></i> offline since Oct 28
            </div>
          </div>
        </li>
        
        <li class="clearfix">
          <img src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/195612/chat_avatar_10.jpg" alt="avatar" />
          <div class="about">
            <div class="name">Peyton Mckinney</div>
            <div class="status">
              <i class="fa fa-circle online"></i> online
            </div>
          </div>
        </li>
      </ul>
    </div>
    
    <div class="chat">
      <div class="chat-header clearfix">
        <img src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/195612/chat_avatar_01_green.jpg" alt="avatar" />
        
        <div class="chat-about">
          <div v-if="userMessage.user" class="chat-with">{{userMessage.user.name}}</div>
          <div class="chat-num-messages">already 1 902 messages</div>
        </div>
        <i class="fa fa-star"></i>
      </div> <!-- end chat-header -->
      
      <div class="chat-history " v-chat-scroll>
        <ul>
          <li class="clearfix" v-for="message in userMessage.messages" :key="message.id">
            <div class="message-data align-right">
              <span class="message-data-time" >10:10 AM, Today</span> &nbsp; &nbsp;
              <span class="message-data-name" >{{message.user.name}}</span> <i class="fa fa-circle me"></i>
              
            </div>
            <div class="message float-right ${message.user.id==userMessage.user.id ? 'other-message':'my message'}">
                {{message.message}}
            </div>
          </li>
          
          
        </ul>
        
      </div>  
      
      <div class="chat-message clearfix">
        <textarea v-if="userMessage.user" @keydown="typeingEvent" @keydown.enter="sendMessage" v-model="message" name="message-to-send" id="message-to-send" placeholder ="Type your message" rows="3"></textarea>
         <textarea v-else disabled name="message-to-send" id="message-to-send" placeholder ="Type your message" rows="3"></textarea>
                
        <i class="fa fa-file-o"></i> &nbsp;&nbsp;&nbsp;
        <i class="fa fa-file-image-o"></i>
        
        <button>Send</button>

      </div> <!-- end chat-message -->
      
    </div> <!-- end chat -->

  
    
  </div> <!-- end container -->
</template>

<script>
export default {
 mounted(){
   
   Echo.private('chat.${authuser.id}')
    .listen('MessageSend', (e) => {
this.selectUser(e.message.user.from);

       // console.log(e.message.message);
    });
$this.store.dispatch('userList');
Echo.private('typingevent')
.listenForWhisper('typing',(e)=>{
console.log(e);
});


},
 data(){
return{
message:'',
}
    },
computed:{
userList(){
    return this.$store.getters.userList
},
userMessage(){
        return this.$store.getters.userMessage

}
},
created(){

},
methods:{
selectUser(userId){
      $this.store.dispatch('userMessage,userId');
  },
  sendMessage(e){
   e.preventDefault();
   if(this.message!=''){
     axios.post('/sendmessage',{message:this.message,user_id:this.userMessage.user.id})
     .then(response=>{
       this.selectUser(this.userMessage.user.id);
     })
     this.message='';
      
   }
   typeingEvent() 
     Echo.private('typingevent')
     .whisper('typing',{
      'user':authuser,
      'typing':this.message
     });
   
  
  }
}
}
</script>

<style>
.people -list ul{overflow-y :scroll!important}
</style>