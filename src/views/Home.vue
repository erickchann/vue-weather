<template>
  <div id="phone" :class="`${time}`">
    <div class="search">
      <input type="text" name="city" id="city" placeholder="Search..." @keyup.enter="getData" v-model="query">
    </div>

    <div class="details">
      <h2><i class="fas fa-map-marker-alt"></i> {{result.name}}, {{result.sys.country}}</h2>
      <p>{{getTime()}}</p>

      <div class="weather">
        <h3>{{Math.round(result.main.temp)}}°C - {{result.weather[0].main}}</h3>
      </div>
    </div>
  </div>
</template>


<script>
import axios from "axios";

export default {
  data() {
    return {
      key: '02ff15f05075a8938af166f92f7ab9a3',
      url: 'http://api.openweathermap.org/data/2.5/',
      query: 'Pontianak',
      time: '',
      result: {}
    }  
  },
  methods: {
    getData() {
      axios(`${this.url}weather?q=${this.query}&units=metric&APPID=${this.key}`).then((res) => {
        this.result = res.data
      });
    },
    getTime() {
      let date = new Date();  
      const arrDate = ["Sunday","Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      const arrMonth = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];

      let day = arrDate[date.getDay()];
      let month = arrMonth[date.getMonth()];
      let year = date.getFullYear();

      return `${day}, ${month} ${year}`;
    },
    bgChanger() {
      let app = document.querySelector("#phone");

      let date = new Date();
      let time = date.toLocaleTimeString('en-US', { hour: 'numeric', minute: 'numeric', hour12: true });
      let arrTime = time.split(" ");
      
      this.time = arrTime[arrTime.length -1];
    }
  },
  created() {
    this.getData();
    this.getTime();
    this.bgChanger();
  },
  mounted() {
  }
}
</script>

<style>

#main {
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

#phone {
  border-radius: 10px;
  background-color: white;
  width: 100vw;
  height: 100vh;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  transition: 0.3s;
}

#phone.AM {
  background-image: url(../assets/morning.png);
}

#phone.PM {
  background-image: url(../assets/night.png);
}

@media screen and (min-width: 400px) {
  #phone {
    height: 70vh;
    width: 300px;
  }
}

.search {
  display: flex;
  justify-content: center;
}

.details {
  display: flex;
  justify-content: center;
  color: #fff;
  flex-direction: column;
  text-align: center;
}

.details h2 {
  margin: 20px 0 0 0;
}

.details p {
  margin: 10px 0;
}

.weather h3 {
  font-style: italic;
}

#city {
  border: none;
  outline: none;
  background-color: rgba(255, 255, 255, 0.7);
  height: 30px;
  width: 80%;
  padding: 0 20px;
  border-radius: 100px;
  margin: 20px auto;
}

</style>
