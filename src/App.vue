<template>
<div class="container" id="app">
  <div class="header">
    <span class="header-title"><img src="https://78.media.tumblr.com/5507fb6ca308f2b4984cf59991e4cf36/tumblr_ozedr6exTk1wbgtmko1_1280.png" alt=""></span>
    <span class="header-img"><img src="https://78.media.tumblr.com/daf57ca8d105b4d6507846ecbaa3bbee/tumblr_oxlw3eWQi91wbgtmko1_1280.png" alt=""></span>
  </div>
  <div class="button-list">
    <div class="button-list-all list-item" v-bind:style="{background: btnAll.bg, color:btnAll.color}" v-on:click="all"><span class="circle circle-all"></span><span class="button-text">All</span></div>
    <div class="button-list-online list-item" v-bind:style="{background: btnOnline.bg, color:btnOnline.color}" v-on:click="online"><span class="circle circle-online"></span><span class="button-text">Online</span></div>
    <div class="button-list-offline list-item" v-bind:style="{background: btnOffline.bg, color:btnOffline.color}" v-on:click="offline"><span class="circle circle-offline"></span><span class="button-text">Offline</span></div>
  </div>
  
  <div class="content" v-for="streamer in streamers">
    <div class="online-content content-list"  v-bind:style="{display:onList,}"v-if="streamer.on">
      <div class="content-list-li online-li">
        <span class="li-photo">
          <span class="photo-img"><img :src="streamer.logo" alt=""></span>
        </span>
        <div class="li-text">
          <div class="text-id text"><a :href="streamer.href" target="_blank">{{streamer.name}}</a></div>
          <div class="text-game text">{{streamer.game}}</div>
          <div class="text-status text">{{streamer.status}}</div>
        </div>
      </div>
    </div>
    <div class="offline-content content-list" v-bind:style="{display:offList,}" v-else >
      <div class="content-list-li offline-li">
        <span class="li-photo">
          <span class="photo-img"><img :src="streamer.logo" alt=""></span>
        </span>
        <div class="li-text">
          <div class="text-id text"><a :href="streamer.href" target="_blank">{{streamer.name}}</a></div>
          <div class="text-offline text">offline</div>
        </div>
      </div>
    </div>
  </div>
</div>
</template>

<script>
var urlStream = "https://api.twitch.tv/kraken/streams/";
var urlChannel = "https://api.twitch.tv/kraken/channels/";
export default {
  name: 'app',
  data() {
    return {
      streamers: [
      { name: "kyo1984123", status: "offline", game: "...", logo:"",on:false,href:''},
      { name: "godjj", status: "offline", game: "...", logo:"",on:false,href:''},
      { name: "garenatw", status: "offline", game: "...", logo:"",on:false,href:''},
      { name: "asiagodtonegg3be0", status: "offline", game: "...", logo:"",on:false,href:''},
      { name: "solorenektononly", status: "offline", game: "...", logo:"",on:false,href:''},
      { name: "tomchen60229", status: "offline", game: "...", logo:"",on:false,href:''},
      { name: "uzra", status: "offline", game: "...", logo:"",on:false,href:''}],
      btnAll:{active:false,bg:'transparent',color:'#c9ccbe'},
      btnOnline:{active:true,bg:'#c9b6af',color:'#1f3a4a'},
      btnOffline:{active:false,bg:'transparent',color:'#c9ccbe'},
      offList:'none',
      onList:'flex'
    }
  },
  created() {
    this.loopStream();
  },
  methods: {
    loopStream(){
      for (var i=0;i<this.streamers.length;i++) {
        this.getStreams(i);
        this.getChannels(i);
      }
    },
    getStreams(num) {
      var self = this;
      var url;
      var StreamerStatus;
      var name = self.streamers[num].name;
      var _status;
      url = urlStream + self.streamers[num].name;
      this.$http
        .get(url, {
          headers: { "Client-ID": "js4of5oupy7ccd75urlh0ujwt9lv3f" }
        })
        .then(function(response) {
          StreamerStatus = response.data.stream;
          if (StreamerStatus === null) {
            _status = "offline";
            self.streamers[num].on = false;
          } else {
            _status = "online";
            self.streamers[num].on = true;
          }
          self.streamers[num].status = _status;

        })
        .catch(function(error) {
          console.log(error);
        });
    },
    getChannels(_num) {
      var self = this;
      var _urlChannel = urlChannel + self.streamers[_num].name;
      this.$http
        .get(_urlChannel, {
          headers: { "Client-ID": "js4of5oupy7ccd75urlh0ujwt9lv3f" }
        })
        .then(function(response) {
          self.streamers[_num].game = response.data.game;
          self.streamers[_num].status = response.data.status;
          self.streamers[_num].logo = response.data.logo;
          self.streamers[_num].href = response.data.url;
        console.log(self.streamers[_num].href);
        })
        .catch(function(error) {
          console.log(error);
        });
    },
    all() {
      this.btnAll = {active:true,bg:'#c9b6af',color:'#1f3a4a'};
      this.btnOnline = {active:false,bg:'#1f3a4a',color:'#c9b6af'};
      this.btnOffline = {active:false,bg:'#1f3a4a',color:'#c9b6af'};
      this.onList = 'flex';
      this.offList = 'flex';

    },
    online() {
      this.btnOnline = {active:true,bg:'#c9b6af',color:'#1f3a4a'};
      this.btnAll = {active:false,bg:'#1f3a4a',color:'#c9b6af'};
      this.btnOffline = {active:false,bg:'#1f3a4a',color:'#c9b6af'};
      this.onList = 'flex';
      this.offList = 'none';
    },
    offline() {
      this.btnOffline = {active:true,bg:'#c9b6af',color:'#1f3a4a'};
      this.btnAll = {active:false,bg:'#1f3a4a',color:'#c9b6af'};
      this.btnOnline = {active:false,bg:'#1f3a4a',color:'#c9b6af'};
      this.onList = 'none';
      this.offList = 'flex';
    }
  }
}
</script>

