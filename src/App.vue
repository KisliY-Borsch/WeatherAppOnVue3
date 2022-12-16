<template>
  <!--Да, можно было разбить на темплейты(я о декомпозиции) и сделать нормальный проект,
  но я очень торопился и старался сделать очень быстро(у меня на районе включают свет только на ночь и днем на 2 часа).
  Кстати, моя работа на нативном js мне понравилась больше, хоть vue и пластилиновый, но нативный js мне ближе.
  До сотрировки таблицы по колонкам я не успел дойти, хоть и понимаю как это можно сделать.
  Сейчас 07:29, я иду спать...-->
  <header>
    <a>Выбери нужные тебе города:
      <select @change="takeInfo">
        <option disabled selected>Выберите город</option>
        <option :key="name.id" v-for="name in cities" :value="name.api">{{name.city}}</option>
      </select>
    </a>
    <br>
    <a> Ты выбрал: <strong v-for="city in activeDays" :key="city">{{city}}, </strong></a>
    <br>
    <a>Прогноз на:
      <select v-model="forDays">
        <option value="1">Сегодня</option>
        <option value="2">3 дня</option>
        <option value="3">7 дней</option>
      </select>
    </a>
  </header>

  <!-- Не забыть разбить на темплейты! -->
  <div class="mobile-table">
    <table class="table_sort" id="CitiesTable" v-if="days1">
      <thead>
      <tr>
        <th @click="sortList('city')">Город</th>
        <th @click="sortList('tempMax')">Максимальная температура сегодня</th>
        <th @click="sortList('tempMin')">Минимальная температура сегодня</th>
        <th @click="sortList('date')">Дата и время</th>
        <th>Управление</th>
      </tr>
      </thead>
      <tbody id="CitiesTableBody">
        <tr v-bind:key="info.id" v-for="info in days">
          <td>{{info.city}}</td>
          <td>{{info.tempMax[0]}}<a>°C</a></td>
          <td>{{info.tempMin[0]}}<a>°C</a></td>
          <td>{{info.date}}</td>
          <td><button @click="deleteRow(info)">Delete</button></td>
        </tr>
      </tbody>
    </table>
  </div>
  <div class="mobile-table">
    <table class="table_sort" id="CitiesTable" v-if="days3">
      <thead>
      <tr>
        <th>Город</th>
        <th>Сегодня</th>
        <th>Завтра</th>
        <th>Послезавтра</th>
        <th>Управление</th>
      </tr>
      </thead>
      <tbody id="CitiesTableBody">
      <tr v-bind:key="info.id" v-for="info in days">
        <td>{{info.city}}</td>
        <td><a>Максимальная температура: </a>{{info.tempMax[0]}}<a>°C</a><br><a>Минимальная температура: </a>{{info.tempMin[0]}}<a>°C</a></td>
        <td><a>Максимальная температура: </a>{{info.tempMax[1]}}<a>°C</a><br><a>Минимальная температура: </a>{{info.tempMin[1]}}<a>°C</a></td>
        <td><a>Максимальная температура: </a>{{info.tempMax[2]}}<a>°C</a><br><a>Минимальная температура: </a>{{info.tempMin[2]}}<a>°C</a></td>
        <td><button @click="deleteRow(info)">Delete</button></td>
      </tr>
      </tbody>
    </table>
  </div>
  <div class="mobile-table">
    <table class="table_sort" id="CitiesTable" v-if="days7">
      <thead>
      <tr>
        <th>Город</th>
        <th>Сегодня</th>
        <th>Завтра</th>
        <th>Послезавтра</th>
        <th>{{this.date.day+3}}<a>.</a>{{this.date.month}}<a>.</a>{{this.date.year}}</th>
        <th>{{this.date.day+4}}<a>.</a>{{this.date.month}}<a>.</a>{{this.date.month}}</th>
        <th>{{this.date.day+5}}<a>.</a>{{this.date.month}}<a>.</a>{{this.date.month}}</th>
        <th>{{this.date.day+6}}<a>.</a>{{this.date.month}}<a>.</a>{{this.date.month}}</th>
        <th>Управление</th>
      </tr>
      </thead>
      <tbody id="CitiesTableBody">
      <tr v-bind:key="info.id" v-for="info in days">
        <td>{{info.city}}</td>
        <td><a>Максимальная температура: </a>{{info.tempMax[0]}}<a>°C</a><br><a>Минимальная температура: </a>{{info.tempMin[0]}}<a>°C</a></td>
        <td><a>Максимальная температура: </a>{{info.tempMax[1]}}<a>°C</a><br><a>Минимальная температура: </a>{{info.tempMin[1]}}<a>°C</a></td>
        <td><a>Максимальная температура: </a>{{info.tempMax[2]}}<a>°C</a><br><a>Минимальная температура: </a>{{info.tempMin[2]}}<a>°C</a></td>
        <td><a>Максимальная температура: </a>{{info.tempMax[3]}}<a>°C</a><br><a>Минимальная температура: </a>{{info.tempMin[3]}}<a>°C</a></td>
        <td><a>Максимальная температура: </a>{{info.tempMax[4]}}<a>°C</a><br><a>Минимальная температура: </a>{{info.tempMin[4]}}<a>°C</a></td>
        <td><a>Максимальная температура: </a>{{info.tempMax[5]}}<a>°C</a><br><a>Минимальная температура: </a>{{info.tempMin[5]}}<a>°C</a></td>
        <td><a>Максимальная температура: </a>{{info.tempMax[6]}}<a>°C</a><br><a>Минимальная температура: </a>{{info.tempMin[6]}}<a>°C</a></td>
        <td><button @click="deleteRow(info)">Delete</button></td>
      </tr>
      </tbody>
    </table>
  </div>
  <!-- Не забыть разбить на темплейт! -->
