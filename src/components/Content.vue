<template>
  <div>
    <span class="header">Inventory Manager</span>
    <div class="search-bar">
    <div class="search-icon">
      <img alt="Migo" class="search-img" src="../assets/search.png">
    </div>
      <input
        type="search"
        v-on:change="filterTitle()"
        v-model="searchStr"
        class="search-box-input-wrapper"
        placeholder="Search for titles in inventory">
    </div>
    <div>
      <div class="table">
          <div class="table-head">
            <div class="item hiddenItem"></div>
            <div class="item id">ID</div>
            <div class="item titleName">Title Name</div>
            <div class="item type">Type</div>
            <div class="item season">Season</div>
            <div class="item id">Episode</div>
            <div class="item publish">Published</div>
            <div class="item programmable">Programmable</div>
            <!-- <div class="hiddenItem"></div> -->
          </div>
          <div class="table-content-wrapper"  v-for="(n, index) in entries" v-bind:key="entries[index].title_id">
              <div class="hiddenItem">
                <img alt="collapse" class="collapse-img" src="../assets/Collapse.png" v-if="entries[index].seasons.length>0">
              </div>
              <div class="item id">{{entries[index].title_id}}</div>
              <div class="item titleName">{{entries[index].title_name}}</div>
              <div class="item type">{{entries[index].content_type}}</div>
              <div class="item season">{{(entries[index].seasons.length)?entries[index].seasons.length:"-"}}</div>
              <div class="item id">{{entries[index].episode_count}}</div>
              <div class="item publish">{{entries[index].publish_timestamp}}</div>
              <div class="item programmable">
                <div class="switch-img" v-on:click="changeProgram(index)">
                  <img alt="switch-on" class="switch-on-img" src="../assets/switch-on.png" v-if="(entries[index].programmable)===true">
                  <img alt="switch-off" class="switch-off-img" src="../assets/switch-off.png" v-else>
                </div>
                <div class="content-type">{{(entries[index].content_type==="Movie")?"Single Movie":"All Episodes"}}</div>
              </div>
          </div>
      </div>
  </div>
  </div>
</template>

<script>

export default {
  data () {
    return {
      entries: [],
      searchStr: '',
      monthNames: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec']
    }
  },
  methods: {
    readData () {
      this.$axios.get('/api/request')
        .then(res => {
          this.resetDate()
          this.entries = res.data
          this.entries.forEach(element => {
            var d = new Date(element.publish_timestamp)
            element.publish_timestamp = this.monthNames[d.getMonth()] + ' ' + d.getDate() + ',' + d.getFullYear()
          })
        })
        .catch(err =>
          console.log(err)
        )
    },
    changeProgram (index) {
      this.$axios({
        method: 'put',
        url: '/api/putrequest',
        params: {index: index}
      })
        .then(res => {
          this.readData()
        })
    },
    filterTitle () {
      if (this.searchStr.length === 0) {
        this.readData()
      } else {
        this.$axios({
          method: 'put',
          url: '/api/filterRequest',
          params: {
            searchStr: this.searchStr
          }
        })
          .then(res => {
            this.resetDate()
            this.entries = res.data
          })
          .catch(err => console.log(err))
      }
    },
    resetDate () {
      this.entries = []
    }
  },
  created () {
    this.readData()
  }
}
</script>

<style scoped>
.header{
  position: absolute;
  width: 342px;
  height: 40px;
  left: 24px;
  top: 80.5px;

  font-family: Roboto;
  font-style: normal;
  font-weight: 500;
  font-size: 16px;
  line-height: 30px;
  /* or 187% */

  display: flex;
  align-items: center;
  letter-spacing: 0.15px;

  /* Black / High Emphasis */

  color: rgba(0, 0, 0, 0.87);
}
.search-bar {
  position: absolute;
  width: 419px;
  height: 40px;
  left: 24px;
  top: 136.5px;

  background: rgba(32, 32, 32, 0.05);
  border: 1px solid #B2B2B2;
  box-sizing: border-box;
  border-radius: 4px;
}
.search-icon {
  position: absolute;
  width: 24px;
  height: 24px;
  left: 17px;
  top: 8px;
}
.search-img {
  position: absolute;
  width: 18px;
  height: 18px;
  left: 12.5%;
  right: 14.63%;
  top: 12.5%;
  bottom: 14.63%;
}
.search-box-input-wrapper{
  background-color:transparent;
  border: none;
  position: absolute;
  width: 353.17px;
  height: 24px;
  left: 56px;
  top: 8px;
  font-family: Roboto;
  font-style: normal;
  font-weight: normal;
  font-size: 14px;
  line-height: 24px;
  display: flex;
  align-items: center;
  letter-spacing: 0.15px;
  color: #727272;
}
input:focus{
  outline: none;
}
.table{
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  padding: 0px;
  position: absolute;
  width: 95%;
  left: 24px;
  top: 192.5px;
  border: 1px solid #C3C3C2;
  border-radius: 4px;
  background: rgba(32, 32, 32, 0.05);
  font-family: Roboto;
  font-style: normal;
  font-weight: 500;
  font-size: 12px;
}
.table-head{
  position: static;
  height: 48px;
  left: 0px;
  top: 0px;
  background: #E8E8E7;
  box-shadow: inset 0px -1px 0px rgba(0, 0, 0, 0.2);
  border-radius: 4px 4px 0px 0px;
  flex: none;
  order: 0;
  display: flex;
  flex-direction: row;
  flex-grow: 0;
  margin: 0px 0px;
  width: 100%;
}

.table-content-wrapper{
  position: relative;
  height: 48px;
  left: 0px;
  top: 0px;
  background: #FFFFFF;
  box-shadow: inset 0px -1px 0px rgba(0, 0, 0, 0.2);
  border-radius: 4px 4px 0px 0px;
  flex: none;
  order: 0;
  display: flex;
  flex-direction: row;
  flex-grow: 0;
  margin: 0px 0px;
  width: 100%;
}
.table-content-wrapper:hover{
  background: rgba(78, 160, 221, 0.1);
}
.hiddenItem {
  flex: 0 0 32px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.item {
  display: flex;
  align-items: center;
  color: #292928;
}
.id {
  flex-basis: 56px;
}
.titleName {
  flex-basis: 320px;
}
.type {
  flex-basis: 72px;
}
.season {
  flex-basis: 48px;
}
.publish {
  flex-grow: 2;
}
.programmable {
  flex-basis: 256px;
}
.collapse-img {
  width: 10px;
  height: 8px;
}
.switch-img{
  width: 47px;
  height: 21px;
}
.switch-img.active {
  transition:all .2s ease;
}
.content-type{
  padding-left: 10px;
}

</style>
