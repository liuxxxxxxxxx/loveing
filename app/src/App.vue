<template>
  <div id="app">
    <StyleEditor ref="styleEditor" :code="currentStyle"></StyleEditor>
    <ResumeEditor
      ref="resumeEditor"
      :markdown="currentMarkdown"
      :enableHtml="enableHtml"
    ></ResumeEditor>
  </div>
</template>
<script>
import StyleEditor from './components/StyleEditor'
import ResumeEditor from './components/ResumeEditor'
import './assets/reset.css'
let isPc = (function () {
  var userAgentInfo = navigator.userAgent
  var Agents = [
    'Android',
    'iPhone',
    'SymbianOS',
    'Windows Phone',
    'iPad',
    'iPod',
  ]
  var flag = true
  for (var v = 0; v < Agents.length; v++) {
    if (userAgentInfo.indexOf(Agents[v]) > 0) {
      flag = false
      break
    }
  }
  return flag
})()
let getDateDiff = function (startDate, endDate) {
  var startTime = new Date(Date.parse(startDate.replace(/-/g, '/'))).getTime()
  var endTime = new Date(Date.parse(endDate.replace(/-/g, '/'))).getTime()
  var dates = Math.abs(startTime - endTime) / (1000 * 60 * 60 * 24)
  return dates
}
document.title +=
  getDateDiff(
    new Date().getFullYear() +
      '-' +
      (new Date().getMonth() + 1) +
      '-' +
      new Date().getDate(),
    '2016-10-01'
  ) +
  1 +
  '天'
