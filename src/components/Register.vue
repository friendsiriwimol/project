<template>
  <div class="app">
  <v-row >
    <v-col
    lg="5"
      sm="2"
      md="6"
      cols="6"

    >
      <div class="content">
        <img src="../assets/registerlogo.png" alt="img" class="image" />
            </div>
    </v-col>
    <v-col
      cols="12"
      sm="9"
      md="6"
      lg="7"
      class="mt-5"
        >
      <center>
      <v-title class="header" style="padding:20px">E-learning for Hens</v-title>
      <v-form ref="form" v-model="valid" lazy-validation action="register" class="sign-in-form ma-10" style="padding:0%">
        <h3 style="margin-bottom:25px"> <span class="linkLogin"  style="color:#aaa;"  onclick="window.location.href='/'">เข้าสู่ระบบ</span> <span class="border ml-3 mr-3"></span> <span class="linkRegister" style="color:#fcad74; font-size:25px;" onclick="window.location.href='Register'">ลงทะเบียน</span></h3>
      <v-row>
      <v-col
          cols="12"
          sm="4"
        >
       <v-text-field
      v-model="user.user_firstname"
      :rules="firstnameRules"
      :error-messages="nameErrors"
      label="ชื่อ"
      required
      filled
       rounded
       dense
       class="input1"
    ></v-text-field>
        </v-col>
     <v-col
          cols="12"
          sm="4"
        >
    <v-text-field
      v-model="user.user_lastname"
      :rules="lastnameRules"
      :error-messages="lastnameErrors"
      label="นามสกุล"
      required
      filled
       rounded
       dense
       class="input1"
    ></v-text-field>
     </v-col>
      <v-col
          cols="12"
          sm="4"
        >
    <v-text-field
      v-model="user.user_email"
      :rules="emailRules"
      :error-messages="emailErrors"
      label="อีเมล"
      filled
       rounded
       dense
       class="input1"
      required
    ></v-text-field>
      </v-col>
       <v-col
          cols="12"
          sm="4"
        >
    <v-text-field
            v-model="user.user_password"
            :append-icon="show ? 'mdi-eye' : 'mdi-eye-off'"
            :rules="[passwordRules.required, passwordRules.min]"
            :type="show ? 'text' : 'password'"
            name="input-10-2"
            label="รหัสผ่าน"
            @click:append="show = !show"
        required
        filled
       rounded
       dense
       class="input1 input-group--focused"
      ></v-text-field>
       </v-col>
       <v-col
          cols="12"
          sm="4"
        >
        <v-text-field
        v-model="user.user_confirmPassword"
        :rules="confirmPasswordRules.concat(passwordConfirmationRule)"
        :append-icon="showConfirm ? 'mdi-eye' : 'mdi-eye-off'"
        :type="showConfirm ? 'text' : 'password'"
        label="ยืนยันรหัสผ่าน"
        required
        filled
       rounded
       dense
       @click:append="showConfirm = !showConfirm"
        ></v-text-field>
        </v-col>
        <!-- <v-col
          cols="12"
          sm="4"
        >
      <v-text-field
        type="text"
        v-model="user.user_tel"
        :rules="telRules"
        label="เบอร์โทร"
        filled
       rounded
       dense
       class="input1"
        required
      ></v-text-field>
        </v-col> -->
     <v-col
          cols="12"
          sm="4"
        >
      <!-- <v-text-field
        type="number"
        v-model="user.user_age"
        :rules="ageRules"
        label="อายุ"
        required
        min="1"
        max="100"
        filled
       rounded
       dense
       class="input1"
      ></v-text-field> -->
      <v-menu
        v-model="menu"
        :close-on-content-click="false"
        :nudge-right="40"
        transition="scale-transition"
        offset-y
        min-width="290px"
      >
        <template v-slot:activator="{ on, attrs }">
          <v-text-field
            v-model="user.user_birthday"
            label="วันเดือนปีเกิด"
            :rules="birthdayRules"
            prepend-inner-icon="mdi-calendar-range"
            required
        filled
       rounded
       dense
            v-bind="attrs"
            v-on="on"
          ></v-text-field>
        </template>
        <v-date-picker v-model="user.user_birthday" locale="th" @input="menu = false"></v-date-picker>
      </v-menu>
     </v-col>
     <v-col
          cols="12"
          sm="4"
        >
    <v-select
          v-model="user.user_type"
        :items="['นักศึกษา', 'เกษตรกร']"
        :rules="rules.select"
        label="กลุ่มผู้ใช้งาน"
      :error-messages="selectErrors"
      required
      filled
       rounded
       dense
       class="input1"
      @change="$v.select.$touch()"
      @blur="$v.select.$touch()"
    >
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
     </v-col>
     <v-col
          cols="12"
          sm="4"
        >
       <v-text-field
      v-model="user.user_district"
      :rules="districtRules"
      :error-messages="districtErrors"
      label="อำเภอ"
      required
      filled
       rounded
       dense
       class="input1"
    ></v-text-field>
        </v-col>
     <v-col
          cols="12"
          sm="4"
        >
    <v-text-field
      v-model="user.user_province"
      :rules="provinceRules"
      :error-messages="provinceErrors"
      label="จังหวัด"
      required
      filled
       rounded
       dense
       class="input1"
    ></v-text-field>
     </v-col>
    <!-- <v-col
          cols="12"
          sm="4"
          class="ma-0"
        >
        <addressinput-district class="addressinput" v-model="user.user_district" placeholder="อำเภอ"/>

        </v-col>
        <v-col
          cols="12"
          sm="4"
        >
        <addressinput-province class="addressinput" v-model="user.user_province" placeholder="จังหวัด"/>
        </v-col> -->
      </v-row>
    <v-row>
        <input type="button" class="button button1 btn mr-5" value="ลงทะเบียน" @click="resgisterUser()"/>
      </v-row>
          </v-form>
        </center>
          </v-col>
          </v-row>
  <!-- Youtube Link -->
