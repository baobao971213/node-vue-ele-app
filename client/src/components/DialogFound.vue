<template>
  <div class="logFund">
    <el-dialog
      :title="dialog.title"
      :visible.sync="dialog.show"
      :close-on-click-modal="false"
      :close-on-press-escape="false"
      :modal-append-to-body="false"
    >
      <div class="form">
        <el-form
          ref="form"
          :model="form"
          :rules="form_rules"
          label-width="120px"
          style="margin: 10px; width: auto"
        >
          <el-form-item label="药物类型:">
            <el-select v-model="form.type" placeholder="药物类型">
              <!-- 遍历 -->
              <el-option
                v-for="(formtype, index) in format_type_list"
                :key="index"
                :label="formtype"
                :value="formtype"
              ></el-option>
            </el-select>
          </el-form-item>

          <el-form-item prop="describe" label="药物名称:">
            <el-input type="describe" v-model="form.describe"></el-input>
          </el-form-item>

          <el-form-item prop="income" label="当天进货:">
            <el-input type="income" v-model="form.income"></el-input>
          </el-form-item>

          <el-form-item prop="expend" label="出售数量:">
            <el-input type="expend" v-model="form.expend"></el-input>
          </el-form-item>

          <el-form-item prop="cash" label="剩余库存:">
            <el-input type="cash" v-model="form.cash"></el-input>
          </el-form-item>

          <el-form-item label="备注:">
            <el-input type="textarea" v-model="form.remark"></el-input>
          </el-form-item>

          <el-form-item class="text_right">
            <el-button @click="dialog.show = false">取 消</el-button>
            <el-button type="primary" @click="onSubmit('form')"
              >提 交</el-button
            >
          </el-form-item>
        </el-form>
      </div>
      <div class="form">
        <el-form
          ref="form"
          :model="form"
          :rules="form_rules"
          label-width="120px"
          style="margin: 10px; width: auto"
        >
        </el-form>
      </div>
    </el-dialog>
  </div>
</template>

<script>
export default {
  name: "logfound",
  props: {
    dialog: Object,
    form: Object,
  },
  data() {
    return {
      format_type_list: [
        //数组
        "处方药",
        "非处方药",
        "中药",
        "西药",
        "医疗器材",
      ],
      form_rules: {
        describe: [
          { required: true, message: "药物描述不能为空！", trigger: "blur" },
        ],
        income: [
          { required: true, message: "当天进货不能为空！", trigger: "blur" },
        ],
        expend: [
          { required: true, message: "出售数量不能为空！", trigger: "blur" },
        ],
        cash: [
          { required: true, message: "剩余库存不能为空！", trigger: "blur" },
        ],
      },
    };
  },
  methods: {
    onSubmit(form) {
      this.$refs[form].validate((valid) => {
        if (valid) {
          //表单数据验证完成之后，提交数据;
          const url =
            this.dialog.option == "add" ? "add" : `edit/${this.form.id}`;
          this.$axios.post(`/api/profile/${url}`, this.form).then((res) => {
            //将数据传送到Profile的add
            // 操作成功
            this.$message({
              message: "保存成功！",
              type: "success",
            });
            this.dialog.show = false;
            this.$emit("update"); //自动刷新页面 在Foundlist里面执行
          });
        }
      });
    },
  },
};
</script>

