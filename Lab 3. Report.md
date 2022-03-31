<p align=center>ФЕДЕРАЛЬНОЕ ГОСУДАРСТВЕННОЕ БЮДЖЕТНОЕ ОБРАЗОВАТЕЛЬНОЕ
<p align=center>УЧРЕЖДЕНИЕ ВЫСШЕГО ОБРАЗОВАНИЯ
<p align=center>«ВЯТСКИЙ ГОСУДАРСТВЕННЫЙ УНИВЕРСИТЕТ»
<p align=center>Институт математики и информационных систем
<p align=center>Факультет автоматики и вычислительной техники
<p align=center>Кафедра систем автоматизации управления
<p><br>
<p align=right>Дата сдачи на проверку:
<p align=right>«31» марта 2022 г.
<p align=right>Проверено:
<p align=right>«__» апреля 2022 г.

<p align=center>Post-запросы. Mock-сервер
<p align=center>Отчет по лабораторной работе № 3
<p align=center>по дисциплине
<p align=center>«Web-программирование»
<p><br><br>
<p align=right>Разработал студент гр. ИТб-2301-01-00 ________________ /Панишев Д.А./
<p align=right>Проверил ст. преподаватель _________________ /Земцов М.А./
<p align=right>Работа защищена с оценкой	«___________» «___» __________ 2022 г.
<p><br><br><br>
<p align=center>Киров 2022  

---
Цель работы: создавать post-запросы с помощью библиотеки axios. 
  
Задачи:
1. сделать mock-сервер;
1. написать post-запрос для отправик данных на сервер;
1. сделать для формы из предидущей лабораторной работы "функцианал" авторизации. 
  
## Ход работы
### Создание Mockserver'а
Mockserver - сервер, который можно использовать для тестривания приложений в условиях отсутсвия api. Он может как принимать запросы, обрабатывать их и отправлять на них ответ.

