<template>
  <el-container>
    <el-header>
      <el-form :inline="true" :model="formInline" class="demo-form-inline">
        <el-form-item label="文章标题">
          <el-input v-model="formInline.title" placeholder="请输入文章标题"></el-input>
        </el-form-item>
        <el-form-item label="文章类型">
          <el-select v-model="formInline.type" placeholder="请选择文章类型">
            <el-option label="ES6" value="ES6"></el-option>
            <el-option label="React" value="React"></el-option>
            <el-option label="Vue" value="Vue"></el-option>
            <el-option label="Node" value="Node"></el-option>
            <el-option label="Css" value="Css"></el-option>
          </el-select>
        </el-form-item>
      </el-form>
    </el-header>
    <el-main style="height:auto" class="quill-editor">
      <quill-editor v-model="formInline.content" ref="myQuillEditor" :options="editorOption" style="height:auto">
      </quill-editor>
    </el-main>
    <el-footer>
      <el-form>
        <el-form-item>
          <el-button type="primary" @click="onSubmit">立即创建</el-button>
        </el-form-item>
      </el-form>
    </el-footer>
  </el-container>
</template>
<script>
import fetch from '../../fetch/api';
import { util } from '../../utils/util';
import hljs from 'highlight.js';
export default {
  data () {
    return {
      editorOption: {
        modules: {
          syntax: {
            highlight: text => hljs.highlightAuto(text).value
          },
          toolbar: [
            ['bold', 'italic', 'underline', 'strike'], // toggled buttons
            ['blockquote', 'code-block'],

            [{ 'header': 1 }, { 'header': 2 }], // custom button values
            [{ 'list': 'ordered' }, { 'list': 'bullet' }],
            [{ 'script': 'sub' }, { 'script': 'super' }], // superscript/subscript
            [{ 'indent': '-1' }, { 'indent': '+1' }], // outdent/indent
            [{ 'direction': 'rtl' }], // text direction

            [{ 'size': ['small', false, 'large', 'huge'] }], // custom dropdown
            [{ 'header': [1, 2, 3, 4, 5, 6, false] }],

            [{ 'color': [] }, { 'background': [] }], // dropdown with defaults from theme
            [{ 'font': [] }],
            [{ 'align': [] }],

            ['clean'] // remove formatting button
          ]
        },
        theme: 'snow'
      },
      formInline: {
        title: null,
        type: null,
        content: null,
        date: util.getCurDateWithOutTimeWeek()
      }
    };
  },
  methods: {
    onSubmit () {
      fetch('artical/create', this.formInline).then((data) => {
        if (data.status === 'true') {
          this.$message({
            message: data.message,
            type: 'success'
          });
          this.formInline = {
            title: null,
            type: null,
            content: null,
            date: util.getCurDateWithOutTimeWeek()
          };
          this.$router.push('/articalList');
        } else {
          this.$message.error(data.message);
        }
      });
    }
  }
};
</script>

<style lang="scss" scoped>
.el-header {
  margin-top: 30px;
}
</style>

<style lang="scss">
.quill-editor .ql-container {
  min-height: 270px;
}
</style>
