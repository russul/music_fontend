
<template>
  <el-container style="height: 1100px; width: 50%; padding: 0px; text-align: center; margin-left: 25%; margin-right: 25%;">
    <el-header style="height: 200px;width: 100%;margin-right: 0;margin-left: 0;padding: 0;background-color: white">
      <div style="height: 30%">
        <span style="font-family: 'Brush Script MT';color: goldenrod; font-size: 45px">The Evaluation of Automatic Composition</span>
      </div>
      <div style="height: 70%;width: 100%;margin-right: 0;margin-left: 0;padding: 0">
        <img src="static/logo1.gif" style="width: 100%;height: 100%"/>
      </div>
    </el-header>
    <el-container>
      <el-aside class="el-aside" width="100%">
        <div v-for="(item) in viewArr" :key="item.music_name">

        <div style="height: 75px;width:100%">
          <div  style="height: 75px;margin-top: 10px;width: 55%;border-right: burlywood solid 2px;float: left">
            <p style="font-size: 10px">音乐名称：<span>{{item.music_name}}</span></p>
            <div style="height: 80%;width: 100%">
              <!-- 此处的ref属性，可以很方便的在vue组件中通过 this.$refs.audio获取该dom元素 -->
              <audio ref="audio"
                     @pause="onPause"
                     @play="onPlay"
                     v-bind:src="item.url" controls="controls">
              </audio>

              <!-- 音频播放控件 -->
              <!--<div>-->
              <!--<el-button type="text" @click="startPlayOrPause">{{audio.playing | transPlayPause}}</el-button>-->
              <!--</div>-->
            </div>
          </div>
          <div style="height: 75px;margin-top:10px;float: right">
            <p style="font-size: 10px"><span>请试听后评分</span></p>
            <el-rate
              v-model="item.score"
              :colors="colors"
            >
            </el-rate>
          </div>
        </div>
        </div>
        <div style="margin-top: 150px;margin-left: 50%">
          <el-form ref="viewArr" :model="viewArr">
          <el-form-item>
            <el-button type="primary" @click="submit_info(viewArr)">提交</el-button>
          </el-form-item>
          </el-form>
        </div>
        </el-aside>
    </el-container>
  </el-container>
</template>
<script>

export default {

  data () {
    return {
      audio: {
        // 该字段是音频是否处于播放状态的属性
        playing: false
      },
      colors: ['#99A9BF', '#F7BA2A', '#FF9900'],
      viewArr: [
        {music_name: '', url: '', score: null},
        {music_name: '', url: '', score: null},
        {music_name: '', url: '', score: null},
        {music_name: '', url: '', score: null},
        {music_name: '', url: '', score: null},
        {music_name: '', url: '', score: null},
        {music_name: '', url: '', score: null},
        {music_name: '', url: '', score: null},
        {music_name: '', url: '', score: null},
        {music_name: '', url: '', score: null}
      ]
    }
  },
  name: 'my_rate',
  methods: {
    // 控制音频的播放与暂停
    startPlayOrPause () {
      return this.audio.playing ? this.pause() : this.play()
    },
    // 播放音频
    play () {
      this.$refs.audio.play()
    },
    // 暂停音频
    pause () {
      this.$refs.audio.pause()
    },
    // 当音频播放
    onPlay () {
      this.audio.playing = true
    },
    // 当音频暂停
    onPause () {
      this.audio.playing = false
    },
    submit_info (info) {
      this.axios({
        method: 'post',
        url: 'http://31y3p89758.eicp.vip/submit',
        data: info
      })
        .then(function (res) {
          console.log(res)
          if (res.data === 'Ok') {
            alert('感谢您的参与')
          } else {
            alert('wawa,好像有点问题')
          }
        })
        .catch(function (err) {
          console.log(err)
        })
    },
    music_list () {
      var _this = this
      this.axios({
        method: 'get',
        url: 'http://31y3p89758.eicp.vip/musicList'
      })
        .then(function (res) {
          console.log(res)
          var musics = res.data
          // console.log(this.viewArr)
          _this.viewArr = musics

          // for (var i = 0; i < musics.length; i++) {
          //   // alert(musics[i].music_name)
          //   this.viewArr[i].music_name = musics[i].music_name
          //   alert(this.viewArr[i].music_name)
          //   this.viewArr[i].url = musics[i].url
          // }
        })
        .catch(function (err) {
          console.log(err)
        })
    }
  },
  filters: {
    // 使用组件过滤器来动态改变按钮的显示
    transPlayPause (value) {
      return value ? '暂停' : '播放'
    }
  },
  mounted: function () {
    this.music_list()
  }
}
</script>
<style>
  .el-header,
  .el-footer {
    background-color: #B3C0D1;
    color: #333;
    text-align: center;
    line-height: 60px;
  }
  .el-aside {
    background-color: white;
    color: #333;
    text-align: left;
    /*line-height: 10px;*/
   }
  .el-main {
    background-color: #B3C0D1;
    color: #333;
    text-align: left;
    /*line-height: 10px;*/
  }
</style>
