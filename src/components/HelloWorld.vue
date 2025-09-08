<template>
  <div class="block-container">
    <div class="outWrapper" :style="{width:  `${(numOfCube+2)*2*margin + numOfCube*blockSize + 2*borderSize}px` }">
      <!-- <button @click="sout">sout</button> -->

      <div class="line" :style="{
        width:  `${(numOfCube)*2*margin + numOfCube*blockSize }px`,
        height:  `${(numOfCube)*2*margin + numOfCube*blockSize }px`,
        left : `${2*margin+borderSize}px`,
        top : `${2*margin+borderSize}px`,
        }">

      </div>

      <div v-if="setting.length" v-for="rowIndex in range(this.numOfCube+2)" class="row" :key="rowIndex">
          <div
            @click="handeleClick($event,rowIndex,blockIndex)"
 
            v-for="(blockIndex) in range(numOfCube+2)"
            :key="`${rowIndex}-${blockIndex}`"
            :style="{
              'background-color': setting[rowIndex*(numOfCube+2)+blockIndex].isActive ? setting[rowIndex*(numOfCube+2)+blockIndex].color :'' ,
              margin: `${margin}px`,
              height: setting[rowIndex*(numOfCube+2)+blockIndex].isHigh ? `${blockSize}px` : `${borderSize}px`,
              width: setting[rowIndex*(numOfCube+2)+blockIndex].isWide  ? `${blockSize}px` : `${borderSize}px`
            }"
          >

          </div>
        </div>


    </div>
    <div :style="{width:  `${(numOfCube+2)*2*margin + numOfCube*blockSize + 2*borderSize}px`}">
      {{ meth }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    numOfCube: {
      type: Number,
      default: 3
    },
    blockSize: {
      type: Number,
      default: 100
    },
    borderSize: {
      type: Number,
      default: 20
    },
    margin: {
      type: Number,
      default: 5
    },
    blockColor: {
      type: String,
      default: 'rgba(31, 210, 120, 0.634)'
    },
    borderColor: {
      type: String,
      default: 'rgb(75, 26, 26)'
    },
    strr: {
      type: String,
      default: "AABBB,A010A,B111B,B110B,BBBAB,"
    },
    meth: {
      type: String,
      default: "123"
    },

  },
  data(){
    return {
      setting:[],
      soutStr:""
    }
  },
  mounted(){
    this.layout()
  },
computed: {

  },
  methods: {
      sout(){

    const numOfCube = this.numOfCube;
    var s = '';
    for(let i =0; i <(numOfCube+2)*(numOfCube+2); i++){
      let cur = (this.setting[i])
      let isSide = (i % (numOfCube+2) == 0 ) || (i % (numOfCube+2) == (numOfCube+1) )

      let nextChar = ''
      s += cur.isActive ? 1 :0;

      if((i+1) % (numOfCube+2) == 0 ){
         s += ","
      }
    }


    console.log(s)
  },
  createBlock(isBlock, isHigh, isWide, color,isActive) {
    const block = {}; // 先创建空对象
    // 用 this.$set 添加每个属性，确保响应式
    this.$set(block, "isBlock", isBlock);
    this.$set(block, "isHigh", isHigh);
    this.$set(block, "isWide", isWide);
    this.$set(block, "color", color);
    this.$set(block, "isActive", isActive);
    return block; // 现在 block 是响应式的
  },
  getBackgroundColor(isBlock,isActive){
    if(isBlock){
        return this.blockColor;
    }else{
        return this.borderColor;
    }
  },

  layout() {
    const numOfCube = this.numOfCube;

    const ret = [];
    // var rowIndex = 0;
    var testStr = this.strr
    console.log(testStr)
    testStr = testStr.split(",").join("");  // ✅ 先按逗号分割成数组，再合并
    
    console.log(testStr)
    console.log("testStr")
    var temp =  []
    

    for(let i =0; i <(numOfCube+2)*(numOfCube+2); i++){
      let row = Math.floor (i/(numOfCube+2));
      let col = i%(numOfCube+2);
      let isHigh = !((row==0) || (row == numOfCube+1));
      let isWide = !((col==0) || (col == numOfCube+1));
      let isActive = (testStr[i]== 'A') || (testStr[i]== '1');
      let isBlock = true;
      console.log(row+" " + col)


    temp.push(this.createBlock(isBlock ? 1 : 0,
     isHigh? 1 : 0 ,
     isWide? 1 : 0, 
      this.getBackgroundColor(isBlock,isActive),
      isActive
    ))


    }

    this.setting = temp;
    this.setting[0].isActive = 0;
    this.setting[(numOfCube+2)*(numOfCube+2)-1].isActive = 0
    return;
    } ,
    handeleClick(evevt,row,col){
      if(row==0 && col==0){
        this.sout()
        return;
      }
      const index = row * (this.numOfCube + 2) + col;
      this.$set(this.setting[index], 'isActive', !this.setting[index].isActive); // ✅ 正确用法
    },
    range(num){
      let ret =[];
      for(let i=0;i<num;i++){
        ret.push(i);
      }
      return ret;
    }
  }
};
</script>

<style lang="less" scoped>
.block-container {
  .outWrapper{
    position: relative;
    // border: 1px solid black;
  }
  .line{
    position: absolute;
    border: 1px solid black;
    z-index: -1;

  }
  .row {
    display: flex;
    justify-content: center;
    
    > div {
      box-sizing: content-box;
      transition: all 0.3s ease;

    }
  }
}


</style>