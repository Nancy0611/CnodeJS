<template>
  <div class="secDiv" v-on:scroll="scrollMethod" v-loading="loading">
    <div v-for='item in content' :key="item.id">
      <router-link :to="{name:'UserRoute',params:{name:item.author.loginname}}"><img :src="item.author.avatar_url" :title="item.loginname"/></router-link>
      <div class="textDiv">
        <router-link :to="{name:'ArticleRoute',params:{id:item.id}}">{{item.title}}</router-link>
        <div class="stuff">
          <span>回复：{{item.reply_count}}</span>
          <span>创建于：{{dealCommentTime(item.create_at)}}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MainSection',
  data () {
    return {
      content: [],
      item: {
        create_at: '2017-02-22T11:32:43.547Z'
      },
      limit: 0,
      loading: true
    }
  },
  methods: {
    scrollMethod () {
      const sumH = document.documentElement.scrollHeight
      const viewH = document.documentElement.clientHeight
      const scrollH = document.documentElement.scrollTop || document.body.scrollTop
      if (viewH + scrollH === sumH) {
        this.getData()
      }
    },
    getData () {
      this.limit += 10
      this.$http({
        url: 'https://cnodejs.org/api/v1/topics',
        method: 'get',
        params: {
          page: 1,
          limit: this.limit,
          mdrender: 'false'
        }
      }).then((res) => {
        this.content = res.body.data
        console.log(this.content)
      }).catch((res) => {
        console.log('MainSec.vue:', res)
      })
    },
    dealCommentTime (time) {
      return String(time).match(/.{16}/)[0].replace(/.{2}/, '').replace(/[T]/, ' ')
    }
  },
  mounted () {
    window.addEventListener('scroll', this.scrollMethod)
  },
  computed: {
    dealTime () {
      return String(this.item.create_at).match(/.{10}/)[0]
    }
  },
  created () {
    this.getData()
  },
  watch: {
    content (val) {
      if (val) {
        console.log(val)
        this.loading = false
      }
    }
  }
}
</script>
<style scoped>
  .secDiv{
    width: 60%;
    background: #f9fafc;
    border: 1px solid #ddd;
    display: flex;
    flex-direction: column;
    font-size: 22px;
    padding: 2rem;
    min-height: 40rem;
  }
  a{
    text-decoration: none;
  }
  .secDiv>div{
    display: flex;
    justify-content: flex-start;
    align-items: center;
    margin: 0.5rem 0;
    border-bottom: 2px solid #C0CCDA;
    padding-bottom: 1rem;
  }
  .secDiv>div img{
    width: 4rem;
    height: 4rem;
    margin-right: 2rem;
  }
  .textDiv{
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    justify-content: flex-start;
    width: 100%;
  }
  .textDiv a{
    color: black;
    font-size: 25px;
  }
  .textDiv a:visited{
    color: grey;
  }
  .stuff span:first-child{
    font-size: 17px;
    margin-right: 2rem;
    color: #8492A6;
  }
</style>
