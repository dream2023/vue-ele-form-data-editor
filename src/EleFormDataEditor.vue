<template>
  <div
    class="vue-ele-form-data-editor"
    :class="{ 'is-error': isError, 'is-success': isSuccess }"
  >
    <el-input
      v-model="newValue"
      :class="desc.class"
      type="textarea"
      @blur="attrs.autoSave ? handleChange() : null"
      @change="isError = false"
      :style="desc.style"
      v-bind="attrs"
      v-on="desc.on"
    >
    </el-input>
    <el-button
      size="medium"
      v-if="!attrs.autoSave"
      type="primary"
      style="margin-top:5px;margin-top:10px"
      >保存改动</el-button
    >
    <div class="err-msg" v-if="isError">{{ errMsg }}</div>
  </div>
</template>
<script>
import formMixin from "vue-ele-form/lib/mixins/formMixin";
import serialize from "serialize-javascript";
export default {
  inheritAttrs: false,
  name: "vue-ele-data-editor",
  mixins: [formMixin],
  props: ["field", "value", "desc"],
  data() {
    return {
      isSuccess: false,
      isError: false,
      errMsg: "",
      newValue: "",
      defaultAttrs: {
        rows: 6,
        autoSave: true
      }
    };
  },
  watch: {
    value: {
      handler(val) {
        this.oldVal = val;

        if (val !== undefined && val !== null) {
          val = serialize(val, { space: 2 });
          this.newValue = val;
        }
      },
      immediate: true
    }
  },
  computed: {
    types() {
      return this.toArray(this.attrs.types);
    }
  },
  methods: {
    validate(isSaveChange = false) {
      return new Promise((resolve, reject) => {
        try {
          if (!isSaveChange) {
            if (this.newValue !== this.oldVal) {
              throw new Error("未保存更改");
            }
          }
          if (!this.newValue) return resolve();
          const value = eval("(" + this.newValue + ")");
          const valType = this.getType(value);
          const types = this.attrs.types;
          if (this.types.length && !this.types.includes(valType)) {
            throw new TypeError(
              `类型错误，期望类型为: ${types}, 实际类型为 ${valType}`
            );
          }
          resolve(value);
        } catch (err) {
          console.error(err);
          this.isError = true;
          const msg =
            err instanceof SyntaxError || err instanceof ReferenceError
              ? `数据解析失败${
                  this.types.includes("string")
                    ? '，如果是字符串，请用"引号"'
                    : ""
                }`
              : err.message;
          reject({
            [this.field]: msg
          });
        }
      });
    },
    // 获取类型
    getType(val) {
      return Object.prototype.toString
        .call(val)
        .toLocaleLowerCase()
        .slice(1, -1)
        .split(" ")[1];
    },
    toArray(val) {
      if (val === undefined || val === null) return [];
      return Array.isArray(val) ? val : [val];
    },
    async handleChange() {
      this.isError = false;
      try {
        const value = await this.validate(true);
        this.$emit("input", value);
        this.oldVal = this.newValue;

        this.isSuccess = true;
        setTimeout(() => {
          this.isSuccess = false;
        }, 1000);
      } catch (err) {
        this.errMsg = err[this.field];
      }
    }
  }
};
</script>

<style>
.vue-ele-form-data-editor.is-error .el-textarea__inner {
  border-color: #f56c6c;
}
.vue-ele-form-data-editor.is-error .err-msg {
  color: #f56c6c;
}

.vue-ele-form-data-editor.is-success .el-textarea__inner {
  border-color: #67c23a;
  border-style: dashed;
}
</style>
