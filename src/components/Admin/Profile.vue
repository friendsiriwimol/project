<template>
<div>
  <NavbarAdmin />
    <div>
<v-breadcrumbs
  :items="breadcrumbs"
  large
></v-breadcrumbs>
</div>
<v-card class="cardShowuser mt-0">
    <v-card-title>
        <!-- <v-tabs-items v-model="tab">
      <v-tab-item
        v-for="item in items"
        :key="item"
      >
      </v-tab-item>
    </v-tabs-items> -->
        <v-icon class="mr-2" color="#fcad74">mdi-account</v-icon>
        บัญชีผู้ใช้
        <v-spacer></v-spacer>
        <v-row justify="end" class="mr-2">
    <v-dialog
      v-model="dialog"
      persistent
      max-width="600px"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-btn
              icon
              color="green"
              v-bind="attrs"
          v-on="on"

            >
              <v-icon>mdi-pencil</v-icon>
            </v-btn>
      </template>
      <v-card>
        <v-card-title>
          แก้ไขบัญชีผู้ใช้
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-row>
              <v-col cols="12">
                <v-text-field
                  label="ชื่อ"
                  v-model="user.user_firstname"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-text-field
                  label="นามสกุล"
                  v-model="user.user_lastname"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-text-field
                  label="อีเมล"
                  v-model="user.user_email"
                  type="email"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-text-field
                  label="เบอร์โทร"
                  v-model="user.user_tel"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-text-field
                  label="อายุ"
                  v-model="user.user_age"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-text-field
                  label="อำเภอ"
                  v-model="user.user_district"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-text-field
                  label="จังหวัด"
                  v-model="user.user_province"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-text-field
                  label="จังหวัด"
                  v-model="user.user_type"
                  disabled
                  required
                ></v-text-field>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="blue darken-1"
            text
            @click="dialog = false"
          >
            ปิด
          </v-btn>
          <v-btn
            color="blue darken-1"
            text
            @click="dialog = false"
          >
            บันทึก
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
        <!-- <button @click="exportUser()" value="Export">โหลด</button> -->
        <!-- <form >
     <input type="submit" name="export" class="btn btn-success"  />
    </form> -->
      </v-card-title>

    <v-divider></v-divider>
    <v-row justify="center" class="ma-5">
    <v-avatar color="primary">
      <span class="white--text text-h5">SP</span>
    </v-avatar>
    </v-row>
        <v-card-text>

    <div class="ma-10">
    <v-row >ชื่อ : {{user.user_firstname}}</v-row>
    <v-row >นามสกุล : {{user.user_latname}}</v-row>
    <v-row >อีเมล : {{user.user_email}}</v-row>
    <v-row >เบอร์โทร : {{user.user_tel}}</v-row>
    <v-row >อายุ : {{user.user_age}}</v-row>
        <v-row >อำเภอ : {{user.user_district}}</v-row>
        <v-row >จังหวัด : {{user.user_province}}</v-row>
        <v-row >กลุ่มผู้ใช้งาน : {{user.user_type}}</v-row>
        </div>
        </v-card-text>
    <!-- <v-virtual-scroll
      :items="items"
      :item-height="50"
      height="300"
    >
      <template v-slot:default="{ item }">
        <v-list-item>
          <v-list-item-avatar>
            <v-avatar
              :color="item.color"
              size="56"
              class="white--text"
            >
              {{ item.initials }}
            </v-avatar>
          </v-list-item-avatar>

          <v-list-item-content>
            <v-list-item-title>{{ item.fullName }}</v-list-item-title>
          </v-list-item-content>

          <v-list-item-action>
            <v-btn
              depressed
              small
            >
              View User

              <v-icon
                color="orange darken-4"
                right
              >
                mdi-open-in-new
              </v-icon>
            </v-btn>
          </v-list-item-action>
        </v-list-item>
      </template>
    </v-virtual-scroll> -->
  </v-card>
  </div>
</template>

<script>
import NavbarAdmin from '@/components/NavbarAdmin'

export default {
  components: {
    NavbarAdmin
  },
  data: () => ({
    user: {
      user_firstname: 'สิริวิมล',
      user_lastname: 'เพชรเวียง',
      user_email: 'friend.siriwimol@kkumail.com',
      user_tel: '0943706163',
      user_age: '22',
      user_district: 'บ้านไผ่',
      user_province: 'ขอนแก่น',
      user_type: 'นักศึกษา'
    },
    colors: ['#2196F3', '#90CAF9', '#64B5F6', '#42A5F5', '#1E88E5', '#1976D2', '#1565C0', '#0D47A1', '#82B1FF', '#448AFF', '#2979FF', '#2962FF'],
    names: ['Oliver', 'Jake', 'Noah', 'James', 'Jack', 'Connor', 'Liam', 'John', 'Harry', 'Callum', 'Mason', 'Robert', 'Jacob', 'Jacob', 'Jacob', 'Michael', 'Charlie', 'Kyle', 'William', 'William', 'Thomas', 'Joe', 'Ethan', 'David', 'George', 'Reece', 'Michael', 'Richard', 'Oscar', 'Rhys', 'Alexander', 'Joseph', 'James', 'Charlie', 'James', 'Charles', 'William', 'Damian', 'Daniel', 'Thomas', 'Amelia', 'Margaret', 'Emma', 'Mary', 'Olivia', 'Samantha', 'Olivia', 'Patricia', 'Isla', 'Bethany'],
    surnames: ['Smith', 'Anderson', 'Clark', 'Wright', 'Mitchell', 'Johnson', 'Thomas', 'Rodriguez', 'Lopez', 'Perez', 'Williams', 'Jackson', 'Lewis', 'Hill', 'Roberts', 'Jones', 'White', 'Lee', 'Scott', 'Turner', 'Brown', 'Harris', 'Walker', 'Green', 'Phillips', 'Davis', 'Martin', 'Hall', 'Adams', 'Campbell', 'Miller', 'Thompson', 'Allen', 'Baker', 'Parker', 'Wilson', 'Garcia', 'Young', 'Gonzalez', 'Evans', 'Moore', 'Martinez', 'Hernandez', 'Nelson', 'Edwards', 'Taylor', 'Robinson', 'King', 'Carter', 'Collins'],
    dialog: false,
    breadcrumbs: [
      {
        text: 'Dashboard',
        disabled: false,
        href: 'admindashboard'
      },
      {
        text: 'บัญชีผู้ใช้',
        disabled: true,
        href: 'adminprofile'
      }
    ]

  }),

  computed: {
    items () {
      const namesLength = this.names.length
      const surnamesLength = this.surnames.length
      const colorsLength = this.colors.length

      return Array.from({ length: 10000 }, (k, v) => {
        const name = this.names[this.genRandomIndex(namesLength)]
        const surname = this.surnames[this.genRandomIndex(surnamesLength)]

        return {
          color: this.colors[this.genRandomIndex(colorsLength)],
          fullName: `${name} ${surname}`,
          initials: `${name[0]} ${surname[0]}`
        }
      })
    }
  },

  methods: {
    genRandomIndex (length) {
      return Math.ceil(Math.random() * (length - 1))
    }
  }
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Prompt&display=swap');

*{
  font-family: 'Prompt', sans-serif !important;
}
.v-breadcrumbs >>> a {
    color: #fcad74 ;
}
</style>
