<template>
  <div>
    <!-- <div></div> -->
    <canvas
      v-on="{ mousedown: draw, mouseup: drawOut }"
      ref="canvasReal"
      width="800"
      height="400"
      >如果不支持canvas就会渲染这段文字</canvas
    >
    <button @click="clearCanvas">清空画布</button>
    线条粗细:
    <select id="lineWidth" v-model.number="lineWidths">
      <option value="1">1</option>
      <option value="2">2</option>
      <option value="3">3</option>
      <option value="4">4</option>
    </select>
    线条颜色:
    <select id="lineColor" v-model="color">
      <option value="red">red</option>
      <option value="orange">orange</option>
      <option value="yellow">yellow</option>
    </select>
    <button @click="saveSignature">保存签名</button>
    <a @click="imgDownload">预览</a>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",

  data() {
    return {
      x: Number(),
      y: Number(),
      imgData: "",
      imgUrl: '',
      canvas: "",
      ctx: "",
      color: 'red',
      lineWidths: 1
    };
  },

  mounted() {
    /*绘制一个正方形 */
    /*let canvas = this.$refs.canvasReal
    console.log(canvas);
    if(canvas) {
      let ctx = canvas.getContext('2d');
      console.log(ctx);  
      // ctx.fillStyle = 'rgb(200,0,0)' //指定颜色color, 默认为黑; 或者渐变gradient或者自定义样式pattern
      ctx.fillRect(0,0,100,100)
    }*/
    /*绘制三角形*/
    /*let canvas = this.$refs.canvasReal
    if(canvas) {
      let ctx = canvas.getContext('2d');
      ctx.beginPath();
      ctx.moveTo(60,70); //自定义起始点
      ctx.lineTo(120,30)  //绘制直线
      ctx.lineTo(120,160);
      ctx.fill(); //填充会自动闭合closePath(), 但是stroke()不会, 然后可以重新绘制新的
    }*/
    /*绘制圆脸笑脸 */
    /*let canvas = this.$refs.canvasReal;
    if (canvas) {
      this.ctx = canvas.getContext("2d");
      this.ctx.beginPath();
      this.ctx.arc(250, 200, 50, 0, 1.5 * Math.PI, false); //圆心x,y,r,起始角度(三点钟为0), 转动角度几个Math.PI, 逆时针画false
      // this.ctx.arc(250,200,50,2*Math.PI, false)
      this.ctx.stroke();
    }*/
  },

  methods: {
    clearCanvas() {
      this.ctx.clearRect(0, 0, 800, 400); //x,y,width,height清除画布
    },
    saveSignature() {
      let a = confirm("确定是否保存签名?");
      if (a == 1) {
        this.imgData = this.ctx.getImageData(0, 0, 800, 400);
        this.imgUrl = this.canvas.toDataURL();
        // console.log(this.imgUrl);
        // downloadImage(imgUrl, 'myCanvas.img')
        // window.location.href = this.imgUrl;
        this.clearCanvas();
      }
    },
    draw(e1) {
      this.canvas = this.$refs.canvasReal;
      this.ctx = this.canvas.getContext("2d");
      this.ctx.beginPath();
      this.ctx.strokeStyle = this.color
      this.ctx.lineWidth = this.lineWidths
      this.ctx.moveTo(e1.offsetX, e1.offsetY);
      let handleMousemove = (e) => {
        console.log(e);
        this.x = e.offsetX;
        this.y = e.offsetY;
        this.ctx.lineTo(this.x, this.y);
        this.ctx.stroke();
      };
      this.canvas.addEventListener("mousemove", handleMousemove);
      this.canvas.addEventListener("mouseup", () => {
        this.canvas.removeEventListener("mousemove", handleMousemove, () => {
          //笔画的间隔
          // let width = ctx.canvas.width;
          // let height = ctx.canvas.height;
          this.imgData = ctx.getImageData(0, 0, 800, 400); //x,y,width,height保存画画
          // ctx.putImageData(this.imgData, 0, 0); //调用保存的画放在画布上
        });
      });
    },
    drawOut() {},
    imgDownload(){
    if(this.imgUrl == '') {
      alert('图片为空!!!')
    } else {
      let iframe = `<iframe width="100%" height="100%" src="${this.imgUrl}"></iframe>`
      let x = window.open()
      x.document.open()
      x.document.write(iframe)
      x.document.close()
    }
  }
  },
  
};
</script>

<style scoped>
canvas {
  display: block;
  position: relative;
  margin: 0 auto 5px;
  width: 800px;
  height: 400px;
  border: 1px solid #666;
}
a {
  margin-left: 5px;
  color: red;
}
</style>