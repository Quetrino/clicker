<template>
  <div class="app">
    <div class="value" @click="increment()">
      <div v-if="seen" class="withLetter">
        {{ valueChar }}{{ valueAfter }}{{ valueLetter }}
      </div>
      <div v-else class="outLetter">
        {{ valueChar }}{{ value }}
      </div>
    </div>
    <Additionals class="additionals"
                 v-on:adds-click="addsClicked()"
                 v-on:opts-click="optsClicked()" />
    <footer>Created by Quetrino | 2018 &copy;</footer>
    <div class="adds">
      <div class="adds-content">
        <div class="option">
          <h1>Zwiększenie mnożnika x2</h1>
          <p>
            <strong>Zwiększenie mnożnika x2</strong> zwiększa twój mnożnik dwukrotnie.
            Możesz kupować to ulepszenie do znudzenia, lecz za każdym razem jego cena
            będzie podwajana.
          </p>
          <span class="price">
            Aktulana cena: {{ valuePrice[0] }}{{ valueChar }}
          </span>
        </div>
        <div class="option">
          <h1>Zwiększenie mnożnika</h1>
        </div>
      </div>
    </div>
    <div class="opts">
      <div class="opts-content"></div>
    </div>
  </div>
</template>

<script>
import $ from 'jquery';
import Additionals from './components/Additionals.vue';

let appValue = 40000000;
if (localStorage.getItem('value')) {
  appValue = JSON.parse(localStorage.getItem('value'));
}
const incrementValue = 10000000;
const valueLetters = ['k', 'M', 'B'];
export default {
  name: 'App',
  components: {
    Additionals,
  },
  data() {
    return {
      value: 0,
      valueChar: '$',
      valueAfter: 0,
      valueLetter: null,
      seen: false,
      addsActive: false,
      optsActive: false,
      valuePrice: [100],
    };
  },
  created() {
    this.setValue();
  },
  methods: {
    addsClicked() {
      $('.opts').stop(true).animate({ left: '100%' }, 1000);
      this.optsActive = false;
      const addsWidth = $('.adds').width() * (-1);
      if (this.addsActive === false) {
        this.addsActive = true;
        $('.adds').stop(true).animate({ left: 0 }, 1000);
      } else {
        this.addsActive = false;
        $('.adds').stop(true).animate({ left: addsWidth }, 1000);
      }
    },
    optsClicked() {
      const addsWidth = $('.adds').width() * (-1);
      this.addsActive = false;
      $('.adds').stop(true).animate({ left: addsWidth }, 1000);
      const optsWidth = $(window).innerWidth() - $('.opts').width();
      if (this.optsActive === false) {
        this.optsActive = true;
        $('.opts').stop(true).animate({ left: optsWidth }, 1000);
      } else {
        this.optsActive = false;
        $('.opts').stop(true).animate({ left: '100%' }, 1000);
      }
    },
    increment() {
      appValue += incrementValue;
      this.value = appValue;
      this.setValue();
      localStorage.clear();
      localStorage.setItem('value', JSON.stringify(appValue));
    },
    setValue() {
      if (appValue >= 1000) {
        this.seen = true;
        if (appValue >= 1000000) {
          if (appValue >= 1000000000) {
            this.valueAfter = appValue / 1000000000;
            [, , this.valueLetter] = valueLetters;
          } else {
            this.valueAfter = appValue / 1000000;
            [, this.valueLetter] = valueLetters;
          }
        } else {
          this.valueAfter = appValue / 1000;
          [this.valueLetter] = valueLetters;
        }
      }
      this.value = appValue;
    },
  },
};
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

input:focus,
select:focus,
textarea:focus,
button:focus {
    outline: none;
}

.app {
  height: 100vh;
  background: black;
  color: white;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-size: 1em;
  user-select: none;

  .value {
    width: 80%;
    height: 10vh;
    text-align: center;
    border-bottom: 1px solid #61a;
    line-height: 10vh;
    font-size: 4vh;
    user-select: none;
    outline: none;

    &:hover {
      color: #61a;
      cursor: pointer;
    }
  }

  .additionals {
    position: fixed;
    left: 0;
    top: 0;
    width: 100%;
    text-align: center;
  }

  footer {
    position: fixed;
    bottom: 0;
    left: 0;
    width: 100%;
    padding: 2.5vh;
    text-align: center;
  }

  .adds {
    position: fixed;
    top: 10vh;
    left: -100%;
    width: 100%;
    height: 90vh;
    background: #fff;
    color: black;
    overflow-x: hidden;
    overflow-y: scroll;

    .option {
      width: 100%;
      min-height: 15vh;
      padding: 2.5vh;
      text-align: center;
      border-bottom: 1px solid #61a;

      h1 {
        font-size: 3.5vh;
      }

      p {
        font-size: 2vh;
      }

      &:hover, &:focus {
        background: #61a;
        color: white;
        cursor: pointer;
      }
    }
  }

  .opts {
    position: fixed;
    top: 10vh;
    left: 100%;
    width: 100%;
    height: 90vh;
    background: #fff;
    color: black;
    overflow-x: hidden;
    overflow: scroll;

    .option {
      width: 100%;
    }
  }
}

@media all and (min-width: 1200px) {
  .app {
    .value {
      width: 20%;
      font-size: 5vh;
    }

    .adds {
      left: -30%;
      width: 30%;
    }

    .opts {
      left: 100%;
      width: 30%;
    }
  }
}
</style>
