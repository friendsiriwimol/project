<template>
<div>
  <v-card class="mx-auto" max-width="70%" max-height="80%" outlined>
    <v-list-item three-line>
      <v-list-item-content>
        <div class="text-overline mb-4">{{ password }}</div>
        <center><h1>สมัครสมาชิก</h1></center>
      </v-list-item-content>
    </v-list-item>

  <v-form v-model="valid" ref="form">
    <v-text-field
      v-model="user.user_firstname"
      :rules="firstnameRules"
      label="ชื่อ"
      required
    ></v-text-field>
    <v-text-field
      v-model="user.user_lastname"
      :rules="lastnameRules"
      label="นามสกุล"
      required
    ></v-text-field>
    <v-text-field
    type="email"
      v-model="user.user_email"
      :rules="emailRules"
      label="อีเมล"
      required
    ></v-text-field>
    <v-text-field
            v-model="user.user_password"
            :append-icon="show ? 'mdi-eye' : 'mdi-eye-off'"
            :rules="[passwordRules.required, passwordRules.min]"
            :type="show ? 'text' : 'password'"
            name="input-10-2"
            label="รหัสผ่าน"
            class="input-group--focused"
            @click:append="show = !show"
          ></v-text-field>
      <v-text-field
        type="text"
        v-model="user.user_tel"
        :rules="telRules"
        label="เบอร์โทร"
        required
      ></v-text-field>
      <v-select
      v-model="user.user_type"
        :items="['นักศึกษา', 'เกษตรกร']"
        :rules="typeRules"
        label="กลุ่มผู้ใช้งาน">
        <template v-slot:item="{ item, attrs, on }">
        <v-list-item
            v-bind="attrs"
            v-on="on"
        >
            <v-list-item-title
            :id="attrs['aria-labelledby']"
            v-text="item"
            ></v-list-item-title>
        </v-list-item>
        </template>
    </v-select>
      <v-text-field
        type="number"
        v-model="user.user_age"
        :rules="ageRules"
        label="อายุ"
        required
        min="1"
        max="100"
      ></v-text-field>

    <div class="btn">
         <input type="button" class="button button1" value="ลงทะเบียน" @click="resgisterUser()"/>
          </div>
  </v-form>
    <v-card-actions>
    </v-card-actions>
  </v-card>
  </div>
</template>

<script>
import axios from 'axios'
import Swal from 'sweetalert2'

export default {
  data () {
    return {
      user: {
        user_firstname: '',
        user_lastname: '',
        user_email: '',
        user_password: '',
        user_tel: '',
        user_type: '',
        user_age: ''
      },
      valid: true,
      show: false,
      firstnameRules: [
        v => !!v || 'กรุณากรอกชื่อ'
      ],
      lastnameRules: [
        v => !!v || 'กรุณากรอกนามสกุล'
      ],
      emailRules: [
        v => !!v || 'กรุณากรอกอีเมล',
        v => /.+@.+\..+/.test(v) || 'กรุณาใส่กรอกอีเมลให้ถูกต้อง'
      ],
      passwordRules: {
        required: value => !!value || 'กรุณากรอกรหัสผ่าน',
        min: v => v.length >= 8 || 'กรุณากรอกรหัสผ่านอย่างน้อย 8 ตัวอักษร',
        emailMatch: () => ('The email and password you entered don\'t match')
      },
      telRules: [
        v => !!v || 'กรุณากรอกเบอร์โทร'
      ],
      typeRules: [
        v => !!v || 'กรุณากรอกกลุ่มผู้ใช้งาน'
      ],
      ageRules: [
        v => !!v || 'กรุณากรอกอายุ'
      ]
    }
  },
  methods: {
    resgisterUser () {
    //   console.log(this.user)
      console.log(this.$refs.form.validate(), 'pp')
      if (this.$refs.form.validate()) {
        console.log('mmmm')
        axios
          .post('http://localhost/vue-backend/register.php', {
            user_firstname: this.user.user_firstname,
            user_lastname: this.user.user_lastname,
            user_email: this.user.user_email,
            user_password: this.user.user_password,
            user_tel: this.user.user_tel,
            user_type: this.user.user_type,
            user_age: this.user.user_age
          })
          .then(function (response) {
            console.log(response, 'ii')
            Swal.fire({
              icon: 'success',
              title: 'ลงทะเบียนสำเร็จ',
              showConfirmButton: false,
              // text: 'คำอธิบาย',
              customClass: {
                title: 'csss'
              },
              timer: 1500
            })
          })
        this.$router.push('/')
          .catch(function (error) {
            console.log(error)
          })
      } else {
        Swal.fire({
          icon: 'warning',
          title: 'กรุณากรอกข้อมูลให้ครบ',
          showConfirmButton: false,
          // text: 'คำอธิบาย',
          customClass: {
            title: 'csss'
          },
          timer: 1500
        })
      }
    }
  }
}
</script>

<style scoped>
*{
font-family: 'Prompt', sans-serif;
}
.mx-auto{
   margin-top: -5vw;
   padding: 30px ;
   margin-left: auto;
   margin-right: auto;
}
.text-h5{
  text-align: center;
}
.text-center{
    margin-top: 50px;
    color: #EF5B9C;
}
.v-card {
  transition: opacity .5s ease-in-out rgb(255, 255, 107) ;
   border: 1px solid #FAD335 !important;
    border-radius: 30px !important;
    box-shadow: 1px 1px 9px 9px #FAD335;
}
.v-card:not(.on-hover) {
  box-shadow: 1px 1px 30px 8px #FAD335;
 }
 .button {
  background-color: #4CAF50; /* Green */
  border: none;
  color: white;
  padding: 10px 20px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 40px;
  transition-duration: 0.4s;
  cursor: pointer;
  border-radius: 30px;
  width: 180px;
  margin: 20px auto;
  margin-right: auto;
  margin-left: auto;
  /* display: block; */
}
.button1 {
  background-color: white;
  color: black !important;
  border: 2px solid #FAD335;
  margin: 20px;
}
.button1:hover {
  background-color: #FAD335;
  color: white;
}
.button3 {
  background-color: white;
  color: black;
  border: 2px solid #EF5B9B;
  border-radius: 30px;
  margin: 20px;
}
.button3:hover {
  background-color: #EF5B9B;
  color: white;
}
.btn{
  text-align: center;
   margin: 20px;
   padding: 20px;
}
</style>
