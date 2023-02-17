<template>
  <div>
    <nav>
      <v-toolbar app height="90px" elevation="9" class="rounded-lg" color="#099fae">
        <v-btn @click="drawer = !drawer" class="ma-2" fab dark elevation="0" text icon color="#white">
          <v-icon>
            mdi-menu
          </v-icon>
        </v-btn>
        <v-toolbar-title color="#white">
          <span class="font-weight-light text" color="#white">E-learning </span>
          <span class="text">for </span>
          <span class="text"> Hens</span>
        </v-toolbar-title>
        <v-spacer></v-spacer>
      </v-toolbar>
      <v-navigation-drawer app v-model="drawer" color="#099fae">
        <div class="nav-menu">
          <v-list-item class="px-2">
            <v-list-item-avatar>
              <v-img
                src="https://scontent.fkkc3-1.fna.fbcdn.net/v/t1.6435-9/187955402_1805265456321792_5790365418668920358_n.jpg?_nc_cat=106&ccb=1-7&_nc_sid=09cbfe&_nc_eui2=AeFjnZw5Nq60vr2i-vtQx02zt_TST1ymdpi39NJPXKZ2mKc6wPDghlYK-NI2b8K70uFEBMDorIYT4x8wUiD4sod4&_nc_ohc=gc5K5a-etUIAX8jo5gB&_nc_ht=scontent.fkkc3-1.fna&oh=00_AT8CgTmO1G_DIKzQ4qaflnFdc9pY9PfdWifv95tmk0pp3g&oe=637B29A9"></v-img>
            </v-list-item-avatar>

            <v-list-item-title class="text">Friend</v-list-item-title>

          </v-list-item>
          <v-divider></v-divider>

          <v-list dense @click="r()">
            <v-list-item v-for="item in items" :key="item.title" link router :to="item.route" class="icon">
              <v-list-item-icon class="icon">
                <v-icon class="icon">{{ item.icon }}</v-icon>
              </v-list-item-icon>

              <v-list-item-content class="title">
                <v-list-item-title>{{ item.title }}</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list>
        </div>

      </v-navigation-drawer>
    </nav>
  </div>
</template>
<script>
export default {
  data () {
    return {
      drawer: false,
      items: [
        { title: 'Dashboard', icon: 'mdi-view-dashboard', route: '/admindashboard' },
        { title: 'หน้าแรก', icon: 'mdi-home', route: '/adminhome' },
        { title: 'จัดการโพสต์', icon: 'mdi-post', route: '/adminpost' },
        { title: 'จัดการบทเรียน', icon: 'mdi-book-open-variant', route: '/adminlesson' },
        { title: 'จัดการแบบทดสอบ', icon: 'mdi-notebook-edit', route: '/adminquiz' },
        { title: 'คะเเนนนักศึกษา', icon: 'mdi-format-list-numbered', route: '/adminscore' },
        { title: 'จัดการวิดีโอ', icon: 'mdi-video', route: '/adminvideo' },
        { title: 'จัดการเว็บไซต์ที่เกี่ยวข้อง', icon: 'mdi-link', route: 'adminwebsite' },
        { title: 'จัดการผู้ใช้', icon: 'mdi-account-group', route: '/adminuser' },
        { title: 'บัญชีผู้ใช้', icon: 'mdi-account', route: '/adminprofile' },
        { title: 'ออกจากระบบ', icon: 'mdi-logout', route: '/' }
      ]
    }
  }
}
</script>
<style lang="sass" scoped>
.title
 font-size: 2.5em !important
 color: #AB4D4 !important
 cursor: pointer
.font
 font-size: 2em !important
 padding: 10% 5% !important
.icon
  color: #fff !important
.img
 margin:30px auto
 font-size: 1.2em !important
.px-2
 background: #099fae
 color: #AB4D4 !important
.ma-2
  font-size: 0.8em !important
  width:100%
  background: #AB4D4 !important
.text
 color:#fff
</style>
