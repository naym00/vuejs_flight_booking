<template>
  <div>
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
      <b class="FlightForYou" style="font-size: 150%; color: #dcdcdc"
        >FlightForYou.Com</b
      >
      <button
        class="navbar-toggler"
        type="button"
        data-toggle="collapse"
        data-target="#navbarSupportedContent"
        aria-controls="navbarSupportedContent"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>

      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul
          class="navbar-nav"
          style="
            display: flex;
            justify-content: flex-end;
            width: 100%;
            padding: 0;
          "
        >
          <li class="nav-item">
            <a class="nav-link btn btn-dark" href="#">Offers</a>
          </li>
          <li class="nav-item">
            <a class="nav-link btn btn-dark" href="#">Manage</a>
          </li>
          <li class="nav-item">
            <a class="nav-link btn btn-dark" href="#">Experience</a>
          </li>
          <li class="nav-item">
            <a class="nav-link btn btn-dark" href="#">Marco Polo Club</a>
          </li>
          <li class="nav-item">
            <a class="nav-link btn btn-dark" href="#">Business</a>
          </li>
        </ul>
      </div>
    </nav>

    <div class="container bg-transparent m-auto mt-5">
      <!-- input fields start -->
      <div class="container">
        <div class="mt-4">
          <div class="row bg-secondary">
            <div class="col-md-4 mb-3 mt-4">
              <input
                id="from_location"
                type="text"
                class="form-control text-center"
                style="height: 50px"
                placeholder="Start From"
                @input="searchfrom()"
              />
              <div
                class="from-list"
                v-if="statusfrom"
                style="position: absolute; z-index: 1000"
                id="flightfrom-list"
              >
                <table>
                  <tr
                    v-for="(list, index) in flight_from_api"
                    :key="index"
                    style="cursor: pointer; background-color: whitesmoke"
                  >
                    <td class="h6" scope="col" @click="setFlightFrom(list)">
                      <b>{{ list }}</b>
                    </td>
                  </tr>
                </table>
              </div>
            </div>

            <div class="col-md-4 mb-3 mt-4">
              <input
                id="to_location"
                type="text"
                class="form-control text-center"
                style="height: 50px"
                placeholder="Destination"
                aria-label="Arrival"
                @input="searchto()"
              />
              <div
                class="from-list"
                v-if="statusto"
                style="position: absolute; z-index: 1000"
                id="flightto-list"
              >
                <table>
                  <tr
                    v-for="(list, index) in flight_to_api"
                    :key="index"
                    style="cursor: pointer; background-color: whitesmoke"
                  >
                    <td scope="col" @click="setFlightTo(list)">
                      <b>{{ list }}</b>
                    </td>
                  </tr>
                </table>
              </div>
            </div>

            <div class="col-md-4 mb-3 mt-4 rounded">
              <HotelDatePicker
                @check-in-changed="checkInDate"
                @check-out-changed="checkOutDate"
                format="YYYY-MM-DD"
              >
              </HotelDatePicker>
            </div>
            <button
              type="button"
              class="btn btn-primary mb-4 rounded"
              style="margin: auto; width: 45%"
              v-on:click="getFlight()"
            >
              {{ Flight_Search }}
            </button>
          </div>
        </div>
      </div>
      <!-- input fields start -->
    </div>

    <div v-if="lower_portion" class="container">
      <div class="row mt-4">
        <p class="lg_font">
          <b>Weather in {{ location }}</b>
        </p>
      </div>
      <div class="row">
        <div
          class="col-md-3 mb-3 rounded border"
          style="background-color: blanchedalmond"
        >
          <div class="row">
            <p style="margin: auto"><b>Morning (05:00 AM - 11:00 AM)</b></p>
          </div>
          <div class="row mt-3">
            <p style="font-size: 150%; margin: auto" v-if="cel">
              Temperature: {{ morning_temperature }}<sup>o</sup>C
            </p>
            <p style="font-size: 150%; margin: auto" v-if="fer">
              Temperature: {{ morning_temperature }}<sup>o</sup>F
            </p>
          </div>
          <div class="row">
            <i
              style="margin: auto; font-size: 300%"
              class="fas fa-cloud-sun"
            ></i>
          </div>
          <div class="row mt-3">
            <p style="font-size: 150%; margin: auto">
              Humidity: {{ morning_humidity }}%
            </p>
          </div>
        </div>

        <div
          class="col-md-3 mb-3 rounded border"
          style="background-color: blanchedalmond"
        >
          <div class="row" style="margin: auto">
            <p style="margin: auto"><b>Afternoon (12:00 PM - 05:00 PM)</b></p>
          </div>
          <div class="row mt-3">
            <p style="font-size: 150%; margin: auto" v-if="cel">
              Temperature: {{ afternoon_temperature }}<sup>o</sup>C
            </p>
            <p style="font-size: 150%; margin: auto" v-if="fer">
              Temperature: {{ afternoon_temperature }}<sup>o</sup>F
            </p>
          </div>
          <div class="row">
            <i
              style="margin: auto; font-size: 300%"
              class="fas fa-cloud-showers-heavy"
            ></i>
          </div>
          <div class="row mt-3">
            <p style="font-size: 150%; margin: auto">
              Humidity: {{ afternoon_humidity }}%
            </p>
          </div>
        </div>

        <div
          class="col-md-3 mb-3 rounded border"
          style="background-color: blanchedalmond"
        >
          <div class="row" style="margin: auto">
            <p style="margin: auto"><b>Evening (06:00 PM - 09:00 PM)</b></p>
          </div>
          <div class="row mt-3">
            <p style="font-size: 150%; margin: auto" v-if="cel">
              Temperature: {{ evening_temperature }}<sup>o</sup>C
            </p>
            <p style="font-size: 150%; margin: auto" v-if="fer">
              Temperature: {{ evening_temperature }}<sup>o</sup>F
            </p>
          </div>
          <div class="row">
            <i
              style="margin: auto; font-size: 300%"
              class="fas fa-cloud-moon"
            ></i>
          </div>
          <div class="row mt-3">
            <p style="font-size: 150%; margin: auto">
              Humidity: {{ evening_humidity }}%
            </p>
          </div>
        </div>

        <div
          class="col-md-3 mb-3 rounded border"
          style="background-color: blanchedalmond"
        >
          <div class="row" style="margin: auto">
            <p style="margin: auto"><b>Overnight (10:00 PM - 04:00 AM)</b></p>
          </div>
          <div class="row mt-3">
            <p style="font-size: 150%; margin: auto" v-if="cel">
              Temperature: {{ overnight_temperature }}<sup>o</sup>C
            </p>
            <p style="font-size: 150%; margin: auto" v-if="fer">
              Temperature: {{ overnight_temperature }}<sup>o</sup>F
            </p>
          </div>
          <div class="row">
            <i style="margin: auto; font-size: 300%" class="fas fa-moon"></i>
          </div>
          <div class="row mt-3">
            <p style="font-size: 150%; margin: auto">
              Humidity: {{ overnight_humidity }}%
            </p>
          </div>
        </div>
      </div>
      <div class="row">
        <div class="col bg_color lg_font rounded border">
          <p v-if="cel">
            Day's Max Temperature: {{ maximum_temperature }}<sup>o</sup>C
          </p>
          <p v-if="fer">
            Day's Max Temperature: {{ maximum_temperature }}<sup>o</sup>F
          </p>
        </div>
        <div class="col bg_color lg_font rounded border">
          <p v-if="cel">
            Day's Avg Temperature: {{ average_temperature }}<sup>o</sup>C
          </p>
          <p v-if="fer">
            Day's Avg Temperature: {{ average_temperature }}<sup>o</sup>F
          </p>
        </div>
        <div class="col bg_color lg_font rounded border">
          <p v-if="cel">
            Day's Min Temperature: {{ minimum_temperature }}<sup>o</sup>C
          </p>
          <p v-if="fer">
            Day's Min Temperature: {{ minimum_temperature }}<sup>o</sup>F
          </p>
        </div>
      </div>

      <div class="row">
        <div class="col bg_color lg_font rounded border">
          <p>Day's Max Humidity: {{ maximum_humidity }}%</p>
        </div>
        <div class="col bg_color lg_font rounded border">
          <p>Day's Avg Humidity: {{ average_humidity }}%</p>
        </div>
        <div class="col bg_color lg_font rounded border">
          <p>Day's Min Humidity: {{ minimum_humidity }}%</p>
        </div>
      </div>

      <div class="row">
        <button
          style="margin: auto"
          type="button"
          class="btn btn-primary w-25"
          v-on:click="c_f()"
        >
          {{ c_f_converter }}
        </button>
      </div>

      <div class="row mt-4">
        <table>
          <tr>
            <th>Flight</th>
            <th>Departure</th>
            <th>Arrival</th>
            <th>Duration</th>
            <th>Cost</th>
          </tr>
          <tr v-for="user in users" :key="user.id">
            <th>{{ user.Flight }}</th>
            <th>
              {{ user.Departure }}
            </th>
            <th>
              {{ user.Arrival }}
            </th>
            <th>
              {{ user.Duration }}
            </th>
            <th>
              {{ user.Cost }}
            </th>
          </tr>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