<style lang="scss">
@import url(https://fonts.googleapis.com/css?family=Press+Start+2P);//font-family:'Press Start 2P'
@import url(https://fonts.googleapis.com/earlyaccess/notosanstc.css);//font-family: 'Noto Sans TC', sans-serif
$title:#c9ccbe;
$bg:#1f3a4a;
$all:#bf939a;
$online:#bf939a;
$offline:#b35d78;
$listBorder:#cbb8b1;
$white:#c9ccbe;
$pink:#c1969d;
$dark-pink:#b56d80;

$off-bg:#493a4d;
$off-id:#c1969d;
$off-content:#b56d80;
$on-bg:#1d3848;
$on-border:1px solid #c9b6af;
$on-game:#c9bcb2;

html, body {
  width: 100%;
  margin: 0;
  padding: 0;
  background: $bg;

}

.container {
  position:relative;
  left:25%;
  width:50%;
  height:1000px;
  font-family:'Press Start 2P';
  top:30px;
  .header {
    position:relative;
    width:100%;
    height:100px;
    display:flex;
    align-items:center;
    
    .header-title {
      position:absolute;
      width:75%;
      img {
        position:aboslute;
        width:90%;
        padding-left:10%;
      }
    }
    .header-img {
      position:absolute;
      width:30%;
      right:0;
      display:flex;
      align-items:center;
      justify-content:center;
      img {
        position:absolute;
        width:40%;
      }
    }
  }
  
  .button-list {
    position:relative;
    width:100%;
    height:80px;
    display:flex;
    align-items:center;
    justify-content:center;
    color: $white;
    .list-item {
      width: 110px;
      height:33px;
      border: 1.3px solid $listBorder;
      margin: 0px 10px;
      font-size:.8em;
      display:flex;
      align-items: center;
      justify-content:center;
      transition:.3s;
      &:hover {
        cursor:pointer;
      }
      .circle {
        height: 10px;
        width: 10px;
        margin-right:5px;
        border-radius: 50%;
        border: 2px solid #213849;
        background-color:#bf939a;
      }
    }
  }
  .online-content {
    margin-bottom:10px;
    .online-li {
      border:$on-border;
      background:$on-bg;
    }
  }
  .offline-content{
    .offline-li {
      background:$off-bg;
    }
  }
  .content-list {
    position:relative;
    width:100%;
    display:flex;
    justify-content:center;
    .content-list-li {
      position:relative;
      width:90%;
      height:110px;
      margin:10px 0;
      display:flex;
      justify-content:center;
      .li-photo {
        position:absolute;
        width: 30%;
        height: 100%;
        left:0;
        display:flex;
        justify-content:center;
        align-items:center;
        .photo-img {
          position:absolute;
          width: 50px;
          height: 50px;
          border-radius: 50%;
          border: 1.5px solid;
          border-color:#c9bcb2;
          overflow:hidden;
          img {
            position:absolute;
            width:100%;
          }
        }
      }
      .li-text {
        position:absolute;
        width: 70%;
        height: 100%;
        right:0;
        font-size:.8em;
        .text {
          position:relative;
          width:90%;
          height:22px;
          margin-top:10px;
          left:5%;
          display:flex;
          align-items: center;
          overflow: hidden;
          white-space: nowrap;
          text-overflow: ellipsis;
          a{
            text-decoration: none;
            color:$pink;
          }
        }
        .text-id {
          color:$pink;
        }
        .text-status {
          color:$pink;
          font-family: 'Noto Sans TC', sans-serif;
        }
        .text-game {
          color:$white;
        }
        .text-offline {
          color:$dark-pink;
        }
      }
    }
  }
}

@media (max-width: 800px) {
  .container {
    width:70%;
    left:15%;
  }
}
@media (max-width: 720px) {
  .container {
    width:80%;
    left:10%;
  }
}
@media (max-width: 550px) {
  .container {
    width:100%;
    left:0;
    top:10px;
  }
  .circle {
    display:none;
  }
}
</style>
