<!--
 * @Author: kuikui.Ge
 * @Date: 2021-01-25
 * @LastEditors: kuikui.Ge
 * @FilePath: \resource\treeDemo2.vue
 * @Description: 树组件扩展，方法二
-->
<template>
  <div class="container">
    <a-row>
      <a-col class="left"
             :span="8">
        <a-tree ref="tree"
                v-model="checkedKeys"
                :tree-data="treeData"
                show-icon
                checkable
                @check="onCheck">
          <a-icon slot="user"
                  type="user" />
        </a-tree>
      </a-col>
      <a-col class="right"
             :span="16">
        <div class="result">
          <ul class="result-inner"
              v-if="Object.keys(userMap).filter(v=>!!userMap[v].checked).length>0">
            <li v-for="item in user"
                v-show="!!userMap[item.key].checked"
                :key="item.key">
              {{item.title}}
              <span class="btn-delete"
                    @click="()=>deleteUser(item)">删除</span>
            </li>
          </ul>
          <p v-else>请选择节点</p>
        </div>
      </a-col>
    </a-row>
  </div>
</template>

<script>
import { parseSimpleTreeData } from 'ant-design-vue/lib/vc-tree-select/src/util'
import { conductCheck } from 'ant-design-vue/lib/vc-tree/src/util';
export default {
  data () {
    return {
      checkedKeys: [],
      treeData: [],
      dept: [{
        title: '开发部',
        key: '0-0',
      }],
      user: [
        { title: '张三', key: '0-0-0', parentId: '0-0', slots: { icon: 'user' }, isUser: true },
        { title: '李四', key: '0-0-1', parentId: '0-0', scopedSlots: { icon: 'user' }, isUser: true },
      ],
      userMap: {},
      oriCheckedKeys: [],
      oriHalfCheckedKeys: [],
    }
  },
  methods: {
    buildTree () {
      const { dept, user } = this
      const treeData = [...dept, ...user]
      this.userMap = user.reduce((a, c) => {
        c.checked = false
        a[c.key] = c
        return a
      }, {})
      this.treeData = parseSimpleTreeData(treeData, { id: 'key', pId: 'parentId', rootPId: null })
    },
    onCheck (checkedKeys, info) {
      const { checked } = info;
      const update = (node) => {
        const { children = [] } = node;
        if (children.length) {
          children.map((v) => update(v));
        } else {
          if (node.isUser) {
            const { key } = node;
            this.userMap[key].checked = checked
          }
        }
      };

      update(info.node.dataRef);
      this.oriCheckedKeys = checkedKeys;
      this.oriHalfCheckedKeys = info.halfCheckedKeys;
    },
    // 基于ant-design-vue的conductCheck方法，更新选中的节点
    updateTreeNode (checked, id) {
      const { _keyEntities: keyEntities } = this.$refs.tree.$refs.tree._data;
      const { oriCheckedKeys, oriHalfCheckedKeys } = this;
      const { checkedKeys, halfCheckedKeys } = conductCheck([id], checked, keyEntities, {
        checkedKeys: oriCheckedKeys, //上一次的选中状态
        halfCheckedKeys: oriHalfCheckedKeys, //上一次的半选状态
      });
      this.checkedKeys = checkedKeys;
      this.oriCheckedKeys = checkedKeys;
      this.oriHalfCheckedKeys = halfCheckedKeys;
    },
    deleteUser (data) {
      this.userMap[data.key].checked = false
      this.updateTreeNode(false, data.key)
    }
  },
  mounted () {
    this.buildTree()
  }
}
</script>

<style lang="scss" scoped>
.container {
  width: 700px;
  margin: 50px auto;
  border: 1px solid green;
}
.left {
  height: 380px;
  padding: 16px;
  border-right: 1px solid green;
  text-align: left;
}
.right {
  height: 380px;
  padding: 16px;
}
.result {
  text-align: left;
}
.result-inner {
  margin: 0;
  padding: 0;
}
.result-inner li {
  display: inline-block;
  padding: 4px 6px;
  list-style: none;
  border: 1px solid green;
  border-radius: 4px;
  font-size: 14px;
}
.result-inner li {
  margin-right: 8px;
}
.btn-delete {
  display: inline-block;
  font-size: 12px;
}
.btn-delete:hover {
  cursor: pointer;
}
</style>