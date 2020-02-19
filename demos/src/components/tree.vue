<template>
  <div class='menu'>
    <div v-for="(item, index) in menuLists" :key="index" class="menuList">
       <template v-if="!item.children">
          <h5 @click="clicks($event,index)" :style="{'margin-left': item.type * 20 + 'px'}"> 
          <p>
            <img src="~@/assets/arrow.png" alt="" v-if="item.children && item.children.length" :style="{'transform': item.flag? 'rotate(0deg)':'rotate(-90deg)'}"/>
            <span>{{item.name}}</span>
          </p>
          <p class='edit'>
            <span v-if=" index !== 0" @click="moves($event,'up', index)">上移</span>
            <span v-if=" index !== menuLists.length - 1" @click="moves($event,'down', index)">下移</span>
            <span class='addNote' v-if="item.type !== 3" @click="addmenus($event,item.type, index)">添加下一级</span>
            <span @click="deletes($event,index)">删除</span>
          </p>
        </h5>
       </template>
        <template v-else>
          <h5 @click="clicks($event,index)" :style="{'margin-left': item.type * 20 + 'px'}"> 
          <p>
            <img src="~@/assets/arrow.png" alt="" v-if="item.children && item.children.length" :style="{'transform': item.flag? 'rotate(0deg)':'rotate(-90deg)'}"/>
            <span>{{item.name}}</span>
          </p>
           <p class='edit'>
            <span v-if=" index !== 0" @click="moves($event,'up', index)">上移</span>
            <span v-if=" index !== menuLists.length - 1" @click="moves($event,'down', index)">下移</span>
            <span class='addNote' v-if="item.type !== 3" @click="addmenus($event,item.type, index)">添加下一级</span>
            <span @click="deletes($event,index)">删除</span>
          </p>
          </h5>
          <menus :menuLists='item.children' v-if="item.flag"></menus>
        </template>
        <div class="modal" v-show="modalFlag">
         <div class="modal_content">
            <p class="close"><b></b><span>编辑名称</span><span @click="cancel">X</span></p>
            <input type="text" @change="changeAdd">
            <div>
              <button @click="cancel">取消</button>
              <button style="background:#3385ff;color: #fff" @click="sure">确认</button>
            </div>
         </div>
        </div>
    </div>
  </div>
</template>

<script>
// import menuList from './menuList.vue'
export default {
  name: 'menus',
  props: ['menuLists'],
  data() {
    return {
      num: 0,
      menus: '',
      index: -1,
      type: 0,
      modalFlag: false
    }
  },
  components: {
  },
  created() {
  },
  methods: {
    clicks(e, index) {
      this.$set(this.menuLists[index], 'flag', !this.menuLists[index].flag)
    },
     changeAdd(e) {
      this.menus = e.target.value
    },
    cancel() {
      this.modalFlag = false
    },
    addmenus(e, type, index) {
      e.stopPropagation()
      this.type = type
      this.index = index
      this.modalFlag = true

    },
    sure() {
      let obj = {
        type: this.type + 1,
        id: '1' + this.index,
        name: this.menus,
        flag: false
      }
      if(this.menuLists[this.index].children) {
        this.menuLists[this.index].children.push(obj)
      } else {
        this.$set(this.menuLists[this.index], 'children', [])
        this.menuLists[this.index].children.push(obj)

      }
     this.menus = ''
     this.modalFlag = false
      // console.log(this.menus)
    },
    deletes(e,index) {
      e.stopPropagation()
      this.menuLists.splice(index, 1)
    },
    moves(e,type, index) {
      e.stopPropagation()
      if(type ==='up') {
        this.menuLists[index] = this.menuLists.splice(index - 1, 1, this.menuLists[index])[0];
      } else {
        this.menuLists[index] = this.menuLists.splice(index + 1, 1, this.menuLists[index])[0];
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang='less' scoped>
  .menu {
    width: 600px;
    .edit {
      display: none;
    span {
      margin: 0 5px;
    }
  }
    img {
      transform: rotate(-90deg);
    }
    .menuList {
      cursor: pointer;
      h5 {
        display: flex;
        height: 30px;
        align-items: center;
        justify-content: space-between;
        &:hover  {
          p {
          display: block;
          }
          background: #eeeeee;
        }
      }
    }
    .modal {
      position: fixed;
      top: 0;
      left: 0;
      background: rgba(0, 0, 0, 0.5);
      width: 100%;
      height: 100%;
      display: flex;
      justify-content: center;
      align-items: center;
      .modal_content {
        display: flex;
        flex-direction:column;
        align-items:center;
        justify-content: space-between;
        width: 400px;
        height: 200px;
        border-radius: 6px;
        background: #fff;
        padding:30px;
        box-sizing:border-box;
        .close{
          display: flex;
          width: 100%;
          justify-content: space-between;
        }
        input{
          width: 300px;
          height: 45px;
          border-radius: 6px;
          outline: none;
          border:solid 1px #ccc;
        }
        button{
          width: 100px;
          height: 45px;
          background: none;
          border-radius: 6px;
          border: solid 1px #ccc;
          margin-left: 10px;
          outline: none;
        }
      }
    }
  }
</style>
