<template>
  <div class="container d-flex justify-content-center align-items-center p-4">
    <div
      v-if="show" 
      class="form-group anim"
    >
      <h1 for="exampleInputEmail1">Write your city and learn temperature in your city</h1>
      <input 
        type="text"
        class="form-control border-0 mt-4 p-3 text-center shadow bg-white rounded"
        id="exampleInputEmail1"
        v-model="city"
        @focus="hideError"
        autocomplete="off"
        aria-describedby="city"
        placeholder="Enter city"
      >
      <small 
        v-if="errorShow"
        id="emailHelp" 
        class="form-text alert alert-danger w-100 d-block anim"
      >
        {{ this.error }}
      </small>
      <button 
        type="button"
        class="btn border-0 bg-dark text-white bg-gradient w-100 p-3 mt-2"
        @click="sendCity"
      >
        Use
      </button>
    </div>
    <div
      v-if="!show"
      class="block anim p-3 rounded text-light"
    >
      <h2 class="mb-4"> {{ object.name }} </h2>
      <div
        @click="start" 
        class="return d-flex justify-content-center align-items-center">
        <svg xmlns="http://www.w3.org/2000/svg" width="48" height="48" viewBox="0 0 48 48" fill="none">
          <g clip-path="url(#clip0_1_2)">
            <path
              d="M26.561 0H2.561C2.009 0 1.561 0.447 1.561 1C1.561 1.553 2.009 2 2.561 2H26.561C36.486 2 44.561 10.075 44.561 20C44.561 29.925 36.486 38 26.561 38H4.975L11.268 31.707L9.854 30.293L1.855 38.292C1.854 38.293 1.854 38.293 1.853 38.294L1.146 39L1.852 39.706C1.853 39.707 1.853 39.707 1.854 39.708L9.853 47.707L11.267 46.293L4.975 40H26.561C37.589 40 46.561 31.028 46.561 20C46.561 8.972 37.588 0 26.561 0Z"
              fill="white" />
          </g>
          <defs>
            <clipPath id="clip0_1_2">
              <rect width="47.707" height="47.707" fill="white" />
            </clipPath>
          </defs>
        </svg>
      </div>
      <div class="d-flex justify-content-around align-items-center">
        <img 
          :src="icon"
          :alt="object.weather[0].main"
          :title="object.weather[0].main"
        />
        <p> {{ temperature }} °C </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',

  data() {
    return {
      city: '',
      key: '3c5296fc88242a5c2837975f1a42f965',
      error: '',
      errorShow: false,
      object: {},
      show: true,
      temperature: 0,
      icon: '',
    }
  },

  methods: {
    sendCity() {
      this.error = 'City is empty'

      if (this.city.length > 0) {
        this.errorShow = false
        fetch(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=${this.key}&exclude=current`)
          .then((response) => response.json())
          .then((data) => {

            if ((Object.hasOwnProperty.call(data, 'message'))) {
              const capitalize = str => str.charAt(0).toUpperCase() + str.slice(1);
              this.error = (capitalize(data.message))
              this.errorShow = true
            } else {
              this.show = false
              return this.object = data
            }
          });
      } else {
        this.errorShow = true
      }
    },

    hideError() {
      this.errorShow = false
    },

    generateIcon() {
      this.icon = `https://openweathermap.org/img/wn/${this.object.weather[0].icon}@2x.png`
    },

    translateTemp() {
      this.temperature = (this.object.main.temp - 273).toFixed(1)
    },

    start() {
      this.show = true
      this.city = ''
      this.object = null
    },
  },

  watch: {
    show() {
      if (!this.show) {
        this.generateIcon()
        this.translateTemp()
      }
    }
  },
}
</script>

<style scoped>
.container {
  position: relative;
}

.anim {
  animation: fade 0.3s ease-in;
}

.return {
  position: absolute;
  top: 8px;
  right: 8px;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  transition: 0.3s;
  cursor: pointer;
}
.return svg {
  width: 22px;
  height: 22px;
}

.return path {
  transition: 0.3s;
}

.return:hover {
  background: #FFF;
  box-shadow: 0px 0px 40px 0px rgba(255, 255, 255, 0.4);
}

.return:hover path {
  fill: #252525;
}


.block {
  position: relative;
  background: #252525;
  min-width: 600px;
}

input::-webkit-input-placeholder {
  transition: 0.3s;
}

input:focus::-webkit-input-placeholder {
  color: transparent;
}

input:-moz-placeholder {
  transition: 0.3s;
}

input:focus:-moz-placeholder {
  color: transparent;
}

input:focus.form-control  {
  border-color: #ced4da;
}

@media screen and (max-width: 700px) {
  .block {
    width: 100%;
    min-width: unset;
  }
}
</style>