Для лабораторной работы был выбрать сервис [Postman](https://www.postman.com/), который позволяет сзодавать коллекции с данными, личный mock-сервер, настраивать ответы на запросы и многое другое.

Была создана коллекция с данными для входи и ответом "Hello World" (рисунок 1), а также сам mock-сервер. На рисунке 2 изображены два запроса: 
1. запрос, в котором не было данных, в "Response" получена ошибка;
1. запрос, в котором были верные данные, ответ - "Hello World".

<p align=center><img src=./img/lab3/requests.png>
<p align=center>Рисунок 1 - Требуемый запрос (сверху) и ответ (снизу)

<p align=center>2

---
<p align=center><img src=./img/lab3/server-requests.png>
<p align=center>Рисунок 2 - Запрос с ошибкой (сверху) и запрос с верными данными (снизу)

### Создание post-запроса
Post-запрос, в отличие от get, отправляет данные на сервер и ожидвет ответа от него. Он может получить как ошибку, так и ответ в какмо-либо виде. Axios же в свою очередь ожидвет ответа и может обработать ошибку полученную от сервера, чего не может например Fetch.

С помощью axios можно быстро и компактно реализовать post-запрос:

    axios
        .post(url, data, { headers })
        .then((res) => {
          alert("Successful Authorization!");
        })
        .catch((err) => {
          alert("Somthing was wrong!");
        });

В метод post подаются данные ссылки на сервер (url), данные в json вормате (data) и Heder запроса - его настройки.

### Практическое применение post-запроса
В форму из прошлой лабораторной работы встроить функционал отправки на mock-сервер логина и пароля для проверки, если данные совпадают, то непобходимо оповестить пользователя об успешном входе, при не совпадении - об ошибке.

Код файла с реализваций представлен в приложении А.

Скриншот успешной авторизации представлен на рисунке 3. Неудачи - рисунок 4.

<p align=center>3

---
<p align=center><img src=./img/lab3/succsess.png>
<p align=center>Рисунок 3 - Успешная авторизация
<p align=center><img src=./img/lab3/wrong.png>
<p align=center>Рисунок 4 - Неудачная авторизация

Все модули из предидущей лабораторной работы также присутвсуют в проекте, файл App.vue не изменился.

Вывод: в рамках лабораторной работы были получены навыки работы с mock-сервером и создания post-запросов с помощью библиотеки axios.

<p align=center>4

---
<p align = center><b>Приложение А</b>
<p align = center>(обязательное) 
<p align = center><b>Листинг файла Login.vue</b>  

    <template>
    <div :class="[checkWidth() ? 'container-pc' : 'container-phone']">
        <img v-if="isPC" src="../assets/left.png" class="left-img" />
        <img v-else src="../assets/top.png" class="top-img" />
        <div class="main-form" aria-hidden="false">
        <p>Login</p>
        <label for="login-inp" class="input-form">
            <input id="login-inp" type="text" />
        </label>

        <p>Password</p>
        <div class="pass-block input-form">
            <label for="pass-inp">
            <input id="pass-inp" :type="passType" />
            </label>
            <img class="show-pass" src="../assets/opened-eye.svg" @mousedown="switchEye" />
        </div>
        <button class="login" @click="sendRequest">Login!</button>
        </div>
    </div>
    </template>

    <script lang="ts">
    import axios from "axios";

    export default {
    name: "LoginForm",
    data: () => ({
        isPC: Boolean,
        isShowPass: Boolean,
        passType: String,
        url: String,
    }),

    created() {
        this.isPC = this.checkWidth();
        this.isShowPass = true;
        this.switchEye();
        this.url = "https://ca4952d7-b451-4173-96a4-879f680514d1.mock.pstmn.io";
    },

    methods: {
        checkWidth(): boolean {
        return window.innerWidth > 640;
        },

        switchEye() {
        this.isShowPass = !this.isShowPass;
        if (this.isShowPass) this.passType = "text";
        else this.passType = "password";
        },

        sendRequest() {
        const url = this.url + "/auth/check";
        const headers = {
            "Content-Type": "application/json",
            "x-mock-match-request-body": "true",
        };

        const data = {
            login: (document.getElementById("login-inp") as HTMLInputElement).value,
            password: (document.getElementById("pass-inp") as HTMLInputElement).value,
        };

        axios
            .post(url, data, { headers })
            .then((res) => {
            alert("Successful Authorization!");
            })
            .catch((err) => {
            alert("Somthing was wrong!");
            });
        },
    },
    };
    </script>

    <style scoped>
    .container-pc {
    width: 100%;
    margin: 0 0;
    display: flex;
    justify-content: center;
    }

    .container-phone {
    width: 100%;
    display: block;
    }

    .left-img {
    width: 30%;
    }

    .top-img {
    width: 100%;
    }

    .main-form {
    min-width: 360px;
    max-width: 640px;

    margin-top: 36px;
    text-align: center;
    }

    p {
    margin: 4px 0;
    }
    .pass-block {
    display: flex;
    justify-content: center;
    margin-left: -0.5em;
    }

    input {
    text-align: center;
    font-size: 1.5em;
    border: 3px solid transparent;
    border-radius: 0px;
    border-image: linear-gradient(45deg, #6e5642, #253547);
    -moz-border-image: -moz-linear-gradient(45deg, #6e5642, #253547);
    -webkit-border-image: -webkit-linear-gradient(45deg, #6e5642, #253547);
    border-image-slice: 1;
    margin-bottom: 16px;
    }

    .show-pass {
    height: 1.5em;
    vertical-align: center;
    margin-left: -2em;
    margin-top: 4px;
    }

    .login {
    width: 280px;
    height: 48px;
    margin: 16px 0;
    font-size: 2em;
    color: white;

    background: linear-gradient(45deg, #6e5642, #253547);
    border: 0;
    border-radius: 6px;
    border-color: #e7d379;
    }

    .login:hover {
    transform: scale(1.05);
    }

    .login:active {
    transform: scale(0.98);
    }
    </style>

<p align=center>5

---