</template>

<script>
import axios from "axios";

  export default {
    data(){
      return{
        cities:[
          {id:1, city: "Киев", api: "https://api.open-meteo.com/v1/forecast?latitude=50.45&longitude=30.52&daily=temperature_2m_max,temperature_2m_min&timezone=Europe%2FBerlin"},
          {id:2, city: "Харьков", api: "https://api.open-meteo.com/v1/forecast?latitude=49.98&longitude=36.25&daily=temperature_2m_max,temperature_2m_min&timezone=Europe%2FBerlin"},
          {id:3, city: "Львов", api: "https://api.open-meteo.com/v1/forecast?latitude=49.84&longitude=24.02&daily=temperature_2m_max,temperature_2m_min&timezone=Europe%2FBerlin"},
          {id:4, city: "Одесса", api: "https://api.open-meteo.com/v1/forecast?latitude=46.49&longitude=30.74&daily=temperature_2m_max,temperature_2m_min&timezone=Europe%2FBerlin"},
          {id:5, city: "Днепр", api: "https://api.open-meteo.com/v1/forecast?latitude=48.47&longitude=35.04&daily=temperature_2m_max,temperature_2m_min&timezone=Europe%2FBerlin"},
          {id:6, city: "Херсон", api: "https://api.open-meteo.com/v1/forecast?latitude=46.64&longitude=32.61&daily=temperature_2m_max,temperature_2m_min&timezone=Europe%2FBerlin"},
          {id:7, city: "Черкассы", api: "https://api.open-meteo.com/v1/forecast?latitude=49.44&longitude=32.06&daily=temperature_2m_max,temperature_2m_min&timezone=Europe%2FBerlin"},
        ],
        forDays: "1",/*Для скольки дней(нужно для срипта)*/
        activeDays: [],/*Массив для проверки при выборе(если город есть в массиве, он его добавлять повторно не будет, а выведет алерт о том, что город уже выбран)*/
        date: {day: new Date().getDate(), month: new Date().getMonth(), year: new Date().getFullYear()},
        days1: false,/**/
        days3: false,/*Сколько дней нужно человеку(нужно для отображения нужной таблицы пользователю)*/
        days7: false,/**/
        days: [],/*Массив из объектов с инфой для всех выбранных дней*/
      }
    },
    methods:{
      sortList(){
      },
      /*Функция получения данных по API*/
     async takeInfo(event){
       this.activeDays.push(event.target.options[event.target.options.selectedIndex].text);/*Отображаем какие города выбраны*/
       this.days1 = false;/**/
       this.days3 = false;/*Обнуление инфы о кол-ве дней*/
       this.days7 = false;/**/
       try{
          const response = await axios.get(event.target.value);
          /*Добавляем логику сортировки по дням*/
          if(this.forDays == 1){/*для 1 дня*/
            this.days1 = true;
            const newDay = {
              id: Date.now(),
              city: event.target.options[event.target.options.selectedIndex].text,
              tempMax: response.data.daily.temperature_2m_max,
              tempMin: response.data.daily.temperature_2m_min,
              date: "Время: " + new Date().toLocaleTimeString() + ", Дата: " + new Date().toLocaleDateString(),
              day: response.data.daily.time,
            }
            this.days.push(newDay);
            console.log(this.days);
          }else if(this.forDays == 2){/*для 3-х дней*/
            this.days3 = true;
            const newDay = {
              id: Date.now(),
              city: event.target.options[event.target.options.selectedIndex].text,
              tempMax: response.data.daily.temperature_2m_max,
              tempMin: response.data.daily.temperature_2m_min,
              date: "Время: " + new Date().toLocaleTimeString() + ", Дата: " + new Date().toLocaleDateString(),
              day: response.data.daily.time,
            }
            this.days.push(newDay);
            console.log(this.days);
          }else{/*для недели*/
            this.days7 = true;
            const newDay = {
              id: Date.now(),
              city: event.target.options[event.target.options.selectedIndex].text,
              tempMax: response.data.daily.temperature_2m_max,
              tempMin: response.data.daily.temperature_2m_min,
              date: "Время: " + new Date().toLocaleTimeString() + ", Дата: " + new Date().toLocaleDateString(),
              day: response.data.daily.time,
            }
            this.days.push(newDay);
            console.log(this.days);
          }
        }catch(event){
          alert("F**k, error!");
        }
      },
      deleteRow(info){
        this.days = this.days.filter(i => i.id !== info.id);
      },
    },
    watch:{},
    computed:{

    }
  }
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Balsamiq+Sans:ital@1&display=swap');
html, body{
  margin: 0;
  font-family: 'Balsamiq Sans', cursive;
  background-image: url("./components/style/1.jpg");
  background-repeat: no-repeat;
  background-position: center center;
  background-attachment: fixed;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
}
table#CitiesTable{
  text-decoration: none;
  border-collapse:collapse;
  width:100%;
  text-align:center;
}
table#CitiesTable th{
  font-family: 'Balsamiq Sans', cursive;
  font-weight:normal;
  font-size:16px;
  color:#ffffff;
  background:#354251;
}
table#CitiesTable th{
  white-space:pre-wrap;
  padding:10px 5px;
  line-height:13px;
  vertical-align: middle;
  border: 1px solid #354251;
  cursor:pointer;
}
table#CitiesTable tr:hover{
  backdrop-filter: blur(8px);
}
td{
  white-space:pre-wrap;
  padding:10px 5px;
  vertical-align: middle;
  border: 2px solid black;
  cursor: default	;
}
.mobile-table{
  width: 100%;
  max-width: 100%;
  overflow-x: auto;
}
select{
  background: white;
}
#app {
  font-size: 20px;
  font-family: 'Balsamiq Sans', cursive;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
