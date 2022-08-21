<template>
  <div class="appContainer">
    <main class="calculatorContainer">
      <section class="actionHistory">
        <h5 class="process">
          <!-- <span v-if="history">{{ history ? history : '' }}</span> -->
          <!-- <span v-else>{{ screen ? screen : '0' }}</span> -->
        </h5>
        <h5 class="process">
          <span v-if="screen">{{ screen ? screen : '' }}</span>
          <span v-else-if="history">{{ history ? history : '' }}</span>
          <span v-else>0</span>
        </h5>
        <h4 class="result">{{ sonuc ? sonuc + ' =' : '' }}</h4>
      </section>
      <section class="numpad">
        <div class="numeric">
          <button v-on:click="yaz(7)" class="col-3"> <span>7</span> </button>
          <button v-on:click="yaz(8)" class="col-3"> <span>8</span> </button>
          <button v-on:click="yaz(9)" class="col-3"> <span>9</span> </button>
          <button v-on:click="action('/')" class="col-3"> <span>/</span> </button>
          <button v-on:click="yaz(4)" class="col-3"> <span>4</span> </button>
          <button v-on:click="yaz(5)" class="col-3"> <span>5</span> </button>
          <button v-on:click="yaz(6)" class="col-3"> <span>6</span> </button>
          <button v-on:click="action('*')" class="col-3"> <span>*</span> </button>
          <button v-on:click="yaz(1)" class="col-3"> <span>1</span> </button>
          <button v-on:click="yaz(2)" class="col-3"> <span>2</span> </button>
          <button v-on:click="yaz(3)" class="col-3"> <span>3</span> </button>
          <button v-on:click="action('-')" class="col-3"> <span>-</span> </button>
          <button v-on:click="action('=')" class="col-3 aspect-0"> <span>=</span> </button>
          <button v-on:click="yaz(0)" class="col-3 aspect-0"> <span>0</span> </button>
          <button v-on:click="yaz('.')" class="col-3 aspect-0"> <span>.</span> </button>
          <button v-on:click="action('+')" class="col-3"> <span>+</span> </button>
          <button v-on:click="action('c')" class="col-6"> <span>AC</span> </button>
          <button v-on:click="backspace()" class="col-6"> <span>←</span> </button>
        </div>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  data () {
    return {
      screen: '',
      sonuc: '',
      operator: '',
      history: ''
    }
  },
  methods: {
    yaz (s) {
      this.screen += s
    },
    backspace () {
      this.screen = this.screen.substring(0, this.screen.length - 1)
    },
    action (action) {
      if (action === 'c') {
        this.sonuc = ''
        this.screen = ''
        this.history = ''
      }
      if (action === '=') {
        const history = this.screen
        this.history = history
        const sonuc = eval(history) /* eslint no-eval: 0 */
        this.sonuc = sonuc
        this.screen = ''
      }
      var patt = /\+|\-|\*|\// /* eslint no-useless-escape: 0 */
      if (patt.test(this.screen)) {
        return false
      }
      if (action === '+') {
        this.screen += '+'
        this.operator = '+'
      }
      if (action === '-') {
        this.screen += '-'
        this.operator = '-'
      }
      if (action === '*') {
        this.screen += '*'
        this.operator = '*'
      }
      if (action === '/') {
        this.screen += '/'
        this.operator = '/'
      }
    }
  }
}
</script>

<style lang="scss">
$gap-spacing: .25rem;
body{
  padding: 0;
  margin: 0;
  height: 100vh;
  width: 100vw;

  #app {
    height: 100vh;
    width: 100vw;
    display: flex;
    justify-content: center;
    align-items: center;

    .appContainer {
      .calculatorContainer {
        border: 1px solid #dedede;
        padding: 5px;
        max-width: 150px;
        width: 150px;

        .actionHistory {
          padding: 10px 5px;
          text-align: right;
          border: 1px solid #dedede;
          margin-bottom: $gap-spacing;

          .result, .process {
            margin: 0;
            padding: 0;
          }
        }
        .numpad {
          .numeric {
            display: grid;
            grid-template-columns: repeat(12, minmax(0, 1fr));
            gap: $gap-spacing;
            width: 100%;

            button {
              width: 100%;
              border: 1px solid #dedede;
              aspect-ratio: 1 / 1;
              display: flex;
              justify-content: center;
              align-items: center;

              &:hover {
                transition: 0.3s;
                background-color: #dedede;
                cursor: pointer;
              }
              &.col-3 {
                grid-column-start: span 3;
                grid-column-end: span 1;
              }
              &.col-6 {
                grid-column-start: span 6;
                grid-column-end: span 1;
                aspect-ratio: 6 / 2.8
              }
            }
          }
        }
      }
    }
  }
}
</style>