export default {
  name: 'app',
  components: {
    StyleEditor,
    ResumeEditor,
  },
  data() {
    return {
      interval: 27,
      currentStyle: '',
      enableHtml: false,
      fullStyle: [
        `/*
* Hi。阿科！
* 这么久了。还没和你说过我的工作呢！
* 我是个JAVA开发工程师。俗称程序员。网页相关。
* 如这个页面。就是个什么也没有的网页。
* 我的工作就是给这种空白的页面加点儿东西。
* 嗯。说起来手机和电脑还得区分一下。
* 你现在用的是。。。${isPc ? '电脑' : '手机'}
*/

/* 首先给所有元素加上过渡效果 */
* {
  -webkit-transition: all .3s;
  transition: all .3s;
}
/* 白色背景太单调了。来点背景 */
html {
  color: rgb(222,222,222);
  background: rgb(0,43,54); 
}
/* 文字太近了 */
.styleEditor {
  padding: .5em;
  border: 1px solid;
  margin: .5em;
  overflow: auto;
  ${isPc ? 'width: 48%;height: 96%;' : 'width: 96%;height: 50%;'}
  font-size: 14px;
  line-height:1.5;
}
/* 这些代码颜色都一样。加点儿高亮区别来 */
.token.selector{ color: rgb(133,153,0) }
.token.property{ color: rgb(187,137,0) }
.token.punctuation{ color: yellow }
.token.function{ color: rgb(42,161,152) }
.token.comment{ color: rgb(177,177,177) }
/* 加个 3D 效果 */
html{
  -webkit-perspective: 1000px;
          perspective: 1000px;
}
.styleEditor {
  position: fixed; 
  ${isPc ? 'left: 0;' : 'left:0;right:0;margin:auto;'}
  top: 0; 
  -webkit-transition: none; 
  transition: none;   
  ${
    isPc
      ? '-webkit-transform: rotateY(10deg) translateZ(-100px) ;transform: rotateY(10deg) translateZ(-100px) ;'
      : '-webkit-transform: rotateX(-10deg) translateZ(-100px) ;transform: rotateX(-10deg) translateZ(-100px) ;'
  }
  ${
    isPc ? '' : '-webkit-transform-origin: 50% 0% 0;transform-origin: 50% 0% 0;'
  }
}

/* 再来一张信纸 */
.resumeEditor{
  position: fixed; 
  ${isPc ? 'right: 0;' : 'left:0;right:0;margin:auto;'}
  ${isPc ? 'top: 0;' : 'bottom:2%;'}
  padding: .5em;  
  ${isPc ? 'margin: .5em;' : ''}
  ${isPc ? 'width: 48%;height: 96%;' : 'width: 96%;height: 50%;'}
  border: 1px solid;
  color: #222;
  overflow: auto;
  font-size: 14px;
  line-height:1.5;
  ${
    isPc
      ? '-webkit-transform: rotateY(-10deg) translateZ(-100px) ;transform: rotateY(-10deg) translateZ(-100px) ;'
      : '-webkit-transform: rotateX(10deg) translateZ(-100px) ;transform: rotateX(10deg) translateZ(-100px) ;'
  }
    ${
      isPc
        ? ''
        : '-webkit-transform-origin: 50% 0% 0;transform-origin: 50% 0% 0;'
    }
  }
/* 我开始写了 */


`,
        `
/* 是不是看着很简陋粗糙？
 * 因为这是 Markdown 格式的
 * 一种程序员用来写文档日志的简易语言
 * 翻译成 网页 就行了
 */
`,
        `
/* 再加点样式 */
.resumeEditor{
  padding: 2em;
  line-height:1.8;
}
.resumeEditor h2{
  display: inline-block;
  border-bottom: 1px solid;
  margin: 1em 0 .5em;
  font-size:18px;
}
.resumeEditor ul,.resumeEditor ol{
  list-style: none;
}
.resumeEditor ul> li::before{
  content: '•';
  margin-right: .5em;
}
.resumeEditor ol {
  counter-reset: section;
}
.resumeEditor ol li::before {
  counter-increment: section;            
  content: counters(section, ".") " ";  
  margin-right: .5em;
}
.resumeEditor blockquote {
  margin: 1em;
  padding: .5em;
  background: rgba(221,221,221,.5);
}

/* OK。完成！ */

`,
      ],
      currentMarkdown: '',
      fullMarkdown: `科科 × 小欣欣
----

2015年01月01日。我第一次见到你，我就心动了。  
2016年10月01日。我怀着激动的心情来到苏州。在一起了。  
2020年08月25日。即阴历七月初七。yeah！我们结婚啦~~  
如果能撑到今天，那么我们在一起的时间已有 \`${
        getDateDiff(
          new Date().getFullYear() +
            '-' +
            (new Date().getMonth() + 1) +
            '-' +
            new Date().getDate(),
          '2016-10-01'
        ) + 1
      }\` 天了耶

一起呲过的餐厅
----

* 桃花源记
* 牛鲜生
* 鲁员外地锅鸡（嘻嘻，我超喜欢）
* 农夫烤牛肉
* 弄堂里
* 嘭嘭鲜牛杂煲
* 蜀大侠
* 豪渝火锅（我们真的我喜欢吃火锅呀(●'◡'●)）
* 丑丑牛
* ......
* 各种藏书羊肉🤭
* 老阿爸
* 同人 木山溪

一起看过的电影
----

1. 霍比特人3（这是我们还没在一起时你带我看的呢!!!激动(*^_^*)）
2. 雷神3
3. 黑豹（看这部的时候我们还在说一起看黑豹2呢）
4. 复仇者联盟3
5. 名侦探柯南
6. 精灵旅社3
7. 精灵旅社4
8. 夏洛特烦恼（原来你是从看这部电影就开始讨厌我了呀，嘤嘤嘤）
9. ......
10. 还有好多小电影呐

一起玩过的地方
----

* 杭州
* 九江
* 赣州
* 可惜，最终没等到蜜月耶(T_T)

一起玩过的游戏
----

1. 保卫萝卜2
2. 保卫萝卜3
3. 保卫萝卜4
4. 打恶龙🤭🤭（哎呀，我也不知道那个游戏叫什么）
5. 波西亚时光
6. 沙市镇时光（你说要和我一起玩的也停滞了，我真的太笨了~~）

> 昨天收到了法院的离婚判决书，原来觉得在一起是那么的迅速和突然，现在发现分开也能如此猝不及防.

> 哎，无论我们最后生疏成什么样子，曾经对你的好都是真的，虽然我很难过终有一散，也别辜负相遇，希望你不后悔认识我，也是真的快乐过，如果能回到以前，我会选择不认识你，不是我后悔，是我不能面对现在的结局。

> 不管结局怎么样，最后就祝我两情人节快乐吧(●'◡'●)

`,
    }
  },
  created() {
    this.makeResume()
  },

  methods: {
    makeResume: async function () {
      await this.progressivelyShowStyle(0)
      await this.progressivelyShowResume()
      await this.progressivelyShowStyle(1)
      await this.showHtml()
      await this.progressivelyShowStyle(2)
    },
    showHtml: function () {
      return new Promise((resolve, reject) => {
        this.enableHtml = true
        resolve()
      })
    },
    progressivelyShowStyle(n) {
      return new Promise((resolve, reject) => {
        let interval = this.interval
        let showStyle = async function () {
          let style = this.fullStyle[n]
          if (!style) {
            return
          }
          // 计算前 n 个 style 的字符总数
          let length = this.fullStyle
            .filter((_, index) => index <= n)
            .map((item) => item.length)
            .reduce((p, c) => p + c, 0)
          let prefixLength = length - style.length
          if (this.currentStyle.length < length) {
            let l = this.currentStyle.length - prefixLength
            let char = style.substring(l, l + 1) || ' '
            this.currentStyle += char
            if (style.substring(l - 1, l) === '\n' && this.$refs.styleEditor) {
              this.$nextTick(() => {
                this.$refs.styleEditor.goBottom()
              })
            }
            setTimeout(showStyle, interval)
          } else {
            resolve()
          }
        }.bind(this)
        showStyle()
      })
    },
    progressivelyShowResume() {
      return new Promise((resolve, reject) => {
        let length = this.fullMarkdown.length
        let interval = this.interval
        let showResume = () => {
          if (this.currentMarkdown.length < length) {
            this.currentMarkdown = this.fullMarkdown.substring(
              0,
              this.currentMarkdown.length + 1
            )
            let lastChar = this.currentMarkdown[this.currentMarkdown.length - 1]
            let prevChar = this.currentMarkdown[this.currentMarkdown.length - 2]
            if (prevChar === '\n' && this.$refs.resumeEditor) {
              this.$nextTick(() => this.$refs.resumeEditor.goBottom())
            }
            setTimeout(showResume, interval)
          } else {
            resolve()
          }
        }
        showResume()
      })
    },
  },
}
</script>
<style scoped>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

html {
  min-height: 100%;
}
.styleEditor {
  -webkit-backface-visibility: hidden;
}
</style>
