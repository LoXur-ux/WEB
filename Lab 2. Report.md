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

<p align=center>Адаптирование верстки под различные платформы
<p align=center>Отчет по лабораторной работе № 2
<p align=center>по дисциплине
<p align=center>«Web-программирование»
<p align=center>Вариант 2
<p><br><br>
<p align=right>Разработал студент гр. ИТб-2301-01-00 ________________ /Панишев Д.А./
<p align=right>Проверил ст. преподаватель _________________ /Земцов М.А./
<p align=right>Работа защищена с оценкой	«___________» «___» __________ 2022 г.
<p><br><br><br>
<p align=center>Киров 2022  

---
Цель работы: научится делать адавтиыне страницы с помощью vue. 
  
Задачи:
1. сделать верстку формы авторизации;
1. сделать для нее адавтивность;
1. сделать дополнительное задание в зависимости от варианта.
  
## Ход работы

### Создание формы

Задание: должна быть реализована форма с вводом логина и пароля, на форме в зависимости от устройтсва должна быть картинка: на широкоформатном экране картинка должна быть слева, на мобильном - сверху.

В ходе работы была реализована форма, скриншот которой приведен на рисунке 1. 

<p align=center><img src=./img/lab2/pc-frame.png>
<p align=center>Рисунок 1 - Форма авторизации для широкоформатных экранов
<p align=center>2

---
### Адаптивность
Адаптивность для задания была сделана с помощью компонетов vue. Единственный минус - невозможность делать это асинхронно, без перезагрузки страницы.

Проект при загрузке определяет ширину устройтсва и если она ниже 640 пикселей, то vue устанавливает разметке необходимый css-контейнер, с помощью которого элементы выстриваются в необходимом порядке.

Сриншот мобтльной версии приведн на рисунке 2.

<p align=center><img src=./img/lab2/mobile-frame.png>
<p align=center>Рисунок 2 - Форма авторизации для мобильных устройств
<p align=center>3

---
### Допольнительное задание
Задание варьируется от четности или нечетности номеров зачетной книжки.

Было реализовано задание для четных вариантов: сделать в поле ввода пароля "глаз", по нажатию на который либо пароль показывался, либо скрывался - отображался символами "*".

Кнопка скрытия и показа пароля находится справа в поле ввода пароля и выполнена в виде глаза. По умолчанию пароль скрыт.

Скриншот разных состояний приведен на рисунках 3-4.

<p align=center><img src=./img/lab2/close.png>
<p align=center>Рисунок 3 - Скрытый пароль

<p align=center><img src=./img/lab2/show.png>
<p align=center>Рисунок 4 - Не скрытый пароль

<p align=center>4

---
Код App.vue - файла, который объединяет все компоненты - привден в приложении А.

Код Login.vue, ответсвенный за всю форму авторизации приведен в приложении Б.

Все компоненты, которые были в ЛР1, также присутвсуют в этой лабораторной работе.

Вывод: в рамках лабораторной работы были выполнены все поставленные задачи, отработаны навыки создания адаптивности с помощью Vue.

<p align=center>5

---
<p align = center><b>Приложение А</b>
<p align = center>(обязательное) 
<p align = center><b>Листинг файла App.vue</b>  

    <template>
    <div id="app">
        <MyName firName="Данил" midName="Александрович" lstName="Панишев" class="block" />
        <Vyatsu class="block"/>
        <Login class="block"/>
    </div>
    </template>

    <script lang="ts">
    import MyName from '../../my-name/src/components/MyName.vue';
    import Vyatsu from '../../vyatsu/src/components/Vyatsu.vue';
    import Login from '../../login/src/components/Login.vue';

    export default {
    name: 'App',
    components: {
        MyName,
        Vyatsu,
        Login,
    },
    };
    </script>

    <style scoped>
    .block {
    margin: 0 auto 16px auto;
    }
    </style>

<p align=center>6

---
<p align = center><b>Приложение Б</b>
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
    }),

    created() {
        this.isPC = this.checkWidth();
        this.isShowPass = true;
        this.switchEye();
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

<p align=center>7

---