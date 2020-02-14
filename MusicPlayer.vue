<template>
    <div id="musicPlayer">
        <div id="top">
            <span id="musicSymbol" class="glyphicon glyphicon-music"></span>
            <img id="logo" src="../../../assets/img/LOGO_white.png">
            <span id="title"> <b>Rabbiter Mini Audio Player</b></span>
        </div>
        <div id="menu">
            <button class="btn-ex btn btn-default glyphicon glyphicon-step-backward" @click="prevMusic"></button>
            <button class="btn-ex btn btn-default glyphicon glyphicon-repeat" v-if="!ifLoop"
                @click="ifLoop = true">顺序播放</button>
            <button class="btn-ex btn btn-default glyphicon glyphicon-refresh" v-if="ifLoop"
                @click="ifLoop = false">单曲循环</button>
            <button class="btn-ex btn btn-default glyphicon glyphicon-step-forward" @click="nextMusic"></button>
        </div>
        <div id="musicMsg">
            歌手: {{music.singer}}<br>
            歌曲: {{music.name}}
        </div>
        <audio id="audio" ref="audio" autoplay="false" controls @ended="playMusic">
            <source :src="music.musicSrc" type="audio/mpeg">
        </audio>
    </div>
</template>

<script>
    export default {
        name: "musicPlayer",
        props: ["musicList", "minVolume"],
        data() {
            return {
                ifLoop: false, //是否单曲循环
                music: {
                    index: "", //歌曲下标
                    musicSrc: "", //歌曲路径
                    name: "", //歌曲名
                    singer: "" //歌手
                },
            }
        },
        methods: {
            prevMusic() { //上一首
                if (this.music.index == 0) { //如果是第一首,加载最后一首
                    this.music = this.musicList[this.musicList.length - 1];
                } else { //如果不是第一首,加载上一首
                    this.music = this.musicList[this.music.index - 1];
                }
                this.$refs.audio.load();
                this.$refs.audio.play(); //开始播放
            },
            nextMusic() { //下一首
                if (this.music.index >= this.musicList.length - 1) { //如果是最后一首,加载第一首
                    this.music = this.musicList[0];
                } else { //如果不是最后一首,加载下一首
                    this.music = this.musicList[this.music.index + 1];
                }
                this.$refs.audio.load();
                this.$refs.audio.play(); //开始播放
            },
            playMusic() { //播放结束后的动作
                if (this.ifLoop) { //单曲循环
                    this.$refs.audio.play(); //不需要切换歌曲,开始播放
                } else { //顺序播放
                    if (this.music.index >= this.musicList.length - 1) { //当前播放着最后一首
                        this.music = this.musicList[0]; //加载第一首
                    } else { //当前播放的不是最后一首
                        this.music = this.musicList[this.music.index + 1]; //加载下一首
                        console.log(this.music.musicSrc);
                    }
                    this.$refs.audio.load();
                    this.$refs.audio.play(); //开始播放
                }
            }
        },
        mounted() {
            for (let i = 0; i < this.musicList.length; ++i) { //给musicList对每个对象添加一个下标属性
                this.musicList[i].index = i;
            }
            this.$refs.audio.pause(); //初始暂停
            this.music = this.musicList[0]; //初始歌曲,第一首
            this.$refs.audio.volume = this.minVolume; //初始音量
        },
    }
</script>

<style scoped>
    #musicPlayer {
        position: absolute;
    }

    #top {
        background-color: black;
        opacity: 0.7;
        color: white;
        border-radius: 25px 25px 0 0;
    }

    #musicSymbol {
        font-size: 13px;
    }

    #logo {
        width: 20px;
    }

    #title {
        font-size: 13px;
    }

    #menu {
        background-color: black;
        opacity: 0.7;
        padding-bottom: 5px;
    }

    #musicMsg {
        background-color: black;
        opacity: 0.7;
        color: white;
        font-size: 11px;
    }

    #audio {
        border-radius: 0 0 25px 25px;
        background-color: black;
        opacity: 0.7;
        width: 300px;
        border-bottom: 1px solid black;
    }

    .btn-ex {
        color: white;
        margin-left: 5px;
        margin-right: 5px;
    }
</style>