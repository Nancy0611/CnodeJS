<template>
  <div class="secDiv">
    <div class="profile" v-loading="loading">
      <div>
        <img :src="userInfo.avatar_url" :title="userInfo.loginname"/>
        <span>{{userInfo.loginname}}</span>
      </div>
      <p><icon name="score" scale="4"></icon><span>积分：</span>{{userInfo.score}}</p>
      <p><icon name="github" scale="4"></icon><span>Github:</span>https://github.com/{{userInfo.githubUsername}}</p>
      <p ><icon name="time" scale="4"></icon><span>注册时间:</span> {{dealCommentTime(userInfo.create_at)}}</p>
    </div>
    <div class="recentReplies" v-loading="loading">
      <p>最近参与的话题</p>
      <template v-for="(item,index) in userInfo.recent_replies">
        <div v-if="index<4 && item" :key="item.id">
          <router-link :to="{name:'UserRoute',params:{name:item.author.loginname}}">
            <img :src="item.author.avatar_url" :title="item.author.loginname">
          </router-link>
          <router-link :to="{name:'ArticleRoute',params:{id:item.id}}"><p class="userTitle">{{item.title}}</p></router-link>
        </div>
      </template>
    </div>
    <div class="recentTopics" v-loading="loading">
      <p>最近创建的话题</p>
      <template v-for='(item,index) in userInfo.recent_topics'>
        <div v-if="index<5 && item" :key="item.id">
          <img :src="item.author.avatar_url" :title="item.author.loginname">
          <router-link :to='{name:"ArticleRoute",params:{id:item.id}}'>
            <p class="userTitle">{{item.title}}</p>
          </router-link>
        </div>
      </template>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      userInfo: {
        create_at: '2017-04-130000'
      },
      loading: true
    }
  },
  created () {
    this.$http({
      url: `https://cnodejs.org/api/v1${this.$route.path}`,
      method: 'get'
    }).then((res) => {
      this.userInfo = res.body.data
    }).catch((res) => {
      console.log('UserCom.vue:', res)
    })
  },
  methods: {
    dealCommentTime (time) {
      return String(time).match(/.{10}/)[0].replace(/.{2}/, '').replace(/[T]/, ' ')
    }
  },
  beforeRouteUpdate (to, from, next) {
    console.log(to)
    this.$http({
      url: `https://cnodejs.org/api/v1${to.path}`,
      method: 'get'
    }).then((res) => {
      this.userInfo = res.body.data
    }).catch((res) => {
      console.log('UserCom.vue:', res)
    })
    next()
  },
  watch: {
    userInfo (val) {
      if (val) {
        this.loading = false
      }
    }
  }
}
</script>

<style scoped>
  .secDiv{
    padding: 0;
    box-sizing: border-box;
    border: none;
  }
  .profile{
    padding: 1rem;
    background: #EFF2F7;
    border-radius: 0.3rem;
  }
  .profile div{
    margin-bottom: 1.5rem;
  }
  .profile div span{
    font-size: 25px;
    color: black;
    margin-left: 1rem;
  }
  .profile p{
    display: flex;
    align-items: center;
    color: grey;
    margin: 0.5rem 0;
  }
  .profile p span{
    margin: 0 0.5rem;
    color: black;
  }
  .profile svg{
    color: black;
    margin-left:0.2rem;
  }
  .recentTopics img,
  .recentReplies img{
    width:5rem;
    height: 5rem;
  }
  .recentTopics>p,
  .recentReplies>p{
    margin-bottom: 1rem;
    color: black;
    padding-bottom: 1.5rem;
  }
  .userTitle{
    font-size: 25px;
  }
  .recentTopics{
    padding:1rem;
    background: #d3dce6;
    border-radius: 0.3rem;
  }
  .recentReplies{
    background: #e5e9f2;
    padding: 1rem;
    margin: 1rem 0;
    border-radius: 0.3rem;
  }
  .recentReplies div,
  .recentTopics div{
    display: flex;
    align-items: center;
    margin: 1rem 0;
    border-bottom: 2px solid #c0ccda;
    padding-bottom: 1rem;
  }
  .recentTopics div{
    border-bottom: 2px solid #99a9bf;
  }
  .recentTopics div img,
  .recentReplies div img{
    margin-right: 1rem;
  }
</style>
