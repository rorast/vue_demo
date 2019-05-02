<template>
  <section class="m-istyle">
    <dl @mouseover="over">
      <dt>有格調</dt>
      <dd
        :class="{active:kind==='all'}"
        kind="all"
        keyword="景點">全部</dd>
      <dd
        :class="{active:kind==='part'}"
        kind="part"
        keyword="美食">約會聚餐</dd>
      <dd
        :class="{active:kind==='spa'}"
        kind="spa"
        keyword="麗人">麗人SPA</dd>
      <dd
        :class="{active:kind==='movie'}"
        kind="movie"
        keyword="電影">電影演出</dd>
      <dd
        :class="{active:kind==='travel'}"
        kind="travel"
        keyword="旅游">品質出游</dd>
    </dl>
    <ul class="ibody">
      <li
        v-for="item in cur"
        :key="item.title">
        <el-card
          :body-style="{ padding: '0px' }"
          shadow="never">
          <img
            :src="item.img"
            class="image">
          <ul class="cbody">
            <li class="title">{{ item.title }}</li>
            <li class="pos"><span>{{ item.pos }}</span></li>
            <li class="price">NT<em>{{ item.price }}</em><span>/起</span></li>
          </ul>
        </el-card>
      </li>
    </ul>
  </section>
</template>
<script>
export default {
  data: () => {
    return {
      kind: 'all',
      list: {
        all: [
          {title:'神廟',
          img:'http://travelwithoutquit.com/blog/wp-content/uploads/2017/09/paris-1-599x350.jpg',
          pos:'法國',
          price:'45000'},
          {title:'璀璨星空SKY BAR',
          img:'https://pic.pimg.tw/fanyu58/1453369886-426040040_l.jpg',
          pos:'高雄',
          price:'10000'},
          {title:'沖繩無邊際泳池海景飯店',
          img:'https://cdn2.ettoday.net/images/3446/d3446571.jpg',
          pos:'沖繩',
          price:'15690'},
          {title:'情侶約會浪漫地點',
          img:'http://pic.pimg.tw/funtrip0103/652a829cfc425d391255e198c821cd9b_n.jpg',
          pos:'新竹',
          price:'1280'},
          {title:'復仇者聯盟4',
          img:'https://img.4gamers.com.tw/ckfinder/images/TangBao/1904/25-a4-01.jpg?versionId=BQwJRgOTQwmNx91WOElXMJaE9f9mr.9T',
          pos:'大遠百',
          price:'500'},
          {title:'艦隊收藏',
          img:'http://i.imgur.com/XDVeMZY.jpg',
          pos:'義大世界',
          price:'680'},
          {title:'維也納荷夫堡新舊皇宮博物館',
          img:'https://i.ytimg.com/vi/jgycXeI2v30/maxresdefault.jpg',
          pos:'奧地利',
          price:'33680'},
        ],
        part: [
          {title:'璀璨星空SKY BAR',
          img:'https://pic.pimg.tw/fanyu58/1453369886-426040040_l.jpg',
          pos:'高雄',
          price:'10000'},
          {title:'美麗華',
          img:'https://bestdateidea.files.wordpress.com/2016/12/5069da2a-bee6-4dc6-a4c5-118fc9e94522_g.jpg?w=630',
          pos:'台北',
          price:'7680'},
          {title:'情侶約會浪漫地點',
          img:'http://pic.pimg.tw/funtrip0103/652a829cfc425d391255e198c821cd9b_n.jpg',
          pos:'新竹',
          price:'1280'},],
        spa: [{title:'百夫長SPA飯店',
          img:'https://i2.wp.com/www.lawrencenas.com/Adrianne/Blog/201712JP/CH/014.jpg',
          pos:'上野',
          price:'4800'},
          {title:'烏來溫泉',
          img:'https://blog.hotelscombined.com.tw/wp-content/uploads/2018/03/feature-6.jpg',
          pos:'烏來',
          price:'7680'},
          {title:'沖繩無邊際泳池海景飯店',
          img:'https://cdn2.ettoday.net/images/3446/d3446571.jpg',
          pos:'沖繩',
          price:'15690'},],
        movie: [
          {title:'復仇者聯盟4',
          img:'https://img.4gamers.com.tw/ckfinder/images/TangBao/1904/25-a4-01.jpg?versionId=BQwJRgOTQwmNx91WOElXMJaE9f9mr.9T',
          pos:'大遠百',
          price:'500'},
          {title:'美女與野獸',
          img:'https://hips.hearstapps.com/amv-prod-elletw.s3.amazonaws.com/new-dossier/01_1019.jpg?resize=480:*',
          pos:'美麗華',
          price:'450'},
          {title:'艦隊收藏',
          img:'http://i.imgur.com/XDVeMZY.jpg',
          pos:'義大世界',
          price:'680'},
        ],
        travel: [{title:'神廟',
          img:'http://travelwithoutquit.com/blog/wp-content/uploads/2017/09/paris-1-599x350.jpg',
          pos:'法國',
          price:'45000'},
          {title:'大阪城天守閣',
          img:'https://travel.ulifestyle.com.hk/cms/spots_photo/original/20160203170922_0_184954251ed435dd4_m.jpg',
          pos:'日本',
          price:'28000'},
          {title:'維也納荷夫堡新舊皇宮博物館',
          img:'https://i.ytimg.com/vi/jgycXeI2v30/maxresdefault.jpg',
          pos:'奧地利',
          price:'33680'},]
      }
    }
  },
  computed: {
    cur: function () {
      return this.list[this.kind]
    }
  },
  async mounted(){
    let self=this;
    let {status,data:{count,pois}}=await self.$axios.get('/search/resultsByKeywords',{
      params:{
        keyword:'景点',
        city:self.$store.state.geo.position.city
      }
    })
    if(status===200&&count>0){
      let r= pois.filter(item=>item.photos.length).map(item=>{
        return {
          title:item.name,
          pos:item.type.split(';')[0],
          price:item.biz_ext.cost||'暂无',
          img:item.photos[0].url,
          url:'//abc.com'
        }
      })
      self.list[self.kind]=r.slice(0,9)
    }else{
      self.list[self.kind]=[]
    }
  },
  methods: {
    over: async function (e) {
      let dom = e.target
      let tag = dom.tagName.toLowerCase()
      let self = this
      if (tag === 'dd') {
        this.kind = dom.getAttribute('kind')
        let keyword = dom.getAttribute('keyword')
        let {status,data:{count,pois}}=await self.$axios.get('/search/resultsByKeywords',{
          params:{
            keyword,
            city:self.$store.state.geo.position.city
          }
        })
        if(status===200&&count>0){
          let r= pois.filter(item=>item.photos.length).map(item=>{
            return {
              title:item.name,
              pos:item.type.split(';')[0],
              price:item.biz_ext.cost||'暂无',
              img:item.photos[0].url,
              url:'//abc.com'
            }
          })
          self.list[self.kind]=r.slice(0,9)
        }else{
          self.list[self.kind]=[]
        }
      }
    }
  },

}
</script>
<style lang="scss">
    @import "@/assets/css/index/artistic.scss";
</style>
