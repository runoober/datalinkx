<template>
  <a-drawer
    title="动态编译算子"
    placement="right"
    :closable="false"
    :visible="visible"
    :after-visible-change="afterVisibleChange"
    @close="onClose"
    width="500"
  >
    <div class="dynamic-hint">
      <p>动态编译算子允许您编写自定义代码进行处理</p>
    </div>

    <div class="dynamic-content">
      <span class="dynamic-label">源代码</span>
      <a-textarea
        :value="sourceCode"
        style="height: 300px;"
        placeholder="请输入源代码..."
        @input="handleSourceCodeInput"
      />
    </div>
  </a-drawer>
</template>

<script>
export default {
  name: 'DynamicDrawer',

  props: {
    visible: {
      type: Boolean,
      default: false
    },
    sourceCode: {
      type: String,
      default: ''
    }
  },

  methods: {
    handleSourceCodeInput (e) {
      this.$emit('source-code-change', e.target.value)
    },

    afterVisibleChange (val) {
      this.$emit('visible-change', val)
    },

    onClose () {
      this.$emit('close')
    }
  }
}
</script>

<style lang="less" scoped>
.dynamic-hint {
  margin-bottom: 12px;
  padding: 8px;
  background-color: #e6f7ff;
  border: 1px solid #91d5ff;
  border-radius: 4px;

  p {
    margin: 0;
    color: #0050b3;
  }
}

.dynamic-content {
  .dynamic-label {
    display: block;
    margin-bottom: 8px;
    font-weight: 500;
  }
}
</style>
