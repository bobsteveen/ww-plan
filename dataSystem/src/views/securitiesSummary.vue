<template>
  <div id="securitiesSummary">
    <screenData v-on:fullConditions='loadData' :storeAttributes="attribute" :sogo="merchant" :shop="outlet"></screenData>
      <div class="overflow-height" v-loading="loading" element-loading-text="拼命加载中" element-loading-spinner="el-icon-loading" element-loading-background="rgba(250, 250, 250, 1)">
        <div class="box" v-if="dataPageList.length!=0">
          <div class="box-flex">
            <div class="box-list box-birder">指标</div>
            <div class="box-list">领券数</div>
            <div class="box-list">用券数</div>
            <div class="box-list">失效券数</div>
            <div class="box-list">
              <a class="box-font" v-bind:href="url" target="_blank">下线券数</a>
            </div>
            <div class="box-list">
              <a class="box-font" v-bind:href="url1" target="_blank">修改券数</a>
            </div>
            <div class="box-list">
              <a class="box-font" v-bind:href="url2" target="_blank">快到期券数(10天)</a>
            </div>
            <div class="box-list">新增关注券数</div>
            <div class="box-list">关注券领券数</div>
            <div class="box-list">关注券用券数</div>
            <div class="box-list">关注券用券率</div>
            <div class="box-list">关注券失效数</div>
            <div class="box-list">新增促活券数</div>
            <div class="box-list">促活券领券数</div>
            <div class="box-list">促活券用券数</div>
            <div class="box-list">促活券失效数</div>
            <div class="box-list">新增邻店券数</div>
            <div class="box-list">邻店券领券数</div>
            <div class="box-list">邻店券用券数</div>
            <div class="box-list">邻店券失效数</div>
            <div class="box-list">新增基础券数</div>
            <div class="box-list">基础券领券数</div>
            <div class="box-list">基础券用券数</div>
            <div class="box-list">基础券失效数</div>
            <div class="box-list">新增场景券数</div>
            <div class="box-list">场景券领券数</div>
            <div class="box-list">场景券领券数</div>
            <div class="box-list">场景券失效数</div>
          </div>
          <div class="box-left" v-if="page!=0" @click="leftList">
            <i class="el-icon-caret-left"></i>
          </div>
          <div class="box-flex" v-for="(items, index) in dataPageList" :key="index">
            <div class="box-list1 box-birder">{{items._key}}</div>
            <div class="box-list1" v-for="(itemss, indexs) in items._data" :key="indexs">{{itemss}}</div>
          </div>
          <div class="box-right" @click="rightList" v-if="pageright">
            <i class="el-icon-caret-right"></i>
          </div>
        </div>
      </div>
  </div>
</template>

<script>
import screenData from '../components/screenData.vue'
import axios from 'axios'
export default {
  name: 'securitiesSummary',
  data () {
    return {
      url: '#/referralsTicket',
      url1: '#/amendTicket',
      url2: '#/expiredCoupons',
      merchant: false,
      outlet: false,
      adminiStrative: [],
      attribute: false,
      dataList: [],
      shopList: [],
      dataPageList: [],
      tableList: false,
      page: 0,
      pageright: true,
      loading: true
    }
  },
  created () {},
  mounted () {},
  components: {
    'screenData': screenData
  },
  methods: {
    loadData (data) {
      this.loading = true
      this.page = 0
      console.log(data)
      this.shopList = []
      this.dataList = []
      this.dataPageList = []
      axios({method: 'post', url: this.$store.state.url + '/api/table_export', data: data})
        .then(response => {
          if (response.data) {
            this.loading = false
            this.$store.commit('increment', false)
          }
          console.log(response)
          console.log(response.data)
          response.data.splice(0, 1)
          this.dataList = response.data
          console.log(this.dataList)
          if (this.dataList.length <= 10) {
            this.pageright = false
            this.dataPageList = response.data
          } else {
            this.dataPageList = response.data.slice(this.page * 10, 10)
            this.pageright = true
            console.log(this.dataPageList)
          }
        })
        .catch(function (error) {
          console.log(error)
        })
    },
    rightList () {
      this.page += 1
      console.log(this.page)
      console.log(this.dataList)
      console.log(this.dataPageList)
      this.dataPageList = this.dataList.slice(this.page * 10, this.page * 10 + 10)
      if (this.dataPageList.length < 10 || (this.page + 1) * 10 >= this.dataList.length) {
        this.pageright = false
      } else {
        this.pageright = true
      }
    },
    leftList () {
      this.pageright = true
      this.page -= 1
      console.log(this.page)
      console.log(this.dataList)
      console.log(this.dataPageList)
      this.dataPageList = this.dataList.slice(this.page * 10, this.page * 10 + 10)
    }
  }
}
</script>
<style scoped>
#securitiesSummary {
  margin:15px 20px;
  font-size: 14px;
}
.box-font {
  color: #FF9800;
  cursor: pointer;
  text-decoration: underline;
}
.overflow-height {
  min-height: 300px;
}
.box {
  /* width: 90%; */
  /* margin: 15px; */
  /* min-width: 1000px; */
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  align-items: flex-start;
}
.box-flex {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  width: 9%;
  min-width: 90px;
}
.box-list {
  height: 40px;
  /* line-height: 40px; */
  /* min-width: 120px; */
  width: 100%;
  border-right: 1px solid #dddddd;
  text-align: center;
  /* word-wrap:break-word; */
  /* word-break:break-all; */
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  /* margin: 5px; */
}
.box-list1 {
  word-wrap: break-word;
  word-break:break-all;
  height: 40px;
  /* line-height: 40px; */
  /* min-width: 80px; */
  width: 100%;
  word-wrap:break-word;
  text-align: center;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  /* margin: 5px; */
}
.box-birder {
  border-bottom: 1px solid #dddddd;
}
.box-left {
  width: 2%;
  min-width: 10px;
  line-height: 40px;
  height: 40px;
  text-align: center;
  vertical-align: middle;
  border-bottom: 1px solid #dddddd;
  cursor: pointer;
  /* margin: 5px; */
}
.box-right {
  width: 2%;
  min-width: 10px;
  line-height: 40px;
  height: 40px;
  text-align: center;
  vertical-align: middle;
  border-bottom: 1px solid #dddddd;
  cursor: pointer;
  /* margin: 5px; */
}
.box-left:hover {
  opacity: 0.7;
  background: #dddddd;
}
.box-right:hover {
  opacity: 0.7;
  background: #dddddd;
}
.stores-table {
  width: 100%;
}
.stores-pagination {
  float: right;
  margin: 10px 0;
}
</style>
