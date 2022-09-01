<template>
<div class="e-book">
  <div :class="['e-book-stage',{'e-book-stage-move1': curPage===0, 'e-book-stage-move2': curPage===images.length} ]">
  <div class="mag-bottom-shadow-left"></div>
  <div class="mag-bottom-shadow-right"></div>
  <div class="mag-stage-shadow-left"></div>
  <div class="mag-stage-shadow-right" style="width:21px"></div>

    <div class="mag-left">
      <span>
        <div v-for="(item,index) in images" :key="index"  
        :class="['mag-page',
        'mag-page-left',
        {'mag-page-active':curPage=== index+1 && !flipping && index%2 },
        {'left-enter-to':curPage=== index+1 && flipping && index%2}]">
          <div class="mag-item">
            <img :src="images[index]"  class="mag-image">
              {{index}}
          </div>
        </div>
      </span>

    </div>
     <div class="mag-right">
      <span>
         <div v-for="(item,index) in images" :key="index"  
          :class="['mag-page',
          'mag-page-right',
          {'mag-page-active':curPage === index && !flipping && (!!index%2||index===0)  },
          {'right-enter-to':curPage === index && leftFlipping && (!!index%2|| index===0)}
          ]">
          <div class="mag-item">
            <img :src="images[index]"  class="mag-image">
            {{index}}
          </div>
        </div>
      </span>
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
       timer:null,
       flipping :false,
       leftFlipping:false,
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
  mounted(){
    this.refresh(true)
  },
  unmounted(){
    clearTimeout(this.timer)
  },
  methods:{
    right(){  
      if(this.flipping || this.leftFlipping){
        return
      }
      if(this.curPage >= this.images.length-1){
        window.alert('已翻到最后一页面')
        return
      }
      //right 按钮 =》 左边的页面从右向左运动
      this.curPage +=2
      this.flipping = true
      console.log(this.curPage, this.images.length-1);
      this.$nextTick(()=>{
        this.refresh()
      })
      this.timer =  setTimeout(()=>{
        this.flipping = false
      },500)
    },
    left(){
      if(this.leftFlipping || this.flipping){
        return
      }
      if(this.curPage <= 0){
        window.alert('已翻到第一页')
        return
      }
      //left 按钮 =》 右边的页面从左向右运动
      this.curPage -=2
      this.leftFlipping = true
      this.$nextTick(()=>{
        this.refresh()
      })
      this.timer = setTimeout(()=>{
        this.leftFlipping = false
      },500)

    },
    refresh(){
      const lefts = document.querySelectorAll('.mag-page-left')
      for(let i=0; i< lefts.length;i++){

        if(i === this.curPage-1){
          //去掉display = 'none' => 展示出来
          lefts[i].removeAttribute('style')
        }else{
          // 除去再下面一层的左侧页面外隐藏
          if(!(this.flipping && i === this.curPage-3)){
             lefts[i].style.display = 'none'
          }
        }
      }
      const rights = document.querySelectorAll('.mag-page-right')
      for(let i=0; i< rights.length;i++){
        if(i === this.curPage){
          rights[i].removeAttribute('style')
        }else{
          // 除去再下面一层的右侧页面外隐藏
          if(!(this.leftFlipping && i === this.curPage+2)){
            rights[i].style.display = 'none'
          }
        }
      }
      
    }
 
  }
}
</script>

<style lang="less" scoped>

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

@keyframes left-enter-to {
  0%{
    left: 150%;
    width: 50%;
  }  
   100%{
    left:0;
    width:100%;
   }
}

@keyframes right-enter-to {
  0%{
    right:150%;
    width: 20%;
  }  
   100%{
    right:0;
    width:100%;
   }
}
.e-book{
  .mag-bottom-shadow-left{
    position: absolute;
    width: 20px;
    height: 100%;
    top:1px;
    left: 50%;
    transform: translateX(-100%);
    opacity: 0.8;
    background-image: linear-gradient(90deg, rgba(0,0,0,0) 100%, rgba(0,0,0,0.1) 0% );
  }
  .mag-bottom-shadow-right{
    position: absolute;
    width: 20px;
    height: 100%;
    top:1px;
    right: 50%;
    opacity: 0.5;
    transform: translateX(100%);
    background-image: linear-gradient(90deg, rgba(0,0,0,0.1) 0%,  rgba(0,0,0,0) 100%);
  }
  .mag-stage-shadow-right, .mag-stage-shadow-left {
    position: absolute;
    height: 100%;
    top: 0px;
    transition: width 0.5s ease;
    background-size: 100% 100%;
  }

  .mag-stage-shadow-right{
    right: 0;
    opacity: 0.5;
    transform: translateX(100%);
    background-image: linear-gradient(90deg, rgba(0,0,0,0.1) 0%,  rgba(0,0,0,0) 100%);


  }

  position: absolute;
  height: 100%;
  left: 0;
  width: 100%;
  background: #eaeaea;
  background-size: 100% 100%;

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
  .e-book-stage{
    width: 80%;
    height: 100%;
    position: relative;
    margin: 0 auto;
    transition: transform 0.5s ease ;
     &.e-book-stage-move1{
       transform: translate(-25%, 0%);
     }
      &.e-book-stage-move2{
       transform: translate(25%, 0%);
     }
     .mag-left{
       position: absolute;
       left: 0;
       top: 0;
       width: 50%;
       height: 100%;
     }

     .mag-right{
       position: absolute;
       right: 0;
       top: 0;
       width: 50%;
       height: 100%;
     }

     .mag-page{
       position: absolute;
       width: 100%;
       height: 100%;
       top: 0;
       overflow: hidden;
       box-sizing: border-box;
       background-color: #fff;
       border: 1px solid #b6b6b6;
       &-active{
         z-index: 2;
       }
       &-left{
         left: 0;

         &.left-enter-to{
           z-index: 2;
           animation: left-enter-to 0.5s ease forwards
         }

       }

       &-right{
         right: 0;

        //  &.right-enter-to{
        //    z-index: 1;
        //  }

         &.right-enter-to{
           z-index: 2;
           animation: right-enter-to 0.5s ease forwards
         }
       }
        .mag-item{
          position: absolute;
          height: 100%;
          .mag-image{
            height: 97%;
            width: auto;
          }
           img{
              vertical-align: top;
            }
        }

     }
    
    
  }
}

</style>
