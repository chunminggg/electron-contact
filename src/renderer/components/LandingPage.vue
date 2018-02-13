<template>
  <div>
    <main>
      <Row>
         <Col span="8">
       <div v-for="(item,index) in items" :key="index" class="item" @click="openSingleMan(index)">
          <Checkbox v-model="item.isChoose"></Checkbox>
          <Avatar shape="square" icon="person"  />
          <span>{{item.name}}</span>
       </div>
      </Col>
        <Col span="16">
          <Form :model="formItem" :label-width="80" v-show="isShowForm">
             <FormItem label="名字">
            <Input v-model="formItem.name" placeholder="请输入名称"></Input>
        </FormItem>
        <FormItem label="手机">
            <Input v-model="formItem.phoneNumber" placeholder="请输入手机号"></Input>
        </FormItem>
      <FormItem label="邮箱">
            <Input v-model="formItem.emial" placeholder="请输入邮箱"></Input>
        </FormItem>
        <FormItem label="住址">
            <Input v-model="formItem.address" placeholder="请输入住址"></Input>
        </FormItem>
        <FormItem label="备注">
            <Input v-model="formItem.comment" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="请输入备注..."></Input>
        </FormItem>
        <FormItem>
            <Button type="primary" @click="modifyUser">修改</Button>
            <Button type="ghost" style="margin-left: 8px" @click="removeUser">移除</Button>
        </FormItem>
    </Form>
        </Col>
      </Row>
    </main>
    <footer>
         <Checkbox v-model="isAllSelected">全选</Checkbox>
        <span>已选{{selectedLength}}个联系人</span>
        <span>共{{items.length}}个联系人</span>
          <Button type="primary" size="small" @click="addUser">添加联系人</Button>
    </footer>
  </div>
</template>

<script>
import userData from "./data/mock";
export default {
  name: "landing-page",
  components: {},
  watch:{
    isAllSelected:function(newVal,oldVal){
      if(newVal == true){
        for (let item of this.items) {
          item.isChoose = true
        }
      }
      else {
         for (let item of this.items) {
          item.isChoose = false
        }
      }
    },
    items:{
      deep:true,
      handler:function(newVal,oldVal){
        this.selectedLength = 0
         for (let item of this.items) {
           if (item.isChoose == true) {
             this.selectedLength += 1
           }
        }
      }
    }
  },
  data() {
    return {
      single: false,
      items: userData,
      isAllSelected: false,
      selectedIndex:null,
      formItem: userData[0],
      isShowForm:false,
      selectedLength:0,
    };
  },
  methods: {
    openSingleMan(index) {
      this.isShowForm = true
      this.formItem = this.items[index]
      this.selectedIndex = index;
    },
    modifyUser(){
      this.items[this.selectedIndex] = this.formItem
      this.$Message.success('修改成功')
      this.isShowForm = false
      setTimeout(function(){
        this.isShowForm = true
      },1000)
    },
    removeUser(){
      this.items.splice(this.selectedIndex,1)
      this.isShowForm = false
      this.$Message.success('删除成功')
    },
    addUser(){
      this.items.push({name:`miao${this.items.length+1}`,isChoose:false,phoneNumber:'',email:'',address:'',comment:''})
      this.openSingleMan(this.items.length -1)

    },
  }
};
</script>

<style>
.item {
  margin: 10px;
  cursor: pointer;
}
</style>
