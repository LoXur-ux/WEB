<template>
  <div class="container">
    <div class="but-conteiner">
      <button @click="findOne()" class="getter">Получить одного</button>
      <button @click="findAll()" class="getter">Получить всех</button>
    </div>
    <table class="main-table">
      <tr>
        <th>Id</th>
        <th>Имя</th>
        <th>Фамилия</th>
        <th>Средняя оценка</th>
      </tr>
      <tr v-for="stud in list" :key="stud.id" @click="setField(stud.id)">
        <td>{{ stud.id }}</td>
        <td>{{ stud.firName }}</td>
        <td>{{ stud.secName }}</td>
        <td>{{ stud.avgScore }}</td>
      </tr>
    </table>

    <div v-if="showModal" class="modal-shadow">
      <div class="modal">
        <div class="head">
          <h3 style="margin: 0 0 8px 0; color: white">Студент</h3>
        </div>
        <div class="body">
          <table class="modal-table">
            <tr>
              <td>Id:</td>
              <td>{{ id }}</td>
            </tr>
            <tr>
              <td>Имя:</td>
              <td>{{ firName }}</td>
            </tr>
            <tr>
              <td>Фамилия:</td>
              <td>{{ secName }}</td>
            </tr>
            <tr>
              <td>Средний балл:</td>
              <td>{{ avgScore }}</td>
            </tr>
          </table>

          <button @click="closeModal()" class="closer">OK</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component';
import axios from 'axios';

const url = 'https://ca4952d7-b451-4173-96a4-879f680514d1.mock.pstmn.io/stud';

@Options({
  data: () => ({
    showModal: Boolean,
    id: String,
    firName: String,
    secName: String,
    avgScore: String,
    stud: {},
    list: [],
  }),
  created() {
    this.showModal = false;
  },
  methods: {
    findAll() {
      axios
        .get(`${url}/all`)
        .then((res) => {
          console.log(res.data);
          this.list = res.data;
        })
        .catch((err) => {
          console.log(err);
        });
    },
    findOne() {
      axios
        .get(`${url}/one`)
        .then((res) => {
          console.log(res.data);
          this.list = res.data;
        })
        .catch((err) => {
          console.log(err);
        });
    },
    setField(i: number) {
      for (let index = 0; index < this.list.length; index++) {
        const stud = this.list[index];
        if (stud.id === i) {
          console.log(stud);
          this.showModal = true;
          this.id = stud.id;
          this.firName = stud.firName;
          this.secName = stud.secName;
          this.avgScore = stud.avgScore;
        }
      }
    },
    closeModal() {
      this.showModal = false;
    },
  },
})
export default class MyTable extends Vue {}
</script>

<style scoped>
.container {
  width: 400px;
  margin: 0 auto;
}

.but-conteiner {
  display: flex;
  justify-content: space-between;
}

.main-table {
  width: 100%;
  margin-top: 12px;
  margin-bottom: 12px;
  border-spacing: 0;
}

.main-table tr:first-child {
  background: linear-gradient(45deg, #6e5642, #253547);
  color: white;
}
.main-table th:first-child {
  border-top-left-radius: 6px;
}
.main-table th:last-child {
  border-top-right-radius: 6px;
}

.main-table th,
td {
  padding: 6px 12px;
  border-width: 0 1px 1px 0;
  border-style: solid;
  border-color: white;
}

.main-table td {
  background: rgb(238, 238, 238);
}

.getter {
  width: 188px;
  height: 42px;

  font-size: 1.2em;
  color: white;
  background: linear-gradient(45deg, #6e5642, #253547);
  border: 0;
  border-radius: 6px;
  border-color: #e7d379;
}
button:hover {
  transform: scale(1.05);
}

button:active {
  transform: scale(0.98);
}

.modal-shadow {
  position: absolute;
  top: 0;
  left: 0;
  min-height: 100%;
  width: 100%;
  background: rgba(0, 0, 0, 0.39);
}

.modal {
  position: absolute;
  top: 50%;
  left: 50%;

  padding: 6px;

  text-align: center;
  background: linear-gradient(45deg, #6e5642, #253547);
  border-radius: 6px;

  transform: translate(-50%, -50%);
}

.head {
  text-align: center;
  margin: 4px 0;
}

.body {
  padding: 12px;
  background: white;
  border-radius: 6px;
}

.modal-table tr td{
  margin: 4px 0;
  padding: 4px 8px;
}

.modal-table tr td:first-child{
  text-align: right;
}

.modal-table tr td:last-child{
  text-align: left;
}

.closer {
  padding: 4px 16px;
  margin-top: 12px;

  border: 4px solid transparent;
  border-image: linear-gradient(45deg, #6e5642, #253547);
  border-image-slice: 1;
  border-radius: 3px;
}
</style>
