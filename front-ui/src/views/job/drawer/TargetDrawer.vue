<template>
  <a-drawer
    title="目标数据源"
    placement="right"
    :closable="false"
    :visible="visible"
    :after-visible-change="afterVisibleChange"
    @close="onClose"
    width="500"
  >
    <LoadingDx v-if="loading" size="'size-1x'" />

    <div class="select-container">
      任务名称
      <a-input :value="jobName" @input="handleJobNameInput" />
    </div>

    <div class="select-container">
      <a href="https://tool.lu/crontab" target="_blank">
        定时配置（Spring crontab表达式）
      </a>
      <a-input :value="schedulerConf" @input="handleSchedulerInput" />
    </div>

    <div class="select-container">
      目标数据源
      <a-select
        class="input-full-width"
        :value="target.dsId"
        @change="handleDsChange"
      >
        <a-select-option v-for="ds in dsList" :key="ds.dsId" :value="ds.dsId">
          <div class="ds-option">
            <span class="ds-icon">
              <img :src="dsImgObj[ds.type]" :alt="ds.name">
            </span>
            <span>{{ ds.name }}</span>
          </div>
        </a-select-option>
      </a-select>
    </div>

    <div class="input-container">
      目标数据表
      <a-select
        class="input-full-width"
        :value="target.tableName"
        @change="handleTableChange"
      >
        <a-select-option v-for="table in tables" :key="table" :value="table">
          {{ table }}
        </a-select-option>
      </a-select>
    </div>

    <a-form-item label="字段映射关系">
      <a-row :gutter="16">
        <a-col :span="8">
          <span>来源字段</span>
        </a-col>
        <a-col :span="8">
          <span>目标字段</span>
        </a-col>
      </a-row>

      <a-row
        :gutter="16"
        v-for="(mapping, index) in mappings"
        :key="index"
      >
        <a-col :span="8">
          <a-select
            v-model="mapping.sourceField"
            placeholder="请选择来源字段"
            class="input-full-width"
            @change="handleSourceFieldChange(index)"
          >
            <a-select-option
              v-for="field in sourceFields"
              :key="field"
              :value="field"
            >
              {{ field }}
            </a-select-option>
          </a-select>
        </a-col>
        <a-col :span="8">
          <a-select
            v-model="mapping.targetField"
            placeholder="请选择目标字段"
            class="input-full-width"
            @change="handleTargetFieldChange(index)"
          >
            <a-select-option
              v-for="field in targetFields"
              :key="field.name"
              :value="field.name"
            >
              {{ field.name }}
            </a-select-option>
          </a-select>
        </a-col>
        <a-col :span="4">
          <a-icon
            type="minus-circle-o"
            @click="removeMapping(index)"
            v-show="mappings.length > 1"
          />
        </a-col>
      </a-row>

      <a-row :gutter="16">
        <a-col :span="24">
          <a-button type="dashed" @click="addMapping" style="width: 100%">
            <a-icon type="plus" /> 添加字段映射关系
          </a-button>
        </a-col>
      </a-row>
    </a-form-item>
  </a-drawer>
</template>

<script>
import LoadingDx from '@/components/common/loading-dx.vue'
import { dsImgObj } from '@/views/datasource/const'

export default {
  name: 'TargetDrawer',

  components: {
    LoadingDx
  },

  props: {
    visible: {
      type: Boolean,
      default: false
    },
    loading: {
      type: Boolean,
      default: false
    },
    dsList: {
      type: Array,
      default: () => []
    },
    jobName: {
      type: String,
      default: ''
    },
    schedulerConf: {
      type: String,
      default: ''
    },
    target: {
      type: Object,
      default: () => ({
        dsId: '',
        tableName: ''
      })
    },
    tables: {
      type: Array,
      default: () => []
    },
    sourceFields: {
      type: Array,
      default: () => []
    },
    targetFields: {
      type: Array,
      default: () => []
    },
    mappings: {
      type: Array,
      default: () => []
    }
  },

  data () {
    return {
      dsImgObj
    }
  },

  methods: {
    handleJobNameInput (e) {
      this.$emit('job-name-change', e.target.value)
    },

    handleSchedulerInput (e) {
      this.$emit('scheduler-change', e.target.value)
    },

    handleDsChange (value) {
      this.$emit('ds-change', value)
    },

    handleTableChange (value) {
      this.$emit('table-change', value)
    },

    handleSourceFieldChange (index) {
      this.$emit('source-field-change', index)
    },

    handleTargetFieldChange (index) {
      this.$emit('target-field-change', index)
    },

    addMapping () {
      this.$emit('add-mapping')
    },

    removeMapping (index) {
      this.$emit('remove-mapping', index)
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
.ds-option {
  display: flex;
  align-items: center;
}

.ds-icon {
  float: left;
  width: 24px;
  height: 24px;
  border-radius: 6px;
  overflow: hidden;
  margin-right: 4px;

  img {
    width: 24px;
    height: 24px;
    margin: 0;
    padding: 0;
    border: 0;
  }
}

.select-container,
.input-container {
  margin-bottom: 10px;
}

.input-full-width {
  width: 100%;
}
</style>
