<template>
  <el-card
    header="ele-form-data-editor 演示"
    shadow="never"
    style="max-width: 1250px;min-height: 1000px;margin: 20px auto;"
  >
    <ele-form
      :form-data="formData"
      :form-desc="formDesc"
      :span="22"
      :request-fn="handleRequest"
      @request-success="handleSuccess"
    ></ele-form>
  </el-card>
</template>

<script>
export default {
  data() {
    return {
      formData: {
        comic: [
          {
            name: "火影忍者",
            total: 700
          }
        ]
      },
      formDesc: {
        comic: {
          type: "data-editor",
          label: "动漫",
          attrs: {
            types: ["array"],
            row: 8
          }
        }
      }
    };
  },
  methods: {
    handleRequest(data) {
      console.log(data);
      return Promise.resolve();
    },
    handleSuccess() {
      this.$message.success("提交成功");
    }
  }
};
</script>

<style>
body {
  background-color: #f0f2f5;
}
</style>
