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
          <p class="price">
            Aktulana cena: {{ valuePrice[0] }}{{ valueChar }}
          </p>
          <button @click="buy('doubleclick')">Buy it</button>
        </div>
        <div class="option">
          <h1>Automatyczny klikacz</h1>
          <p>
            <strong>Automatyczny klikacz</strong> zarabia za ciebie automatycznie. Co sekundę
            do twojego portfela trafia wartość jednego kliknięcia. Za każdym razem, gdy go
            zakupisz jego cena będzie wzrastała dwukrotnie!
          </p>
          <p class="price">
            Aktualna cena: {{ valuePrice[1] }}{{ valueChar }}
          </p>
          <button @click="buy('autoclick')">Buy it</button>
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

let appValue = 250;
if (localStorage.getItem('value')) {
  appValue = JSON.parse(localStorage.getItem('value'));
}
let incrementValue = 1;
if (localStorage.getItem('increment')) {
  incrementValue = JSON.parse(localStorage.getItem('increment'));
}
let shopping = [100, 250];
if (localStorage.getItem('shopping')) {
  shopping = JSON.parse(localStorage.getItem('shopping'));
}

let autoClicker = false;
if (localStorage.getItem('autoClicker')) {
  autoClicker = JSON.parse(localStorage.getItem('autoClicker'));
}
const valueLetters = ['k', 'M', 'B'];
export default {
  name: 'App',
  components: {
    Additionals,
  },
  data() {
    return {
      value: appValue,
      valueChar: '$',
      valueAfter: 0,
      valueLetter: null,
      seen: false,
      addsActive: false,
      optsActive: false,
      valuePrice: [shopping[0], shopping[1]],
      autoClickActive: autoClicker,
    };
  },
  created() {
    this.setValue();
    if (this.autoClickActive === true) {
      this.autoClicker();
    }
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
      localStorage.setItem('shopping', JSON.stringify(this.valuePrice));
      localStorage.setItem('increment', JSON.stringify(incrementValue));
      localStorage.setItem('autoClicker', JSON.stringify(this.autoClickActive));
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
    buy(item) {
      if (item === 'doubleclick') {
        appValue -= this.valuePrice[0];
        this.valuePrice[0] *= 2;
        incrementValue *= 2;
        this.setValue();
      } else if (item === 'autoclick') {
        appValue -= this.valuePrice[1];
        this.valuePrice[1] *= 2;
        this.autoClickActive = true;
        this.setValue();
        this.autoClicker();
      }
    },
    autoClicker() {
      setInterval(() => {
        this.increment();
        this.setValue();
      }, 1000);
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

      .price {
        margin-top: 1vh;
        font-size: 2vh;
        line-height: 2vh;
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
