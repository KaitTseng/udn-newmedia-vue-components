<template>
	<div class="section pageContent" :style="{backgroundImage: 'url('+ srcRWD(bg, bgWeb) +')', backgroundColor: bgColor}">
		<div class="mediaContainer" :class="{videoIntro: isVideo, imgIntro: isImg}" :style="{backgroundColor: BoxColor}">
            <div class="videoModel" v-if="isVideo">
                <EmbededVideo :src='videoSrc' :srcWeb='videoSrcWeb' :poster='videoPoster' :posterWeb='videoPosterWeb' customControl='yes' :controlColor="controlColor"/>
            </div>
            <div class="imgModel" v-if="isImg">
                <img :src="ImgSrc" :title="ImgSay" :alt="ImgSay">
                <p>{{ImgSay}}</p>
            </div>
            <div class="articleModel" :style="{color: fontColor}">
                <h2>{{title}}</h2>
                <div class="articleContent" :class="{isQuote: useQuote}">
                    <slot></slot>
                </div>
                <div class='quote' :style="{order: isFirst}" v-if='useQuote' :class="{quoteFirst: letFirst}">
                    <p>{{quoteSay}}</p>
                </div>
            </div>
		</div>
	</div>
</template>

<script>
import EmbededVideo from '../../components/EmbededVideo.vue';
import srcRWD from '../mixin/srcRWD.js'
export default {
  name: 'PageContent',
  components:{
  	EmbededVideo
  },
  mixins: [srcRWD],
  props: ['title', 'quoteFirst', 'useQuote', 'quoteSay', 'videoSrc', 'videoSrcWeb', 'videoPoster', 'videoPosterWeb', 'BoxColor', 'ImgSrc', 'ImgSay', 'fontColor', 'bg', 'bgWeb', 'bgColor', 'fontColor', 'controlColor'],
  data () {
    return {

    }
  },
  computed: {
    isVideo: function() {
        if(this.videoSrc || this.videoPoster){
            return true
        } else {
            return false
        }
    },
    isImg: function() {
        if(this.ImgSrc || this.ImgSay){
            return true
        } else {
            return false
        }
    },
    letFirst: function() {
        if(this.quoteFirst === 'yes') {
            return true
        } else if(this.quoteFirst === 'no'){
            return false
        }
    }
  },
  methods: {
    bgRWD: function(){
        if(window.innerWidth <= 768){
            if(window.matchMedia("(orientation: landscape)").matches){
                return this.bgWeb
            }
            else{
                return this.bg
            }
        }
        else{
            return this.bgWeb
        }
    }
  },
  created() {
    window.addEventListener('resize', () => {
        this.$forceUpdate()
    })    
  },
}
</script>

<style lang="scss" scoped>
    .pageContent{
        background-repeat: no-repeat;
        background-size: cover;
        background-position: center center;
    }
	.mediaContainer{
		width: calc(100% - 30px);
        max-width: 940px;
		margin: 0 auto;
		border-radius: 4px;
		padding: 20px 15px;
		background-color: #fff;
		display: flex;
		flex-direction: column;
        align-items: center;
        justify-content: space-around;
	}
    .videoModel{
        width: 100%;
        margin-bottom: 20px;
    }
    .imgModel{
        width: 100%;
        margin-bottom: 30px;
        img{
            width: 100%;
            height: auto;
        }
        p{
            font-size: 15px;
            color: gray;            
        }
    }
    .articleModel{
        width: 100%;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
    }
    .quote{
        width: 100%;
        margin: 15px 0;
        order: 4;
        p{
            border-top: 2px solid #29b0af;
            border-bottom: 2px solid #29b0af;            
            padding-top: 15px;
            padding-bottom: 15px;            
            font-weight: bold;
        }
    }
    .articleContent{
        width: 100%;
        order: 3;
    }
    .quoteFirst{
        order: 2 !important;
        padding-left: 0 !important;
        padding-right: 0 !important;
    }
    h2{
        text-align: left;
        font-size: 25px;
        font-weight: bold;
        margin-top: 0px;
        margin-bottom: 10px;
        line-height: 1.5;
        width: 100%;
        order: 1;
    }
    p{
        width: 100%;
        font-size: 16px;
        margin: 0;
        text-align: left;
        line-height: 1.5;
    }
    p > br{
        line-height: 32px;
    }
    @media screen and (min-width: 1024px){
        .mediaContainer{
            flex-direction: row;
            padding: 30px 25px;
        }            
        .imgModel{
            flex: 1;
            margin-bottom: 0; 
            p{
                font-size: 17px;
            }         
        }
        .videoModel{
            flex: 1;
            margin-bottom: 0;
        }
        h2{
            font-size: 40px;
            margin-bottom: 20px;
        }        
        p{
            max-width: 880px;
            font-size: 18px;
        }        
        .articleModel{
            flex: 1;
            flex-direction: row;
            flex-wrap: wrap;
            align-items: flex-start;
            justify-content: space-between;
            padding: 10px;
        }
        p>br{
            line-height: 36px;
        }
        .isQuote{
            width: 50% !important;
        }
        .quote{
            margin: 0;
            flex: 1;
            padding-left: 20px;
        }
        .quoteFirst{
            order: 2 !important;
            padding-left: 0 !important;
            padding-right: 20px !important;
        }        
    }	
</style>