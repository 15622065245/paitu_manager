<template>
  <div class="search">
    <div class="search-block">
      <search-item
        :clear="clear"
        :key="i"
        v-for="(item,i) in searchItems"
        :item="item"
        @on-change="onChange"
      ></search-item>
      <slot></slot>
      <el-button size="small" class="search-btn" icon="el-icon-search" type="success" @click="onSearch">
        {{$t('搜索')}}
      </el-button>
      <el-button size="small" class="reset-btn" icon="el-icon-refresh" @click="onReset">{{$t('重置')}}</el-button>
    </div>
  </div>
</template>

<script>
  import SearchItem from './search-item.vue'
  import {setTimeout} from 'timers'

  export default {
    name: 'SiSearch',
    props: {
      searchItems: {
        type: Array,
        default() {
          return []
        },
      },
    },
    data() {
      return {
        keywords: {},
        clear: false,
      }
    },
    components: {
      SearchItem,
    },
    computed: {},
    methods: {
      onSearch() {
        // 传递表单整体处理好的数据对象keywords给父级
        this.$emit('on-search', this.keywords, {}) // handle seach
      },
      onReset() {
        this.keywords = {}
        this.clear = true
        setTimeout(() => {
          this.clear = false
        }, 200)
        this.$emit('on-search', this.keywords, {}) //  handle seach
      },
      onChange(obj) {
        // 收到子组件传递过来的每个表单obj对象
        let value = obj.value
        if (value === -1 || value === '' || value === undefined) {
          // 对obj对象再进行判断，如果是undefined 或者空或者-1，例如select选中第一项时默认返回-1，此时不需要加入到合并到整体的obj对象中
          delete this.keywords[obj.key] // if value is undefined or "" or -1 delete it
        } else {
          // 将每一个单元表单的obj对象合并为一个整体的obj对象
          this.keywords[obj.key] = obj.value
        }
      },
    },
  }
</script>
<style lang="less" scoped>
  .search {
    overflow: hidden;

    .search-block {
      .search-btn {
        background: #00a16c;
        border: none;
        border-radius: 2px;

        .reset-btn {
          margin-left: 0;
        }
      }
    }
  }
</style>
