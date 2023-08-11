<template>
  <div>

  </div>
</template>

<script setup>
// pixi导入
import * as PIXI from 'pixi.js'
import { ShockwaveFilter } from "pixi-filters";
const app = new PIXI.Application({
  width:window.innerWidth,
  height:window.innerHeight,
  backgroundColor: 0x1099bb,
  resolution: window.devicePixelRatio || 1,
  antialias:true,
})
document.body.appendChild(app.view)
//1.导入背景图
const background = PIXI.Texture.from('./resource/bg.jpg')
const spriteBg = new PIXI.Sprite(background)
spriteBg.width = app.screen.width
spriteBg.height = app.screen.height 
const container = new PIXI.Container()
container.addChild(spriteBg)
//2.文字
const text = new PIXI.Text('Hello World',{
  fontFamily:'Arial',
  fontSize: 30+Math.floor(window.innerWidth/10),
  fill: 0xffffff,
  align:"center",
  dropShadow:true,
  dropShadowColor:"#000000",
  dropShadowBlur:4,
  dropShadowAngle:Math.PI/2,
  dropShadowDistance: 20,
})
text.anchor.set(0.5)
text.x = app.screen.width/2
text.y = app.screen.height/2
container.addChild(text)
//3.置换贴图
const displacementSprite = PIXI.Sprite.from('./resource/filterPic.webp')
displacementSprite.texture.baseTexture.wrapMode = PIXI.WRAP_MODES.REPEAT
const displacementFilter = new PIXI.DisplacementFilter(displacementSprite)
displacementSprite.scale.set(5)
app.ticker.add(function(){
  displacementSprite.x+=1
  displacementSprite.y+=1
  Createwave(shockwaveFilter,1)
})
container.addChild(displacementSprite)

app.stage.addChild(container)
//4.震波滤镜
const shockwaveFilter = new ShockwaveFilter(
[ Math.random()*app.screen.width,
  Math.random()*app.screen.height,
],{
  radius:100,//半径
  waveLength:30,//波长
  amplitude:100,//振幅
  nbRipples:1,
  pixelate:false,
  paused:false,
  autoFit:false,
  speed:100//速度
},0
);
function Createwave(filter,resetTime){
  filter.time += 0.01
  if(filter.time>resetTime)
  {
    filter.time = 0;
    filter.center =[Math.random()*app.screen.width,Math.random()*app.screen.height];
  }
}
container.filters=[displacementFilter,shockwaveFilter]
//添加点击事件
app.view.addEventListener("click",function(e){
  shockwaveFilter.center = [e.clientX,e.clientY];
  shockwaveFilter.time = 0;
})
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: 0;
}
canvas{
  width: 100%;
  height: 100%;
  position: fixed;
  left: 0;
  right: 0;
}
</style>