var axios = require("axios").default;
import HotelDatePicker from "vue-hotel-datepicker";
import "vue-hotel-datepicker/dist/vueHotelDatepicker.css";
import usersData from "./users.json";
export default {
  data() {
    return {
      users: usersData,

      lower_portion: false,
      cel: true,
      fer: false,
      morning_temperature: 0,
      morning_temperature_c: 0,
      morning_temperature_f: 0,
      morning_humidity: 0,

      afternoon_temperature: 0,
      afternoon_temperature_c: 0,
      afternoon_temperature_f: 0,
      afternoon_humidity: 0,

      evening_temperature: 0,
      evening_temperature_c: 0,
      evening_temperature_f: 0,
      evening_humidity: 0,

      overnight_temperature: 0,
      overnight_temperature_c: 0,
      overnight_temperature_f: 0,
      overnight_humidity: 0,

      maximum_temperature: 0,
      maximum_temperature_c: 0,
      maximum_temperature_f: 0,

      average_temperature: 0,
      average_temperature_c: 0,
      average_temperature_f: 0,

      minimum_temperature: 0,
      minimum_temperature_c: 0,
      minimum_temperature_f: 0,

      maximum_humidity: 0,
      average_humidity: 0,
      minimum_humidity: 0,

      location: "",
      Flight_Search: "Find Flight",
      c_f_converter: "Convert To Fahrenheit",
      start_location: "",
      statusfrom: null,
      statusto: null,
      flight_from_api: [],
      flight_to_api: [],
      to: null,
      dates: {
        in: new Date(),
        out: new Date(),
      },
    };
  },
  methods: {
    checkInDate(date) {
      let d = new Date(date.getTime());
      this.dates.in =
        d.getFullYear().toString() +
        "-" +
        (d.getMonth() + 1).toString() +
        "-" +
        d.getDate().toString();
    },
    checkOutDate(date) {
      let d = new Date(date.getTime());
      this.dates.out =
        d.getFullYear().toString() +
        "-" +
        (d.getMonth() + 1).toString() +
        "-" +
        d.getDate().toString();
    },
    searchfrom() {
      this.calculation_for_fetch_location("from_location", 1);
    },
    setFlightFrom(list) {
      this.start_location = list;
      document.getElementById("flightfrom-list").style.display = "none";
      document.getElementById("from_location").value = list;
      document.getElementById("from_location").disabled = true;
    },
    searchto() {
      this.calculation_for_fetch_location("to_location", 2);
    },
    setFlightTo(list) {
      document.getElementById("flightto-list").style.display = "none";
      document.getElementById("to_location").value = list;
      this.location = list.split(",")[0] + "," + list.split(",")[1] + ".";
      document.getElementById("to_location").disabled = true;
    },
    calculation_for_fetch_location(input_id, flag) {
      let value_from_input = document.getElementById(input_id).value;

      if (value_from_input.length == 0) {
        if (flag == 1) {
          this.statusfrom = false;
        } else if (flag == 2) {
          this.statusto = false;
        }
      } else {
        if (flag == 1) {
          this.statusfrom = true;
        } else if (flag == 2) {
          this.statusto = true;
        }
        axios
          .get(
            "https://api.sharetrip.net/api/v1/flight/search/airport?name=" +
              value_from_input
          )
          .then((res) => {
            let object = res.data;
            let object_length = object.response.length;

            let fullName = object.response[0].name;
            let myArray = fullName.split("(");
            let result = myArray[0].substring(0, myArray[0].length - 1);
            let result_2 = result.split(",");

            if (flag == 1) {
              this.flight_from_api = [];

              for (let i = 0; i < object_length; i++) {
                let full_string = object.response[i].name;
                let city_name = object.response[i].city;
                let two_array = full_string.split("(");
                let result = two_array[0]
                  .substring(0, two_array[0].length - 1)
                  .split(",");
                this.flight_from_api[i] =
                  result[0] + ", " + city_name + "," + result[1];
              }
            } else if (flag == 2) {
              this.flight_to_api = [];

              for (let i = 0; i < object_length; i++) {
                let full_string = object.response[i].name;
                let city_name = object.response[i].city;
                let two_array = full_string.split("(");
                let result = two_array[0]
                  .substring(0, two_array[0].length - 1)
                  .split(",");
                if (
                  (
                    result[0] +
                    ", " +
                    city_name +
                    "," +
                    result[1]
                  ).localeCompare(this.start_location) != 0
                ) {
                  this.flight_to_api[i] =
                    result[0] + ", " + city_name + "," + result[1];
                }
              }
            }
          })
          .catch((err) => {
            console.log(err);
          });
      }
    },
    getFlight() {
      //
      console.log(usersData);
      //

      let from_full_str = document.getElementById("from_location").value;
      let from_three_array = from_full_str.split(",");
      let from_country_name = from_three_array[0];
      let from_city_name = from_three_array[1].substring(
        1,
        from_three_array[1].length
      );

      let to_full_str = document.getElementById("to_location").value;
      let to_three_array = to_full_str.split(",");
      let to_country_name = to_three_array[0];
      let to_city_name = to_three_array[1]
        .substring(1, to_three_array[1].length)
        .toLowerCase();

      let d = new Date(new Date().getTime());
      let date =
        d.getFullYear().toString() +
        "-" +
        (d.getMonth() + 1).toString() +
        "-" +
        d.getDate().toString();

      let options = {
        method: "GET",
        url: "https://weatherapi-com.p.rapidapi.com/forecast.json",
        params: { q: to_city_name, days: "1", dt: date },
        headers: {
          "x-rapidapi-host": "weatherapi-com.p.rapidapi.com",
          "x-rapidapi-key":
            "a4c5b7253amsh12e0df7106eed0ep163511jsna8874ea91118",
        },
      };

      axios
        .request(options)
        .then((response) => {
          let object = response.data;
          // 12 Hours Format        24 Hours Format          Period
          //    12:00 AM                	00:00             Overnight
          //    01:00 AM                 	01:00             Overnight
          //    02:00 AM                 	02:00             Overnight
          //    03:00 AM 	                03:00             Overnight
          //    04:00 AM                 	04:00             Overnight
          //    05:00 AM 	                05:00              Morning
          //    06:00 AM 	                06:00              Morning
          //    07:00 AM                	07:00              Morning
          //    08:00 AM 	                08:00              Morning
          //    09:00 AM                 	09:00              Morning
          //    10:00 AM                 	10:00              Morning
          //    11:00 AM                 	11:00              Morning
          //    12:00 PM 	                12:00             Afternoon
          //    01:00 PM 	                13:00             Afternoon
          //    02:00 PM 	                14:00             Afternoon
          //    03:00 PM 	                15:00             Afternoon
          //    04:00 PM 	                16:00             Afternoon
          //    05:00 PM 	                17:00             Afternoon
          //    06:00 PM 	                18:00              Evening
          //    07:00 PM 	                19:00              Evening
          //    08:00 PM 	                20:00              Evening
          //    09:00 PM 	                21:00              Evening
          //    10:00 PM 	                22:00             Overnight
          //    11:00 PM 	                23:00             Overnight

          let mtc = 0;
          let mtf = 0;
          let mh = 0;

          let atc = 0;
          let atf = 0;
          let ah = 0;

          let etc = 0;
          let etf = 0;
          let eh = 0;

          let otc = 0;
          let otf = 0;
          let oh = 0;

          let humidity_arr = [];
          for (let i in object.forecast.forecastday[0].hour) {
            if (i >= 5 && i <= 11) {
              mtc += object.forecast.forecastday[0].hour[i].temp_c;
              mtf += object.forecast.forecastday[0].hour[i].temp_f;
              mh += object.forecast.forecastday[0].hour[i].humidity;
            } else if (i >= 12 && i <= 17) {
              atc += object.forecast.forecastday[0].hour[i].temp_c;
              atf += object.forecast.forecastday[0].hour[i].temp_f;
              ah += object.forecast.forecastday[0].hour[i].humidity;
            } else if (i >= 18 && i <= 21) {
              etc += object.forecast.forecastday[0].hour[i].temp_c;
              etf += object.forecast.forecastday[0].hour[i].temp_f;
              eh += object.forecast.forecastday[0].hour[i].humidity;
            } else {
              otc += object.forecast.forecastday[0].hour[i].temp_c;
              otf += object.forecast.forecastday[0].hour[i].temp_f;
              oh += object.forecast.forecastday[0].hour[i].humidity;
            }
            humidity_arr.push(object.forecast.forecastday[0].hour[i].humidity);
          }
          mtc = mtc / 7;
          mtf = mtf / 7;
          mh = mh / 7;

          atc = atc / 6;
          atf = atf / 6;
          ah = ah / 6;

          etc = etc / 4;
          etf = etf / 4;
          eh = eh / 4;

          otc = otc / 7;
          otf = otf / 7;
          oh = oh / 7;

          this.lower_portion = true;
          this.morning_temperature = parseFloat(
            (mtc * 1.00000001).toString().split(".")[0] +
              "." +
              (mtc * 1.00000001).toString().split(".")[1].substring(0, 2)
          );
          this.morning_temperature_c = this.morning_temperature;
          this.morning_temperature_f = parseFloat(
            (mtf * 1.00000001).toString().split(".")[0] +
              "." +
              (mtf * 1.00000001).toString().split(".")[1].substring(0, 2)
          );

          this.afternoon_temperature = parseFloat(
            (atc * 1.00000001).toString().split(".")[0] +
              "." +
              (atc * 1.00000001).toString().split(".")[1].substring(0, 2)
          );
          this.afternoon_temperature_c = this.afternoon_temperature;
          this.afternoon_temperature_f = parseFloat(
            (atf * 1.00000001).toString().split(".")[0] +
              "." +
              (atf * 1.00000001).toString().split(".")[1].substring(0, 2)
          );

          this.evening_temperature = parseFloat(
            (etc * 1.00000001).toString().split(".")[0] +
              "." +
              (etc * 1.00000001).toString().split(".")[1].substring(0, 2)
          );
          this.evening_temperature_c = this.evening_temperature;
          this.evening_temperature_f = parseFloat(
            (etf * 1.00000001).toString().split(".")[0] +
              "." +
              (etf * 1.00000001).toString().split(".")[1].substring(0, 2)
          );

          this.overnight_temperature = parseFloat(
            (otc * 1.00000001).toString().split(".")[0] +
              "." +
              (otc * 1.00000001).toString().split(".")[1].substring(0, 2)
          );
          this.overnight_temperature_c = this.overnight_temperature;
          this.overnight_temperature_f = parseFloat(
            (otf * 1.00000001).toString().split(".")[0] +
              "." +
              (otf * 1.00000001).toString().split(".")[1].substring(0, 2)
          );

          this.morning_humidity = parseFloat(
            (mh * 1.00000001).toString().split(".")[0] +
              "." +
              (mh * 1.00000001).toString().split(".")[1].substring(0, 2)
          );
          this.afternoon_humidity = parseFloat(
            (ah * 1.00000001).toString().split(".")[0] +
              "." +
              (ah * 1.00000001).toString().split(".")[1].substring(0, 2)
          );
          this.evening_humidity = parseFloat(
            (eh * 1.00000001).toString().split(".")[0] +
              "." +
              (eh * 1.00000001).toString().split(".")[1].substring(0, 2)
          );
          this.overnight_humidity = parseFloat(
            (oh * 1.00000001).toString().split(".")[0] +
              "." +
              (oh * 1.00000001).toString().split(".")[1].substring(0, 2)
          );

          let max_tem_c = object.forecast.forecastday[0].day.maxtemp_c;
          let max_tem_f = object.forecast.forecastday[0].day.maxtemp_f;

          let avg_tem_c = object.forecast.forecastday[0].day.avgtemp_c;
          let avg_tem_f = object.forecast.forecastday[0].day.avgtemp_f;

          let min_tem_c = object.forecast.forecastday[0].day.mintemp_c;
          let min_tem_f = object.forecast.forecastday[0].day.mintemp_f;

          let max_hum = Math.max(...humidity_arr);
          let avg_hum = object.forecast.forecastday[0].day.avghumidity;
          let min_hum = Math.min(...humidity_arr);

          this.maximum_temperature = parseFloat(
            (max_tem_c * 1.00000001).toString().split(".")[0] +
              "." +
              (max_tem_c * 1.00000001).toString().split(".")[1].substring(0, 2)
          );
          this.maximum_temperature_c = this.maximum_temperature;
          this.maximum_temperature_f = parseFloat(
            (max_tem_f * 1.00000001).toString().split(".")[0] +
              "." +
              (max_tem_f * 1.00000001).toString().split(".")[1].substring(0, 2)
          );

          this.average_temperature = parseFloat(
            (avg_tem_c * 1.00000001).toString().split(".")[0] +
              "." +
              (avg_tem_c * 1.00000001).toString().split(".")[1].substring(0, 2)
          );
          this.average_temperature_c = this.average_temperature;
          this.average_temperature_f = parseFloat(
            (avg_tem_f * 1.00000001).toString().split(".")[0] +
              "." +
              (avg_tem_f * 1.00000001).toString().split(".")[1].substring(0, 2)
          );

          this.minimum_temperature = parseFloat(
            (min_tem_c * 1.00000001).toString().split(".")[0] +
              "." +
              (min_tem_c * 1.00000001).toString().split(".")[1].substring(0, 2)
          );
          this.minimum_temperature_c = this.minimum_temperature;
          this.minimum_temperature_f = parseFloat(
            (min_tem_f * 1.00000001).toString().split(".")[0] +
              "." +
              (min_tem_f * 1.00000001).toString().split(".")[1].substring(0, 2)
          );

          this.maximum_humidity = parseFloat(
            (max_hum * 1.00000001).toString().split(".")[0] +
              "." +
              (max_hum * 1.00000001).toString().split(".")[1].substring(0, 2)
          );
          this.average_humidity = parseFloat(
            (avg_hum * 1.00000001).toString().split(".")[0] +
              "." +
              (avg_hum * 1.00000001).toString().split(".")[1].substring(0, 2)
          );
          this.minimum_humidity = parseFloat(
            (min_hum * 1.00000001).toString().split(".")[0] +
              "." +
              (min_hum * 1.00000001).toString().split(".")[1].substring(0, 2)
          );
        })
        .catch(function (error) {
          console.error(error);
        });
    },
    c_f() {
      if (this.c_f_converter.localeCompare("Convert To Fahrenheit") == 0) {
        this.morning_temperature = this.morning_temperature_f;
        this.afternoon_temperature = this.afternoon_temperature_f;
        this.evening_temperature = this.evening_temperature_f;
        this.overnight_temperature = this.overnight_temperature_f;
        this.maximum_temperature = this.maximum_temperature_f;
        this.average_temperature = this.average_temperature_f;
        this.minimum_temperature = this.minimum_temperature_f;
        this.cel = false;
        this.fer = true;
        this.c_f_converter = "Convert To Celsius";
      } else if (this.c_f_converter.localeCompare("Convert To Celsius") == 0) {
        this.morning_temperature = this.morning_temperature_c;
        this.afternoon_temperature = this.afternoon_temperature_c;
        this.evening_temperature = this.evening_temperature_c;
        this.overnight_temperature = this.overnight_temperature_c;
        this.maximum_temperature = this.maximum_temperature_c;
        this.average_temperature = this.average_temperature_c;
        this.minimum_temperature = this.minimum_temperature_c;
        this.cel = true;
        this.fer = false;
        this.c_f_converter = "Convert To Fahrenheit";
      }
    },
  },
  components: {
    HotelDatePicker,
  },
};
</script>

<style scoped>
table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

td,
th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}

tr:nth-child(even) {
  background-color: #dddddd;
}

.bg_color {
  background-color: blanchedalmond;
}
.lg_font {
  font-size: 150%;
}
.FlightForYou {
  position: relative;
  animation-name: example;
  animation-duration: 5s;
}

@keyframes example {
  0% {
    color: #dcdcdc;
    font-size: 150%;
    left: 0px;
    top: 0px;
  }

  50% {
    color: #d2b48c;
    font-size: 150%;
    left: 200px;
    top: 0px;
  }

  100% {
    color: #dcdcdc;
    font-size: 150%;
    left: 0px;
    top: 0px;
  }
}
</style>