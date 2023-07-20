<template>
  <div id="app">
    <el-form ref="ruleFormRef" :model="ruleForm" :rules="rules" class="ruleForm">
      <el-form-item label-width="200px" prop="name" label ="First Name:">
        <el-input v-model="ruleForm.name" style="width: 200px;" />
      </el-form-item>
      <el-form-item label-width="200px" prop="middle" label="Middle Name:">
        <el-input v-model="ruleForm.middle" style="width: 200px;" />
      </el-form-item>
      <el-form-item label-width="200px" prop="last" label="Last Name:">
        <el-input v-model="ruleForm.last" style="width: 200px;" />
      </el-form-item>
      <el-form-item label="Password:" prop="pass" class="asterisk-after-model" label-width="200px">
        <el-input v-model="input" type="password" show-password style="width: 200px;" />
      </el-form-item>
      <el-form-item label="User Role(s):" prop="role" label-width="200px">
        <el-select v-model="ruleForm.role" placeholder="User Role(s)" style="width: 200px;" multiple>
          <el-option label="Role one" value="role1" />
          <el-option label="Role two" value="role2" />
          <el-option label="Role three" value="role3" />
          <el-option label="Role four" value="role4" />
        </el-select>
      </el-form-item>
      <el-form-item label="Unit Name:" prop="unit" label-width="200px">
        <el-input v-model="ruleForm.unit" style="width: 200px;" />
      </el-form-item>
      <el-form-item label="Date Of Birth:" label-width="200px">
        <el-col :span="11">
          <el-form-item prop="date1">
            <el-date-picker v-model="ruleForm.date1" type="date" label="Pick a date" placeholder="Pick a date"
              style="width: 40%;" />
          </el-form-item>
        </el-col>
      </el-form-item>
      <el-form-item label="Gender:" prop="gender" label-width="200px">
        <el-radio-group v-model="ruleForm.gender">
          <el-radio label="Male" />
          <el-radio label="Female" />
        </el-radio-group>
      </el-form-item>
      <el-form-item label="Rank:" prop="rank" label-width="200px">
        <el-select-v2 v-model="ruleForm.rank" placeholder="Rank" :options="options" style="width: 200px;" />
      </el-form-item>
      <el-form-item label="Professional Phone Number:" prop="pro" label-width="200px">
        <el-input v-model="ruleForm.pro" style="width: 200px;"></el-input>
      </el-form-item>
      <el-form-item label="Personal Phone Number:" prop="per" label-width="200px">
        <el-input v-model="ruleForm.per" style="width: 200px;" />
      </el-form-item>
      <el-form-item label="Professional Email:" prop="email" label-width="200px">
        <el-input v-model="ruleForm.email" style="width: 200px;" />
      </el-form-item>
      <el-form-item label="Personal Email:" prop="pemail" label-width="200px">
        <el-input v-model="ruleForm.pemail" style="width: 200px;" />
      </el-form-item>
      <el-form-item label="Profile Picture:" prop="profile" label-width="200px">
        <div class="butt">
        <el-upload class="avatar-uploader" action="https://run.mocky.io/v3/9d059bf9-4660-45f2-925d-ce80ad6c4d15"
          :show-file-list="false" :on-success="handleAvatarSuccess" :before-upload="beforeAvatarUpload">
          <img v-if="imageUrl" :src="imageUrl" class="avatar" />
          <el-icon v-else class="avatar-uploader-icon"></el-icon>
          <el-button plain>Upload</el-button>
        </el-upload>
      </div>
        <el-button plain v-if="imageUrl" @click="deleteAvatar">Delete</el-button>
      </el-form-item>
      <div class="buttons">
        <el-button plain @click="createForm(ruleFormRef)">Create</el-button>
        <el-button @click="cancelForm(ruleFormRef)">Cancel</el-button>
      </div>

    </el-form>
  </div>
</template>


<script lang="ts" setup>
import { reactive, ref } from 'vue'
import { ElMessage, FormInstance,  UploadProps } from 'element-plus'
const ruleFormRef = ref()
const ruleForm = reactive ({
  name: '',
  middle: '',
  last: '',
  role: '',
  unit: '',
  date1: '',
  gender: '',
  rank: '',
  per: '',
  pro: '',
  email: '',
  pemail: '',
})
console.log("valid")
const input = ref('')
const rules = reactive({
  name: [
    { required: true, message: 'Please input First name', trigger: 'blur' },
  ],
  last: [
    { required: true, message: 'Please input Last name', trigger: 'blur' },
  ],
  pass: [
    { required: true, message: 'Please input a password' },
  ],
  role: [
    { required: true, message: 'Please select one of them', trigger: 'blur' },
  ],
  gender: [
    { required: true, message: 'Please select a gender'  ,trigger: 'blur'},
  ],
})


const createForm = async (formEl: FormInstance) => {
  if (!formEl) return
  await formEl.validate((valid, fields) => {
    if (valid) {
      console.log('submit!')
    } else {
      console.log('error submit!', fields)
    }
  })
}

const imageUrl = ref('')

const handleAvatarSuccess: UploadProps['onSuccess'] = (
  response,
  uploadFile
) => {
  imageUrl.value = URL.createObjectURL(uploadFile.raw!)
}

const beforeAvatarUpload: UploadProps['beforeUpload'] = (rawFile) => {
  if (rawFile.type !== 'image/jpeg') {
    ElMessage.error('Avatar picture must be JPG format!')
    return false
  } else if (rawFile.size / 1024 / 1024 > 2) {
    ElMessage.error('Avatar picture size can not exceed 2MB!')
    return false
  }
  return true
}
const deleteAvatar = () => {
  imageUrl.value = '';
};
const cancelForm = (formEl: FormInstance ) => {
  if (!formEl) return
  formEl.resetFields()
}

const options = Array.from({ length: 10000 }).map((_, idx) => ({
  value: `${idx + 1}`,
  label: `${idx + 1}`,
}))
</script>
<style scoped>
/* /* .butt {
  display: flex;
  align-items: center;
  justify-content: end;
} */
.asterisk-after-label ::after {
  content: "*";
  color: red; 
 
}
.buttons {
  display: flex;
  justify-content: end;
}
</style>