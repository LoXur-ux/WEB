<p align=center>ФЕДЕРАЛЬНОЕ ГОСУДАРСТВЕННОЕ БЮДЖЕТНОЕ ОБРАЗОВАТЕЛЬНОЕ
<p align=center>УЧРЕЖДЕНИЕ ВЫСШЕГО ОБРАЗОВАНИЯ
<p align=center>«ВЯТСКИЙ ГОСУДАРСТВЕННЫЙ УНИВЕРСИТЕТ»
<p align=center>Институт математики и информационных систем
<p align=center>Факультет автоматики и вычислительной техники
<p align=center>Кафедра систем автоматизации управления
<p><br>
<p align=right>Дата сдачи на проверку:
<p align=right>«15» апреля 2022 г.
<p align=right>Проверено:
<p align=right>«__» апреля 2022 г.

<p align=center>Работа Vue с таблицами и модальными окнами
<p align=center>Отчет по лабораторной работе № 5
<p align=center>по дисциплине
<p align=center>«Web-программирование»
<p><br><br>
<p align=right>Разработал студент гр. ИТб-2301-01-00 ________________ /Панишев Д.А./
<p align=right>Проверил ст. преподаватель _________________ /Земцов М.А./
<p align=right>Работа защищена с оценкой	«___________» «___» __________ 2022 г.
<p><br><br><br>
<p align=center>Киров 2022  

---
Цель работы: создать таблицу, для которой будут подгружаться данные с mock-сервера, а также модальное окно с информацией.
  
Задачи:
1. реализавть таблицу, в которую будут поступать данные о студентах;
1. написать два get запроса для получения данных одного и нескольких студентов для таблицы с mock-сервера;
1. сдалть модальное окно, которое при нажатии на строчку таблицы будет выводить такую же информацию. 
  
## Ход работы
### Mock-сервер

Были сделаны два get запроса на сайте [Postman](https://www.postman.com/):
1. запрос с получением информации (id, имя, фамилия, средний балл) о трех студентах (рисунок 1);
1. запрос с получением информации об одном студенте (рисунок 2).

<p align=center><img src="./img/lab5/postman-all.png">
<p align=center>Рисунок 1 - Запрос с тремя студентами

<p align=center><img src="./img/lab5/postman-one.png">
<p align=center>Рисунок 2 - Запрос с одним студентом

<p align=center>2

---
### Get запросы с помощью Axios
Были написаны два get запроса с получением данных. Они активировались только после нажатия одной из двух кнопок, каждая из которых отвечает за свой запрос.

Листинг двух запросов:

    findAll() {
        axios
            .get(`${this.url}/all`)
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
            .get(`${this.url}/one`)
            .then((res) => {
            console.log(res.data);
            this.list = res.data;
            })
            .catch((err) => {
            console.log(err);
            });
    },

<p align=center>3

---
### Таблица
Была реализована таблица с четырмя столбцами. Для получения данных в таблицу были сделаны две кнопки для получения информации об одном и нескольких студентах соответсвенно. Скриншот представлен на рисунке 3.

<p align=center><img src="./img/lab5/table.png">
<p align=center>Рисунок 3 - Таблица, с уже полученными данными

По умолчанию таблица пуста. Ее контент появляется только после нажатия на одну из кнопок. При их повторном нажатии контент обновляется.

<p align=center>4

---
### Модальное окно
Было сделано модальное окно, которое дублирует информацию из таблицы при нажатии на одну из строк. Скриншот окна представлен на рисунке 4.

<p align=center><img src="./img/lab5/modal.png">
<p align=center>Рисунок 4 - Модальное окно

При нажатии на кнопку 'ОК' окно закрывается.

Листинг всего пакета Table.vue представлен в приложении А.

Вывод: была реализована таблица, в которую по кнопке поступают данные с mockc-сервера, и при нажатии на строку появляется модальное окно, дублирующее информацию из выбранной строки.

<p align=center>5

---
<p align = center><b>Приложение А</b>
<p align = center>(обязательное) 
<p align = center><b>Листинг файла Table.vue</b>  

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

    export default {
    name: 'Table',
    data: () => ({
        showModal: Boolean,
        id: String,
        firName: String,
        secName: String,
        avgScore: String,
        url: String,
        stud: {},
        list: [],
    }),
    created() {
        this.showModal = false;
        this.url = 'https://ca4952d7-b451-4173-96a4-879f680514d1.mock.pstmn.io/stud';
    },
    methods: {
        findAll() {
        axios
            .get(`${this.url}/all`)
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
            .get(`${this.url}/one`)
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
    };
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
    position: fixed;
    top: 0;
    left: 0;
    min-height: 100%;
    width: 100%;
    background: rgba(0, 0, 0, 0.75);
    }

    .modal {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);

    padding: 6px;

    text-align: center;
    background: linear-gradient(45deg, #6e5642, #253547);
    border-radius: 6px;

    transition: 0.3s all;
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

    .modal-table tr td {
    margin: 4px 0;
    padding: 4px 8px;
    }

    .modal-table tr td:first-child {
    text-align: right;
    }

    .modal-table tr td:last-child {
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


<p align=center>6

---