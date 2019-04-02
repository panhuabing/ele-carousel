<template>
  <div id="app">
    <el-container>
      <el-main>
        <el-row :gutter="20">
          <el-col :span="6">
            <div class="grid-content bg-purple">
              <el-button type="primary" @click="edit">修改</el-button>
              <el-button type="primary" @click="save">保存</el-button>
            </div>
          </el-col>
        </el-row>
        <el-row :gutter="20">
          <el-col :span="3">
            <div class="grid-content bg-purple">
              <el-tag>轮播图片一</el-tag>
            </div>
          </el-col>
          <el-col :span="3">
            <div class="grid-content bg-purple">
              <el-select v-model="value1" :disabled='s1' placeholder="请选择">
                <el-option
                  v-for="item in options"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value">
                </el-option>
              </el-select>
            </div>
          </el-col>
        </el-row>
        <el-row :gutter="20">
          <el-col :span="3">
            <div class="grid-content bg-purple">
              <el-tag>轮播图片二</el-tag>
            </div>
          </el-col>
          <el-col :span="3">
            <div class="grid-content bg-purple">
              <el-select v-model="value2" :disabled='s2' placeholder="请选择">
                <el-option
                  v-for="item in options"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value">
                </el-option>
              </el-select>
            </div>
          </el-col>
        </el-row>
        <el-row :gutter="20">
          <el-col :span="3">
            <div class="grid-content bg-purple">
              <el-tag>轮播图片三</el-tag>
            </div>
          </el-col>
          <el-col :span="3">
            <div class="grid-content bg-purple">
              <el-select v-model="value3" :disabled='s3' placeholder="请选择">
                <el-option
                  v-for="item in options"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value">
                </el-option>
              </el-select>
            </div>
          </el-col>
        </el-row>
        <el-row :gutter="20">
          <el-col :span="3">
            <div class="grid-content bg-purple">
              <el-tag>轮播图片四</el-tag>
            </div>
          </el-col>
          <el-col :span="3">
            <div class="grid-content bg-purple">
              <el-select v-model="value4" :disabled='s4' placeholder="请选择">
                <el-option
                  v-for="item in options"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value">
                </el-option>
              </el-select>
            </div>
          </el-col>
        </el-row>
      </el-main>
      <el-main>
        <el-upload
          class="upload-demo"
          :auto-upload="false"
          action="1233"
          :limit="12"
          ref="upload"
          :file-list="fileList2"
          :http-request="upload"
          list-type="picture"
          multiple
        >
          <el-button size="small" type="primary">点击选择文件</el-button>
          <div slot="tip" class="el-upload__tip">只能上传jpg文件</div>
        </el-upload>

        <el-button :class="{lcolor:pp}" type="primary" @click="upload">上传</el-button>
      </el-main>
    </el-container>
    <el-container>
      <el-main width="400px">
        <el-carousel :interval="4000" type="card" height="200px">
          <el-carousel-item v-for="(img,index) in selData" :key="index">
            <h3><img ref="imgHeight" :src="img.url" alt=""></h3>
          </el-carousel-item>
        </el-carousel>
      </el-main>
    </el-container>

  </div>
</template>

<script>
  export default {
    name: 'App',
    data() {
      return {
        pp: true,
        options: [
          {value: '1', label: "cat1"},
          {value: '2', label: "cat2"},
          {value: '3', label: "cat3"},
          {value: '4', label: "cat4"},
          {value: '5', label: "cat5"},
          {value: '6', label: "cat6"}
        ],
        s1: true,
        s2: true,
        s3: true,
        s4: true,
        value1: '',
        value2: '',
        value3: '',
        value4: '',
        fileList2: [],
        selData: []
      }
    },
    mounted() {
     this.getPath();

    },
    methods: {
      edit() {
        this.s1 = false;
        this.s2 = false;
        this.s3 = false;
        this.s4 = false;
      },
      save() {

        // const headerConfig = { headers: { 'Content-Type': 'multipart/form-data' } };

        this.$http.get('http://localhost:8081/uploads/1553333876502.jpg', {}).then(res => {
          console.log(res);
        })
      },
      // handleRemove(file, fileList) {
      //   console.log(file, fileList);
      // },
      // handlePreview(file) {
      //   console.log(file);
      // },
      upload() {
        const formData = new FormData();
        const file = this.$refs.upload.uploadFiles[0];
        const headerConfig = {headers: {'Content-Type': 'multipart/form-data'}};
        if (!file) { // 若未选择文件
          alert('请选择文件');
          return;
        }
        formData.append('file', file.raw);
        this.$http.post('http://localhost:8081/api/upload', formData, headerConfig).then(res => {
          console.log(res);
        })
      },
      getPath(){
        this.$http.get('./static/path.json', {}).then(res => {
           this.fileList2 = res.data;
           this.selData = res.data;
        })

      }

    }
  }
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }

  .el-carousel__item h3 {
    color: #475669;
    font-size: 14px;
    opacity: 0.75;
    line-height: 200px;
    margin: 0;
  }

  .el-carousel__item:nth-child(2n) {
    background-color: #99a9bf;
  }

  .el-carousel__item:nth-child(2n+1) {
    background-color: #d3dce6;
  }

  .el-row {
    margin-bottom: 20px;
  }

  .el-col {
    border-radius: 4px;
  }


</style>
