<template>
  <v-footer color="#099fae" padless>
    <v-row justify="center" no-gutters >
      <v-col
      md="12" cols="12" justify="center" text-center style="margin-top: 50px;">
     <center>
    <v-btn
      elevation="0"
      rounded
      color="#099fae"
      dark
      href="/home"
    >
      หน้าแรก
    </v-btn>
    <v-btn
      rounded
      color="#099fae"
      dark
      elevation="0"
      href="/post"
    >
      กระทู้
    </v-btn>
    <v-btn
      rounded
      color="#099fae"
      dark
      elevation="0"
      href="/home"
    >
      บทเรียน
    </v-btn>
    <v-btn
      rounded
      color="#099fae"
      dark
      elevation="0"
      href="/lesson"
    >
      แบบทดสอบ
    </v-btn>
    <v-btn
      rounded
      color="#099fae"
      dark
      elevation="0"
      href="/quiz"
    >
      วิดีโอ
    </v-btn>
    <v-btn
      rounded
      color="#099fae"
      dark
      elevation="0"
      href="/video"
    >
      ประวัติเข้าเรียน
    </v-btn>
    <v-btn
      rounded
      color="#099fae"
      dark
      elevation="0"
      href="/history"
    >
    ประวัติเข้าเรียน
    </v-btn>
    <v-btn
      rounded
      color="#099fae"
      dark
      elevation="0"
      href="/profile"
    >
      บัญชีผู้ใช้
    </v-btn>
    </center>
      </v-col>
      <v-row style="margin-top:25px" >
      <v-col class="#099fae lighten-2 py-4 text-center white--text" md="12" cols="12" >
       <center>ติดต่อที่
       <p>สาขาสัตวศาสตร์ คณะเกษตรศาสตร์ มหาวิทยาลัยขอนแก่น</p>
       <p> อาจารย์ผู้สอน รศ.ดร. สจี กัณหาเรียง</p>
       <p>อีเมล : </p></center>

      </v-col>
      <v-col class="#099fae lighten-2 py-4 text-center white--text" cols="12" >
        <center><GoogleMap :latitude= 16.476069854285484 :longitude= 102.8218708642138 :title="'Titulo Marcador'" /></center>
      </v-col></v-row>
      <v-col class="#099fae lighten-2 py-4 text-center white--text" cols="12">
        {{ new Date().getFullYear() }} — <strong>Khon Kaen University</strong>
      </v-col>
    </v-row>
  </v-footer>
</template>

<script>
import GoogleMap from './GoogleMap'
export default {
  components: {
    GoogleMap
  },
  data: () => ({
  })
}
</script>

<style></style>
