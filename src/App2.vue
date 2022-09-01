<template>
<div class="e-book">
 <div :class="['container',{'flipping':curPage },{'flipping-end':curPage >= images.length }]">
   <div 
   v-for="(item,index) in images" 
   :key="index"  
   :style="{backgroundImage:'url('+item+')'}" 
   :class="['image', 
   {'front':index === curPage  },
   {'back':index === curPage + 1  },
   {'back-animation':index === curPage + 1  && leftFlipping },
   {'next':index === curPage + 2  },
    {'left-front':index === curPage-1  },
    {'left-back':index === curPage-2  },
    {'left-back-animation':index === curPage -2  && rightFlipping },
     {'before':index === curPage - 3  },
   
  
   
   ]">
   {{index}}
   </div> 
 </div>
 <button type="button" class="left"  @click="left" >left</button>
 <button type="button"  @click="right" class="right">right</button>
</div>

</template>

<script>

export default {
  name: 'App',
  components: {
  },
  data(){
    return{
       curPage:0,
       orientation:'right',
       leftFlipping:false,
       rightFlipping:false,
       images:[
          './assests/1.jpg',
          './assests/2.jpg',
          './assests/3.jpg',
          './assests/4.jpg',
          './assests/1.jpg',
          './assests/2.jpg',
          './assests/3.jpg',
          './assests/4.jpg',
          './assests/1.jpg',
          './assests/2.jpg',
          './assests/3.jpg',
          './assests/4.jpg',
       ]
    }
  },
  methods:{
    right(){
      this.$nextTick(()=>{
        console.log('curPage', this.curPage); 
        if(this.curPage >= this.images.length){
          window.alert('您已到了最后一页')
          return 
        }
         this.leftFlipping = true
        const front =document.querySelector('.front');
        const next =document.querySelector('.next');
        if(next){
          next.style.visibility="visible"
        }
        const back =document.querySelector('.back');
        front.style.visibility = 'hidden'
         back.style.visibility="visible"
        setTimeout(()=>{
          front.style.transform = "translateX(-800px) scaleX(0.7)"
          back.style.transform = "translateX(-800px) scaleX(1)"
          this.curPage+=2      
          this.leftFlipping = false
        },700)
      })
     

    },
    left(){
      this.$nextTick(()=>{
        if(!this.curPage){
          window.alert('您已到了第一页')
          return
        }  
        const before =document.querySelector('.before');
        if(before){
          before.style.visibility="visible"
        }      
        const front =document.querySelector('.left-front');
        const back =document.querySelector('.left-back');
        back.style.visibility="visible"
        this.rightFlipping = true
        setTimeout(()=>{ 
          front.style.visibility = 'hidden'
          front.style.transform = "translateX(0px) scaleX(0.7)"
          back.style.transform = "translateX(0px) scaleX(1)"
          this.curPage-=2 
          this.rightFlipping = false
        },700)
      })
  


    // }
   
  }
}
}
</script>

<style lang="less" scoped>
@keyframes left-flipping {
  0%{
    transform: translateX(0px) rotateY(0) scaleX(0.7);
  }

  100%{
  
  }
  
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.e-book{
  button{
    width: 100px;
    height: 50px;
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    font-size: 24px;

    &.right{
      right: 10px;
    }

    &.left{
      left: 10px;
    }
  }

  

}
.container{
  width: 800px;
  height: 900px;
  position: absolute;
  top: 50%;
  left: 50%;
  background: transparent;
  transform: translate(-50%, -50%);
  background-size: cover;
  transform-style: preserve-3d; 
  transition:all 0.4s ease-in;
  
  &.flipping{
    margin-left: 400px;

  }
   &.flipping-end{
    margin-left: 800px;

  }
  .image{
    position: absolute;
    width:100%;
    height: 100%;
    background-size: cover;
    visibility: hidden;
    z-index: -2;
  }
   
 .back{
    background-size: cover;
    z-index: 2;
    visibility: hidden;
    transform-origin: right;
    transform: translateX(0px) scaleX(0.7);
    &.back-animation{
      visibility: visible;
      transition:all  0.8s ease-in;
      transform: translateX(-800px)  scaleX(1);
    }
  }

  .left-back{
      background-size: cover;
      z-index: 1;
      visibility: hidden;
      transform-origin: left;
      transform: translateX(-800px) scaleX(0.7);
     &.left-back-animation{
      visibility: visible;
      transition:all  0.8s ease-in;
      transform: translateX(0px)  scaleX(1);
    }
  }
  .left-front{
    z-index: 2;
    transform-origin: right;
  }
 
  .left-front, .front { 
    visibility: visible;
    background-size: cover; 
    transition:all 0.5s ease-in;
  }

  .front{
    z-index: 3;
    transform-origin: left;
  }


  
  .before, .next {
    background-size: cover;
    visibility: hidden;
    z-index: -1;
  }
  .before{
      z-index: -2;
  }

  
}
</style>
