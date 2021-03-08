<template>
  <div class="container">
    <div>
      <div class="box">
        <ul>
          <li v-for="item in 4" :key="item" @mousemove="mousemove(item)"></li>
        </ul>
        <img class="bgImg_0" :src="imgUrl" alt="🏞">
        <div class="bg">
          <img style="width:100%;height:100%;" :src="imgUrl" alt="🏞">
        </div>
      </div>
      <img class="bgImg_1" :src="imgUrl" alt="🏞"/>
    </div>
  </div>
</template>

<style>
img{
  -webkit-user-select: none;
  cursor: default;
  pointer-events: none;
}
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  user-select: none;
  background: rgba(0,0,0,0.5);
}
</style>

<style lang="scss" scoped>
.bg{
    width: 300px;
    height: 300px;
    position: relative;
    user-select:none;
}
.bgImg_1{
  width:300px;
  height:300px;
  margin-top:30px;
}
.box{
  width: 300px;
  height: 300px;
  position: relative;
  overflow: hidden;
  .bgImg_0{
    position: absolute;
    top:0;
    left: 0;
    width:100%;
    height:100%;
    opacity: 0.5;
  }
  img{
    user-select:none;
  }
  .bg{
    width: 300px;
    height: 300px;
    position: absolute;top:0;z-index:3;
  }
  >ul,>ul>li{
    margin: 0;
    padding: 0;
    list-style-type: none;
  }
  >ul{
    li{
      position: absolute;
      top:0;
      width: 20px;
      height: 20px;
      border:1px solid white;
      border-radius: 50%;
      cursor: pointer;
      z-index: 4;
    }
    li:nth-child(1){
      top: 0;
    }
    li:nth-child(2){
      top: 0;
      left: 50%;
    }
    li:nth-child(3){
      top: 50%;
      left: 50%;
    }
    li:nth-child(4){
      top: 50%;
      left: 0;
    }
  }
}
</style>

<script>

export default {
  layout: "main",
  name: "index",
  data() {
    return {
      x:50,
      y: 75,
      x1: 120,
      y1: 75,
      x2: 120,
      y2: 140,
      x3: 50,
      y3: 140,
      delX: 0,
      delY: 0,
      index: 0,
      imgUrl: 'https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=3363295869,2467511306&fm=26&gp=0.jpg'
    };
  },
  components: {},
  mounted() {
    const box = document.querySelector('.box');
    const bg = document.querySelector('.bg');
    const bgImg = document.querySelector('.bgImg_1');
    const lis = document.querySelectorAll('.box ul li');
    this.createArc();
    document.onmouseup = (event) => {
      const {clientX, clientY} = event;
      const delX = clientX - this.elementLi(this.index).offsetLeft;
      const delY = clientY - this.elementLi(this.index).offsetTop;
      this.createLabel(this.index,clientX - delX,clientY - delY);
      document.onmousemove = null;
    }
    box.addEventListener('mousedown',(event) => {
      this.delX = event.clientX - this.elementLi(this.index).offsetLeft;
      this.delY = event.clientY - this.elementLi(this.index).offsetTop;
      document.onmousemove = (ev) => {
        let {  clientX,
            clientY} = ev;
        let practicalX = clientX - this.delX;
        let practicalY = clientY - this.delY;
        practicalX = practicalX>=280 ? 279: practicalX<=0? 1:practicalX;
        practicalY = practicalY>=280 ? 279: practicalY<=0? 1:practicalY;
        if (this.index == 0) {
          this.x = practicalX;
          this.y = practicalY;
        }
        if (this.index == 1) {
          this.x1 = practicalX;
          this.y1 = practicalY;
        }
        if (this.index == 2) {
          this.x2 = practicalX;
          this.y2 = practicalY;
        }
        if (this.index == 3) {
          this.x3 =practicalX;
          this.y3 = practicalY;
        }
        this.createLabel(this.index,practicalX,practicalY);
        bgImg.style.clipPath = `polygon(${((this.x + 10)/300)*100}% ${((this.y + 10)/300 )*100}%, ${((this.x1 + 10)/300)*100}% ${((this.y1 + 10)/300)*100}%, ${((this.x2 + 10)/300)*100}% ${((this.y2 + 10)/300)*100}%, ${((this.x3+10)/300)*100}% ${((this.y3+10)/300)*100}%)`;
        bg.style.clipPath = `polygon(${((this.x + 10)/300)*100}% ${((this.y + 10)/300 )*100}%, ${((this.x1 + 10)/300)*100}% ${((this.y1 + 10)/300)*100}%, ${((this.x2 + 10)/300)*100}% ${((this.y2 + 10)/300)*100}%, ${((this.x3+10)/300)*100}% ${((this.y3+10)/300)*100}%)`;
      }
    })
  },
  methods: {
    mousemove(index) {
      this.index = index - 1;
    },
    elementLi(index){
      const li = document.querySelectorAll('.box li')[index]
      return li
    },
    createArc() {
      this.createLabel(0,this.x, this.y);
      this.createLabel(1,this.x1, this.y1);
      this.createLabel(2,this.x2, this.y2);
      this.createLabel(3,this.x3, this.y3);
    },
    createLabel(index,x,y) {
      const _li = document.querySelectorAll('.box li')[index]
      _li.style.top = y + 'px';
      _li.style.left = x + 'px';
    },
    createLine() {
      var c = document.getElementById("myCanvas");
      var cxt = c.getContext("2d");
      cxt.moveTo(this.x, this.y);
      cxt.lineTo(this.x1, this.y1);
      cxt.lineTo(this.x2, this.y2);
      cxt.lineTo(this.x3, this.y3);
      cxt.lineTo(this.x, this.y);
      cxt.stroke()
    }
  }
};
</script>
