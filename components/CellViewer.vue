<template>
  <!-- TimeTableViewer에서 하나의 Cell을 담당합니다. -->
  <div ref="cell" :style="`height: ${height}em;`" class="cell">
    <span v-if="data" class="item-parent">
      <span v-if="data.공강">
      </span>
      <span v-else class="item">
        {{data.상세.getSubject()}}({{data.상세.getPf()}})
        <span class="item-hover">
          <div>
          {{data.상세.getSnoCode()}}
          {{data.시작시간}} ~ {{data.종료시간}} <br>
          {{data.상세.getDetailPlace()}} <br>
          {{data.상세.getBigo()}}
          </div>
        </span>
      </span>
    </span>
    <span v-else class="item-parent">
      <span v-if="time" class="item">
        {{time}}교시
        <span class="item-hover-time">
          <span v-if="time%2==1">
            {{`${time/2+8.5}시 ~ ${time/2+8.5}시 30분`}}
          </span>
          <span v-else>
            {{`${time/2+8}시 30분 ~ ${time/2+9}시`}}
          </span>
        </span>
      </span>
      <span v-if="text"> {{text}}
      </span>
    </span>
  </div>
</template>
<script>
export default {
  name: 'cell-viewer',
  props: ['data', 'height', 'time', 'text'],
  data(){
    return {
      colors: ['#FFB2A3', '#FFE0A3', '#EFFFA3', '#C1FFA3', '#A3FFB2', '#A3FFE0', '#A3EFFF', '#A3C1FF', '#B2A3FF']
    }
  },
  methods: {
    getColor(i){
      if(i === undefined || i === -1){ return '#FFFFFF'}
      return this.colors[i<this.colors.length?i:this.colors.length-1]
    },
    changeColor(){
      if(this.data && !this.data.공강){
        this.$refs.cell.style['background-color'] = this.getColor(this.data.index)
      }
      if(this.data && this.data.공강){
        this.$refs.cell.style['background-color'] = '#FFFFFF'
      }      
    }
  },
  mounted (){
    this.changeColor()
  },
  watch:{
    data () {
      this.changeColor()
    }
  }
}
</script>
<style scoped>
.cell {
  border: 0.1px solid white;
  
  margin: 0 auto;
}
.item-parent {
  position: relative;
}
.item-hover {
  width: 300px;
  position: absolute;
  display: none; 
  background: rgba(0,0,0,0.8);
  color:white;
  z-index: 12;
  border-radius: 5px;
  padding: 5px;
}

.item-hover-time {
  width: 130px;
  position: absolute;
  display: none; 
  background: rgba(0,0,0,0.8);
  color:white;
  z-index: 12;
  border-radius: 5px;
  padding: 5px;
}
.item-parent:hover .item-hover{
  display: block;
}

.item-parent:hover .item-hover-time{
  display: block;
}
</style>


