<template>
  <div class="about">
    <div class="list">
      <div class="title">
        <div>字段名称</div>
        <div>字段类型</div>
        <div>是否必填</div>
        <div>字段选项</div>
        <div>操作</div>
      </div>
      <div class="item" v-for="(p, index) in List" :key="index">
        <div>{{ p.name }}</div>
        <div>{{ p.type }}</div>
        <div>{{ p.require }}</div>
        <div>{{ p.choise }}</div>
        <div><button @click="showModal(p, index)">编辑</button></div>
      </div>
    </div>

    <a-modal
      v-if="visible"
      v-model:visible="visible"
      title="编辑"
      :footer="null"
    >
      <a-form
        :model="formState"
        :label-col="labelCol"
        :wrapper-col="wrapperCol"
      >
        <a-form-item
          label="字段名称"
          :rules="[{ required: true, message: '字段名称' }]"
        >
          <a-input v-model:value="formState.name" />
        </a-form-item>
        <a-form-item
          name="type"
          label="字段类型"
          :rules="[{ required: true, message: '字段类型' }]"
        >
          <a-select
            v-model:value="formState.type"
            @change="handleChange"
            :options="
              ['单行文本', '日期', '单选下拉'].map((a) => ({ value: a }))
            "
          />
        </a-form-item>
        <a-form-item
          label="是否必填"
          :rules="[{ required: true, message: '是否必填' }]"
        >
          <a-switch v-model:checked="formState.require" />
        </a-form-item>
        <a-form-item
          :label="formState.type ? formState.type : '字段选项'"
          :rules="[{ required: true, message: '字段选项' }]"
        >
          <a-select
            v-model:value="formState.choise"
            :mode="formState.type == '单选下拉' ? 'multiple' : 'radio'"
            :options="
              choise[formState.type].map((a) => ({
                value: a,
              }))
            "
          />
        </a-form-item>
        <a-form-item :wrapper-col="{ span: 14, offset: 4 }">
          <a-button type="primary" @click="onSubmit">确定</a-button>
          <a-button style="margin-left: 10px" @click="cancel">取消</a-button>
        </a-form-item>
      </a-form>
    </a-modal>
  </div>
</template>

<script>
import { ref } from "vue";
import { message } from "ant-design-vue";
export default {
  setup() {
    const index = ref(0);
    const List = ref([
      {
        name: "测试111",
        type: "单行文本",
        require: "是",
        choise: "无",
      },
      {
        name: "测试222",
        type: "日期",
        require: "否",
        choise: "年-月-日",
      },
      {
        name: "测试333",
        type: "单选下拉",
        require: "否",
        choise: "选项一、选项二",
      },
      {
        name: "测试444",
        type: "单行文本",
        require: "是",
        choise: "无",
      },
      {
        name: "测试555",
        type: "日期",
        require: "否",
        choise: "年-月-日",
      },
      {
        name: "测试666",
        type: "单选下拉",
        require: "是",
        choise: "选项一、选项二",
      },
    ]);
    const formState = ref({
      name: "",
      type: "单行文本",
      require: false,
      choise: "",
    });

    const visible = ref(false);
    const showModal = (p, idx) => {
      console.log((List.value)[idx].name, formState);
      index.value = idx;
      visible.value = true;
      // formState.value = {
        formState.value.name = (List.value)[idx].name,
        formState.value.type =  (List.value)[idx].type,
        formState.value.require = (List.value)[idx].require == '是' ? true : false,
        formState.value.choise = (List.value)[idx].choise.indexOf("、") != -1 ? (List.value)[idx].choise.split('、') : (List.value)[idx].choise
      // };
      console.log(formState)
    };

    const choise = ref({
      单行文本: ["无"],
      日期: ["年-月", "年-月-日", "年-月-日 时-分"],
      单选下拉: ["选项一", "选项二", "选项三", "选项四", "选项五", "选项六"],
    });
    const handleChange = (value) => {
      formState.value.choise = "";
      if (value == "单选下拉") {
        formState.value.choise = ["选项一", "选项二"];
      }
    };
    const onSubmit = () => {
      console.log(formState);
      if (!formState.value.name && !formState.value.choise) {
        message.info("请将表单填写完整");
        return;
      }
      (List.value)[index.value] = {
        name: formState.value.name,
        type: formState.value.type,
        require: formState.value.require ? "是" : "否",
        choise: Array.isArray(formState.value.choise)
          ? formState.value.choise.join("、")
          : formState.value.choise,
      };
      visible.value = false;
    };
    const cancel = () => {
      visible.value = false;
      formState.value = {
        name: "",
        type: "单行文本",
        require: false,
        choise: "",
      };
    };

    return {
      index,
      labelCol: { style: { width: "150px" } },
      wrapperCol: { span: 14 },
      List,
      visible,
      showModal,
      formState,
      choise,
      handleChange,
      onSubmit,
      cancel,
    };
  },
  methods: {},
};
</script>

<style lang="less" scoped>
.about {
  .list {
    width: 1000px;
    margin: 0 auto;
    border: 1px solid gray;
    border-left: none;
    .title,
    .item {
      display: flex;
      > div {
        width: 20%;
        text-align: left;
        padding: 10px 5px;
        border-left: 1px solid gray;
      }
    }
    .title {
      background-color: #f8f8f8;
    }
    .item {
      border-top: 1px solid gray;
    }
  }
}
</style>