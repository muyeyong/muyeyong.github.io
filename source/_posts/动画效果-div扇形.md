---
title: 动画效果-div扇形
top: false
cover: false
toc: true
mathjax: true
date: 2020-12-12 16:35:24
password:
summary: 怎么去“骗”用户🤣
tags: animation
categories: 动画效果
---

# div扇形动画

先上效果图

![](https://i.loli.net/2020/12/12/8NEiPuGOtTVg2j4.gif)

这是最近实现的一个需求效果，本来打算使用Echarts做的，但是动画效果达不到要求，遂卒。然后思考🤔怎么实现，写过一版用canvas实现的，但只写到画一个饼状图 （ ~~不如用Echarts~~）😂，在文末补充画饼状图的过程。

回到正文，上面的效果图中完整的饼状图还是用Echarts画的，但后面的圆弧是用div画的。需求需要实现的主要功能点用圆弧实现圆的循环效果，但如Echarts画的饼状图，在旋转的过程中文字会飘，所以做了隐藏的处理。

## 实现步骤

### 画一个静态的圆弧

```
通过设置border-radius实现（画三角形是通过border在实现的）
```

![](https://i.loli.net/2020/12/12/gLquzUEjBTw9YXp.png)

这就完成了最关键的一步了，现在已经画出了一个90°的圆弧，如果你想画出特定角度的圆弧可以通过叠加 or 遮盖实现视觉上的效果。

### 画很多圆弧

既然画出了一个圆弧，画很多圆弧不也很简单吗🍀，确实是的，但要考虑到最边上的圆弧，中间的圆弧可以通过相互覆盖实现在视觉上画出特定角度的圆弧（实际上我们画的都是90°的圆弧），但最边上的两个圆弧没有谁帮它覆盖，没有就去创造，在这两个圆弧旁边再画圆弧帮它遮挡就可以了。

### 无限循环

当点击每个圆弧的时候需要实现无限循环的效果，可圆弧又不是圆，实在有点为难它了，但只要实现视觉上的效果，管它是牛是马。例如当我们点击最上面的圆弧，需要它实现往下旋转的效果，这就话暗藏需要实现的两个功能点：

* 要实现旋转的视觉效果
* 旋转完后数据显示正常

对于第一点：把整个圆弧放到一个容器（div）中，直接旋转父容器就可以在视觉上实现旋转效果

对于第二点：数据显示正常也就是显示的位置、数据数量以及顺序是不能发生变化的，点击最上面的圆弧，它向下旋转N（☞离视觉的中点隔得圆弧数量）个位置，它下旋了N个位置自然它上面要出现N个圆弧，下面要减少N个圆弧，这里就需要我们提前处理数据，在圆弧在开始以及结束处补数据。开始处补的数据自然是圆弧结尾的若干数据，补几个就要看最上面的圆弧距离视觉中点的距离，结束处补数据的原理同上。

![](https://i.loli.net/2020/12/12/VEQIO4vrT5dgih8.png)

![](https://i.loli.net/2020/12/12/PEWqUoxs2Lp49h1.gif)

动图看起来比较直观吧，虽然只有四条数据但是实际上有八条数据，多余的数据是为了保证正确的数据显示（这里就需要可是范围内的圆弧不能超过180°），然后每次旋转后在去更新圆弧的角度（例如整体向下旋转了18°，那每段圆弧需要向上偏转18°）以及隐藏的圆弧。

看一下整体结构吧😅

![](https://i.loli.net/2020/12/12/OmKVgREN2Hd49cy.png)

这样我们需要实现的功能基本搞定

### 需要注意的

每段圆弧共用的是同一个圆心，如下图第一段圆弧（扇形）有①那么长的长度，第二段圆弧（扇形）有②那么长的长度，但视觉以及功能上要达到它仅仅是一段圆弧以及它的点击区域也仅仅有圆弧那么大。

![](https://i.loli.net/2020/12/12/1f265wLjOUkmd7t.png)

这里我设置了他们的层级关系（z-index）从左到右层级递减，通过前一级的遮挡实现功能上以及视觉上的效果。到了这里你应该可以自己实现了吧🤗，赶紧动手吧。

## Canvas 饼状图

效果

![](https://i.loli.net/2020/12/12/QwXLN214uo58HSR.png)

你以为就这，年轻人大意了吧

![](https://i.loli.net/2020/12/12/zcrNBvkniQDVXEI.gif)

我还做了“动效”，主要步骤有三步：

1. 画一个整圆弧

2. 给中间添一个空白圆

3. 调整文字位置

   ### 需要注意的

   1：Canvas画圆弧的位置

   ![](https://i.loli.net/2020/12/12/J4q8P6ICmtvBEQd.png)

   2：Canvas 不清晰（源码有代码解决）

   3：Canvas点击是点击整块区域，对于点击事件需要代码判断区域

   ```
   //Canvas画圆弧
   <template>
     <div class="chain_wrap">
         <div :class="['pie_wrap',rotateClass]" >
             <canvas id="pie" ref="pie" class="pie" @click="handleClickEvent" width="260" height="260" ></canvas>
         </div>
         
     </div>
   </template>
   
   <script>
   import {firstChain} from './mockData'
   export default {
       data(){
           return {
               ctx: null,
               angleList: [],
               circleX: 0,
               circleY: 0,
               canRotate: false,
               rotateClass: null,
               ratio: 1,
               rotateAngle: 0,
               colorList: ['#5aa0ec','#66985d','#a4983c','#97522b','#88232c','#6e2c97','#4618c2']
           }
       },
       created(){
   
       },
       mounted(){
           this.createPieCanvas()
       },
       methods:{
           createPieCanvas(initAngle = 0){
               const doughnutHoleSize = 0.2
               let total = 0
               firstChain.forEach(item=> total+= item.count)
               let angleList = []
               firstChain.forEach(item=>{
                   // let angle = Math.PI*2*(item.count/total)
                   let angle = Math.PI*2/ firstChain.length
                   angleList.push(angle)
               })
               this.angleList = angleList
               //保证最小角度
               // angleList = this.handleAngle(angleList,0.2)
               let pieDom = this.$refs.pie
               let ctx = pieDom.getContext('2d')
               this.ctx = ctx
               var ratio = this.getPixelRatio(pieDom)
               this.ratio = ratio
               pieDom.style.width = pieDom.width + 'px'
               pieDom.style.height = pieDom.height + 'px'
               pieDom.width = pieDom.width * ratio;
               pieDom.height = pieDom.height * ratio;
               //使canvas清晰
               ctx.scale(ratio, ratio);
               const h = ctx.canvas.width/ratio
               const w = ctx.canvas.height/ratio
               const x0 = w/2
               const y0 = h/2
               this.circleX = x0
               this.circleY = y0
               let startAngle = 0
               let pieRadius = Math.min(x0,y0)
               let offset = (pieRadius*doughnutHoleSize)/2
               angleList.forEach((item,index)=>{
                   var endAngle = startAngle + item
                   let color  = this.getColor(index)
                   let lableX = x0+ (offset + pieRadius/2) * Math.cos(startAngle+ item/2)
                   let lableY = y0+ (offset + pieRadius/2) * Math.sin(startAngle+ item/2)
                   this.drawPieSlice(ctx,x0,y0,pieRadius,startAngle,endAngle,color) 
                   this.drawPieText(ctx,lableX,lableY,firstChain[index].name,startAngle+ item/2)
                   /*记录当前的结束位置作为下一次的起始位置*/
                   startAngle = endAngle
               })
               //画中间的圆心
               this.drawPieSlice(ctx,x0,y0,doughnutHoleSize*Math.min(x0,y0),0,2*Math.PI,"#0e1134")
           },
           getColor(index) {
               const len = this.colorList.length
              
               return this.colorList[index%len]
           },
           getPixelRatio(context) {
               var backingStore = context.backingStorePixelRatio ||
                   context.webkitBackingStorePixelRatio ||
                   context.mozBackingStorePixelRatio ||
                   context.msBackingStorePixelRatio ||
                   context.oBackingStorePixelRatio ||
                   context.backingStorePixelRatio || 1;
               return (window.devicePixelRatio || 1) / backingStore;
           },
           drawPieSlice(ctx,centerX, centerY, radius, startAngle, endAngle, color ){
               ctx.fillStyle = color;
               ctx.beginPath();
               ctx.moveTo(centerX,centerY);
               ctx.arc(centerX, centerY, radius, startAngle, endAngle);
               ctx.closePath();
               ctx.fill();
           },
           drawPieText(ctx,lableX,lableY,text,rotateAngle){
               // console.log(text,lableX,lableY,rotateAngle)
               if(rotateAngle>0.5*Math.PI && rotateAngle< 1.5*Math.PI) rotateAngle+= Math.PI
               ctx.save()
               ctx.fillStyle = 'white'
               ctx.font = 'bold 10px Arial'
               ctx.translate(lableX,lableY)
               ctx.rotate(rotateAngle)
               ctx.fillText(text,0,0)
               // ctx.translate(lableX,lableY)
               ctx.restore()
           },
           handleAngle(angleList,minAngle){
               //beyondTotal: 超出360度的角度和 normalTotal: 大于minAngle的角度和
               let newAngleList = [],beyondTotal = 0,normalTotal = 0
               angleList.forEach(item=>{
                   if(item< minAngle) beyondTotal += (minAngle - item)
                   else if(item > minAngle) normalTotal += item
               })
               angleList.forEach((item,index)=>{
                   let resultAngle = item
                   if(item< minAngle) resultAngle = minAngle
                   else if(item > minAngle) resultAngle =(item  - beyondTotal*(item/normalTotal))
                   newAngleList[index] = resultAngle
               })
               return newAngleList
           },
           handleClickEvent(e){
               let offsetTop = document.querySelectorAll('.company_chart')[0].offsetTop
               let offsetLeft = document.querySelectorAll('.company_chart')[0].offsetLeft
               let angle = this.getClickAreaAngle(e.pageX - offsetLeft ,e.pageY - offsetTop)
               //清除画布
               this.ctx.clearRect(0,0,this.ctx.canvas.width/this.ratio,this.ctx.canvas.height/this.ratio)
               this.rotateAngle += angle
               this.refreshCanvas(this.rotateAngle)
               // this.rotateClass = this.createRotateAnimation(angle)
               // setTimeout(()=>{this.canRotate = false},0.5)
           },
           refreshCanvas(initAngle){
               const doughnutHoleSize = 0.2
               const h = this.ctx.canvas.width/this.ratio
               const w = this.ctx.canvas.height/this.ratio
               const x0 = w/2
               const y0 = h/2
               this.circleX = x0
               this.circleY = y0
               let startAngle = initAngle
               let pieRadius = Math.min(x0,y0)
               let offset = (pieRadius*doughnutHoleSize)/2
               this.angleList.forEach((item,index)=>{
                   var endAngle = startAngle + item
                   let color  = this.getColor(index)
                   let lableX = x0+ (offset + pieRadius/2) * Math.cos(startAngle+ item/2)
                   let lableY = y0+ (offset + pieRadius/2) * Math.sin(startAngle+ item/2)
                   this.drawPieSlice(this.ctx,x0,y0,pieRadius,startAngle,endAngle,color) 
                   this.drawPieText(this.ctx,lableX,lableY,firstChain[index].name,startAngle+ item/2)
                   /*记录当前的结束位置作为下一次的起始位置*/
                   startAngle = endAngle
               })
                //画中间的圆心
               this.drawPieSlice(this.ctx,x0,y0,doughnutHoleSize*Math.min(x0,y0),0,2*Math.PI,"#0e1134")
           },
           getClickAreaAngle(x,y){
            let angle = Math.atan(Math.abs(this.circleY - y) / Math.abs(this.circleX - x) )
             //根据象限求出实际的角度
             //一象限 二象限 三象限 四象限
             if(x > this.circleX && y < this.circleY){
                 return  angle
             }else if(x > this.circleX && y > this.circleY){
                 return  2*Math.PI - angle 
             }else if (x < this.circleX && y > this.circleY){
                 return Math.PI +  angle
             }else if(x < this.circleX && y < this.circleY){
                 return  Math.PI - angle
             }
           },
           createRotateAnimation(angle){
              let  rotateAngle = (2* Math.PI - angle) * (180/Math.PI) 
              const uid = Math.random().toString(36).substr(2)
              const style = document.createElement('style')
              style.type = 'text/css'
              style.innerHTML = `.rotateClass${uid}{transform: rotate(${rotateAngle}deg)}`
              document.getElementsByTagName('head')[0].appendChild(style)
              return `rotateClass${uid}`
           }
       }
   }
   </script>
   
   <style lang='less' scoped>
       .chain_wrap{
           .pie_wrap{
               width: 260px;
               height: 260px;
               transition: all 0.5s;
                .pie{
               
               // &:hover{
               //     transition: all 0.5s;
               //     transform: rotate(180deg);
               // }
           }
           }
          
       }
   </style>
   ```

   

   

