<template>
  <a-drawer
    title="大模型算子"
    placement="right"
    :closable="false"
    :visible="visible"
    :after-visible-change="afterVisibleChange"
    @close="onClose"
    width="500"
  >
    <div class="llm-hint">
      <p>1、如果需要引用来源表字段使用${}，例：${division_name}</p>
    </div>

    <div class="llm-content">
      <span class="llm-label">大模型prompt</span>
      <a-textarea
        :value="prompt"
        style="height: 121px;"
        placeholder="所有字段都将可以用作模型输入，直接使用字段名称编写prompt，只支持英文，例：Determine whether someone is Chinese or American by their name(根据名字判断某人是中国人还是美国人)"
        @input="handlePromptInput"
      />
    </div>
  </a-drawer>
</template>

<script>
export default {
  name: 'LlmDrawer',

  props: {
    visible: {
      type: Boolean,
      default: false
    },
    prompt: {
      type: String,
      default: ''
    }
  },

  methods: {
    handlePromptInput (e) {
      this.$emit('prompt-change', e.target.value)
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
.llm-hint {
  margin-bottom: 12px;
  padding: 8px;
  background-color: #fffbe6;
  border: 1px solid #ffe58f;
  border-radius: 4px;

  p {
    margin: 0;
    color: #d48806;
  }
}

.llm-content {
  .llm-label {
    display: block;
    margin-bottom: 8px;
    font-weight: 500;
  }
}
</style>