</div>
</template>

<script>
import axios from 'axios'
import Swal from 'sweetalert2'

export default {
  data: () => {
    return {
      menu: false,
      subdistrict: '',
      district: '',
      province: '',
      zipcode: '',
      signUp: true,
      user: {
        user_firstname: '',
        user_lastname: '',
        user_email: '',
        user_password: '',
        user_confirmPassword: '',
        user_type: '',
        user_age: '',
        user_district: '',
        user_province: '',
        user_birthday: ''
      },
      valid: true,
      show: false,
      showConfirm: false,
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
      // passwordRules: [v => !!v || 'Password is required'],
      confirmPasswordRules: [v => !!v || 'กรุณากรอกยืนยันรหัสผ่าน'],
      passwordRules: {
        required: value => !!value || 'กรุณากรอกรหัสผ่าน',
        min: v => v.length >= 8 || 'กรุณากรอกรหัสผ่านอย่างน้อย 8 ตัวอักษร',
        emailMatch: () => ('The email and password you entered don\'t match')
      },
      telRules: [
        v => !!v || 'กรุณากรอกเบอร์โทร'
      ],
      ageRules: [
        v => !!v || 'กรุณากรอกอายุ'
      ],
      birthdayRules: [
        v => !!v || 'กรุณากรอกวันเดือนปีเกิด'
      ],
      rules: {
        select: [v => !!v || 'กรุณากรอกกลุ่มผู้ใช้งาน']
      },
      districtRules: [
        v => !!v || 'กรุณากรอกอำเภอ'
      ],
      provinceRules: [
        v => !!v || 'กรุณากรอกจังหวัด'
      ]
    }
  },
  computed: {
    passwordConfirmationRule () {
      return () =>
        this.user.user_password === this.user.user_confirmPassword || 'กรุณากรอกรหัสผ่านให้ตรงกัน'
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
            user_type: this.user.user_type,
            user_birthday: this.user.user_birthday,
            user_district: this.user.user_district,
            user_province: this.user.user_province
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

<style lang="scss" scoped>
body {
  margin: 0;
  padding: 0;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
}

#app{
    padding: 0px;
    margin: 0px;
}

@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@200;300;400;500;600;700;800&display=swap");

* {
  margin: 0;
  // padding: 0;
  box-sizing: border-box;
    font-family: 'Prompt', sans-serif !important;

}
body,
input {
//   font-family: "Poppins", sans-serif;
}
.reg{
    text-decoration: underline;
}
.input1 {
  width: 40vw;
  color: #04befe;
}
.container {
  position: relative;
  width: 100vw;
//   background-color: #fff;
  //min-height: 100vh;
  // overflow: hidden;
}
// .all{
//   margin:auto;
// }
article{
    margin-top: -5vw;
}
// .forms-container {
//   position: absolute;
//   width: 100vw;
//   height: 100%;
//   top: 0;
//   left: 0;
// }

.signin-signup {
  position: absolute;
  top: 10%;
  transform: translate(-50%, -50%);
  // left: 85%;
  width: 70%;
  transition: 0.5s 0.5s ease-in-out;
  // display: grid;
  grid-template-columns: 1fr;
  z-index: 5;
}

form {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  padding: 5rem 5rem;
  transition: all 0.2s 0.2s;
  overflow: hidden;
  grid-column: 1 / 2;
  grid-row: 1 / 2;
}

form.sign-up-form {
  opacity: 0;
  z-index: 1;
}

form.sign-in-form {
  z-index: 2;
}

.title {
  font-size: 4.5rem;
  color: #444;
  margin-bottom: 20px;
//   font-family: "Poppins", sans-serif;
}

.input-field {
  max-width: 380px;
  width: 100%;
  background-color: #f0f0f0;
  margin: 10px 0;
  height: 55px;
  border-radius: 55px;
  display: grid;
  grid-template-columns: 15% 85%;
  padding: 0 0.4rem;
  position: relative;
}

.input-field i {
  text-align: center;
  line-height: 55px;
  color: #acacac;
  transition: 0.5s;
  font-size: 1.1rem;
}

.input-field input {
  background: none;
  outline: none;
  border: none;
  line-height: 1;
  font-weight: 600;
  font-size: 1.1rem;
  color: #333;
}

.input-field input::placeholder {
  color: #aaa;
  font-weight: 500;
}

.social-text {
  padding: 0.7rem 0;
  font-size: 1rem;
}

.social-media {
  display: flex;
  justify-content: center;
}

.social-icon {
  height: 46px;
  width: 46px;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 0 0.45rem;
  color: #333;
  border-radius: 50%;
  border: 1px solid #333;
  text-decoration: none;
  font-size: 1.1rem;
  transition: 0.3s;
}

.social-icon:hover {
  color: #4481eb;
  border-color: #4481eb;
}

.btn {
  width: 150px;
  background-color: #5995fd;
  border: none;
  outline: none;
  height: 49px;
  border-radius: 49px;
  color: #fff;
  text-transform: uppercase;
  font-weight: 600;
  margin: 10px 0;
  cursor: pointer;
  transition: 0.5s;
}

.btn:hover {
  background-color: #2a68d5;
}
.btn1 {
  width: 150px;
  background-color: #fcad74;
  border: none;
  outline: none;
  height: 49px;
  border-radius: 49px;
  color: #fff;
  text-transform: uppercase;
  font-weight: 600;
  margin: 10px 0;
  cursor: pointer;
  transition: 0.5s;
}

.btn1:hover {
  background-color: #D1551B;
}
.linkRegister:hover{
  color: #D1551B !important;
}

.linkLogin:hover{
  font-size: 25px;
  color: gray !important;
}

.image {
  width: 100%;
}

.btn.transparent {
  margin: 0;
  background: none;
  border: 2px solid #fff;
  width: 130px;
  height: 41px;
  font-weight: 600;
  font-size: 0.8rem;

}

/* ANIMATION */

.container.sign-up-mode:before {
  transform: translate(100%, -50%);
  right: 52%;
}

.container.sign-up-mode .left-panel .image,
.container.sign-up-mode .left-panel .content {
  transform: translateX(-800px);
}

.container.sign-up-mode .signin-signup {
  left: 25%;
}

.container.sign-up-mode form.sign-up-form {
  opacity: 1;
  z-index: 2;
}

.container.sign-up-mode form.sign-in-form {
  opacity: 0;
  z-index: 1;
}

.container.sign-up-mode .right-panel .image,
.container.sign-up-mode .right-panel .content {
  transform: translateX(0%);
}

@import url(https://fonts.googleapis.com/css?family=Righteous);
.header{
  // margin-bottom: 30px;
  font-size: 3em;
  font-weight: bold;
  font-family: 'Righteous', serif;
  background: linear-gradient(110deg, #fcad74 33%, rgba(253, 255, 144, 0) 33%), linear-gradient(110deg, #ffffff 34%, #4481eb 34%);
    background-size: 400%;
     -webkit-text-fill-color: transparent;
  background-size: 40%;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;

  animation: gradient 5s infinite;
}

@keyframes gradient {
  0% {
    background-position: 0% 50%;
  }
  100% {
    background-position: 100% 50%;
  }
}
.input1{
  margin: 0%;
}
.border{
  border-left: 2px solid #aaa;

}
</style>
