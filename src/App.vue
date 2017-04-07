<template>
  <v-head :seller="seller"></v-head>
  <div class="tab border-1px">
    <div class="tab-item"><a v-link="{ path: '/goods' }">商品</a></div>
    <div class="tab-item"><a v-link="{ path: '/ratings' }">评论</a></div>
    <div class="tab-item"><a v-link="{ path: '/seller' }">商家</a></div>
  </div>
  <router-view :seller="seller"></router-view>
</template>
<script type="text/ecmascript-6">
import header from 'components/header/header';
const ERR_OK = 0;
  export default{
      data() {
          return {
              seller: {}
          };
      },
      created() {
          this.$http.get('/api/seller').then((response) => {
              response = response.body;
              if (response.errno === ERR_OK) {
                  this.seller = response.data;
              }
          });
      },
       components: {
        'v-head': header
    }
  };
</script>
<style>
  .border-1px:after{
    content: '';
    width: 100%;
    position: absolute;
    left: 0;
    bottom: 0;
    border-top:1px solid rgba(7,17,27,0.1);
  }
  .tab{
    display: flex;
    width: 100%;
    height: 40px;
    line-height: 40px;
    position: relative;
  }
  .tab .tab-item{
    text-align: center;
    flex: 1;
  }
  .tab .tab-item a{
    display: block;
    text-decoration: none;
    font-size: 14px;
    color: rgb(77,85,93);
  }
  .tab .tab-item a.v-link-active{
    color: rgb(240,20,20);
  }
</style>
