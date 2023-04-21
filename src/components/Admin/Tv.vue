<template>
  <div id="app">
  <v-app>
    <div>
      <v-banner color="grey lighten-1">
        <h3>{{ titleMain }}</h3>
        <template v-slot:actions>
          <v-chip outlined small class="ma-2" color="primary" >Vue {{ vueVersion }}</v-chip>
          <v-chip outlined small class="ma-2" color="primary" >Vuetify {{ vuetifyVersion }}</v-chip>
          <v-chip outlined small class="ma-2" color="primary" >Sortable {{ sortableVersion }}</v-chip>
        </template>
      </v-banner>
    </div>

    <div class="my-2">
      <v-btn @click="onClick">check data</v-btn>
    </div>
    <v-data-table
    :footer-props="{itemsPerPageText: 'แถวต่อหน้า',pageText: '{0}-{1} จาก {2}','items-per-page-all-text': 'ทั้งหมด'}"
        :items="alllesson"
        :headers="headers"
        :items-per-page="5"
        :search="search"
        :single-expand="singleExpand"
        :expanded.sync="expanded"
        item-key="name"
        no-data-text="ไม่พบข้อมูล"
        no-results-text="ไม่พบข้อมูลที่ค้นหา"
      >
        <template slot="data"  id="dragTable">
          <td>{{ lesson_id }}</td>
          <td>{{ lesson_name }}</td>
          <td>{{ lesson_nameimg }}</td>
        </template>
        <template v-slot:item.edit="{ item }">
          <v-icon small @click="editItem(item)" color="#56a062">
            mdi-pencil
          </v-icon>
        </template>
        <template v-slot:item.delete="{ item }">
          <v-icon small @click="deleteItem(item)" color="#ea5859">
            mdi-delete
          </v-icon>
        </template>

      </v-data-table>

      <v-simple-table>
      <template>
        <thead>
          <tr class="grey lighten-1">
            <th class="text-left">Order</th>
            <th class="text-left">Id</th>
            <th class="text-left">Name</th>
          </tr>
        </thead>
        <tbody id="dragTable">
          <tr
            v-for="item in alllesson"
            :key="itemKey(item)"
            class="sortableRow sortHandle handle"
          >
            <td>{{ orderNumber(item) }}</td>
            <td>{{ item.lesson_id }}</td>
            <td>{{ item.lesson_name }}</td>
          </tr>
        </tbody>
      </template>
    </v-simple-table>

    <v-simple-table>
      <template>
        <thead>
          <tr class="grey lighten-1">
            <th class="text-left">Order</th>
            <th class="text-left">Id</th>
            <th class="text-left">Name</th>
          </tr>
        </thead>
        <tbody id="dragTable">
          <tr
            v-for="item in desserts"
            :key="itemKey(item)"
            class="sortableRow sortHandle handle"
          >
            <td>{{ orderNumber(item) }}</td>
            <td>{{ item.id }}</td>
            <td>{{ item.name }}</td>
            <td>{{ item.calories }}</td>
          </tr>
        </tbody>
      </template>
    </v-simple-table>

    <v-dialog
      v-model="dialog"
    >
      <v-card>
        <v-card-title>desserts
          <v-spacer></v-spacer>
          <v-btn outlined color="info" text @click="dialog = false">CLOSE</v-btn>
        </v-card-title>
        <v-card-text class="pprint">{{ pprint }}</v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn outlined color="info" text @click="dialog = false">CLOSE</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-app>
</div>
</template>

<script>
import axios from 'axios'
import Sortable from 'sortablejs'

export default {
  el: '#app',
  name: 'table-work-place',
  data () {
    return {
      alllesson: [],
      titleMain: "Drag & Drop of Vuetify's v-simple-table using SortableJS",
      titleSub: '[Vuetify 2.0.3 + SortableJS 1.10.1]',
      memo: 'UPDATE：2020-02-06 ',
      dialog: false,
      itemKeys: new WeakMap(),
      currentItemKey: 0,
      headers: [
        {
          text: 'บทที่',
          align: 'start',
          value: 'lesson_id'
        },
        { text: 'บทเรียนเรื่อง', value: 'lesson_name' },
        // { text: 'เนื้อหาบทเรียน', value: 'lesson_description' },
        { text: 'แก้ไข', value: 'edit', sortable: false },
        { text: 'ลบ', value: 'delete', sortable: false }
      ],

      desserts: [
        { id: 1001, name: 'Frozen Yogurt', calories: 159 },
        { id: 1002, name: 'Ice cream sandwich', calories: 237 },
        { id: 1003, name: 'Eclair', calories: 262 },
        { id: 1004, name: 'Cupcake', calories: 305 },
        { id: 1005, name: 'Gingerbread', calories: 356 },
        { id: 1006, name: 'Jelly bean', calories: 375 },
        { id: 1007, name: 'Lollipop', calories: 392 },
        { id: 1008, name: 'Honeycomb', calories: 408 },
        { id: 1009, name: 'Donut', calories: 452 },
        { id: 1010, name: 'KitKat', calories: 518 }
      ]
    }
  },
  created () {
    this.getLesson()
  },
  mounted () {
    /* eslint-disable no-new */
    new Sortable(
      document.querySelector('#dragTable'),
      {
        draggable: '.sortableRow',
        handle: '.sortHandle',
        onEnd: this.dragReorder
      }
    )
  },
  computed: {
    pprint: {
      get () { return JSON.stringify(this.desserts, undefined, 4) }
    },
    sortableVersion: {
      get () { return Sortable.version }
    }
  },
  methods: {
    async getLesson () {
      axios.get('http://localhost/vue-backend/editLesson.php').then((res) => {
        console.log('data:', res.data)
        if (res.data) {
          this.alllesson = res.data
        }
      })
    },
    itemKey (item) {
      if (!this.itemKeys.has(item)) this.itemKeys.set(item, ++this.currentItemKey)
      return this.itemKeys.get(item)
    },
    dragReorder ({ oldIndex, newIndex }) {
      const movedItem = this.desserts.splice(oldIndex, 1)[0]
      this.desserts.splice(newIndex, 0, movedItem)
    },
    orderNumber (item) {
      return this.desserts.indexOf(item) + 1
    },
    onClick () {
      this.dialog = true
    }
  }
}
</script>
<style scoped>
.handle {
  cursor: move;
}
tbody tr:nth-of-type(even) {
  background-color: rgba(0, 0, 0, 0.01);
}
tbody tr:nth-of-type(odd) {
  background-color: rgba(0, 0, 0, 0.05);
}
.pprint {
  white-space:pre-wrap;
  word-wrap:break-word;
}
</style>
