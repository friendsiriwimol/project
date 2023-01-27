<template>
  <div>

    <v-container fluid pa-0>

            <!-- <v-row align="center" justify="center"
                style="height:70vh" dense>
                <v-col cols="12" lg="6" md="6" class="grey lighten-2 fill-height d-flex flex-column justify-center align-center">
                    <v-card flat tile >
                        <div class="ma-auto" style="max-width: 260px">
                        <v-otp-input
      v-model="otp"
      type="password"
      length="5"
      dark
    ></v-otp-input>
</div>
                    </v-card>
                </v-col>
            </v-row> -->
<div>
                  <v-breadcrumbs
        :items="breadcrumbs"
        large
      ></v-breadcrumbs>
    </div>
            <v-card elevation="0" style="border-color:#099fae; border-width: 2px;" rounded outlined height="70vh"  class="d-flex align-center justify-center">
            <v-row  align="center" justify="center">
              <v-col cols="12" class="ma-auto " style="max-width: 240px">
                <v-card-title>กรุณากรอกเลข OTP</v-card-title>
              </v-col>
              <v-col cols="12" class="ma-auto pa-0">
                        <div class="ma-auto " style="max-width: 240px">
                        <v-otp-input
                          v-model="otp"
                          type="number"
                          length="5"
                          dark
                          background-color="#b3e2e5"
                        ></v-otp-input>
                    </div>
              </v-col>
              <v-col cols="12" class="d-flex align-center justify-center pa-0">
                <v-card-subtitle><a href="#" class="text-decoration-none grey--text"> ส่งรหัสใหม่อีกครั้ง</a></v-card-subtitle>
              </v-col>
              <v-col cols="12" class="ma-auto pa-0" style="max-width: 240px">
                <v-btn block :disabled="!isActive" class="white--text" color="#099fae">ยืนยัน OTP</v-btn>
              </v-col>
            </v-row>
          </v-card>
        </v-container>
  </div>
</template>

<script>
export default {
  data: () => ({
    otp: '',
    length: 5,
    breadcrumbs: [
      {
        text: 'หน้าเข้าสู่ระบบ',
        disabled: false,
        href: '/'
      },
      {
        text: 'หน้ายืนยันรหัส OTP',
        disabled: true,
        href: 'history'
      }
    ]
  }),
  computed: {
    isActive () {
      return this.otp.length === this.length
    }
  }
}
</script>

<style scoped>
.v-breadcrumbs >>> a {
    color: #fcad74;
}
</style>
