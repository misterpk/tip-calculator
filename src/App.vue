<template>
  <div id="app">
    <div class="calculator">
      <div>
        <div class="people-section">
          <h2>Number of People</h2>
          <div>
            <div>
              <a class="button" @click="setNumPeople(1)" v-bind:class="{selected: activePeopleButton === 1}">1</a>
              <a class="button" @click="setNumPeople(2)" v-bind:class="{selected: activePeopleButton === 2}">2</a>
              <a class="button" @click="setNumPeople(3)" v-bind:class="{selected: activePeopleButton === 3}">3</a>
              <a class="button" @click="setNumPeople(4)" v-bind:class="{selected: activePeopleButton === 4}">4</a>
              <a class="button" @click="showCustomNumPeople" v-bind:class="{selected: activePeopleButton === 5}">5+</a>
            </div>
            <div>
              <input class="people"
                     type="number"
                     min="0"
                     v-bind:class="{hide: hidePeopleField}"
                     v-model.number="numPeople" />
            </div>
          </div>
        </div>
        <div class="tip-section">
          <h2>Percent Tip</h2>
          <div>
            <div  >
              <a class="button" @click="setTip(15)" v-bind:class="{selected: activeTipButton === 1}">15</a>
              <a class="button" @click="setTip(18)" v-bind:class="{selected: activeTipButton === 2}">18</a>
              <a class="button" @click="setTip(20)" v-bind:class="{selected: activeTipButton === 3}">20</a>
              <a class="button" @click="showCustomTip" v-bind:class="{selected: activeTipButton === 4}">Custom</a>
            </div>
            <div v-bind:class="{hide: hideTipField}">
              <input class="tip"
                     type="number"
                     min="0"
                     v-model.number="tip"/>
              %
            </div>
          </div>
        </div>
        <div class="results-section">
          <h2>Bill Total</h2>
          $
          <input
              type="number"
              class="total"
              min="0"
              v-model="billTotal"
          />
        </div>
      </div>
      <div>
        <div>
          <button @click="calculate">Calculate</button>
        </div>
        <div v-bind:class="{hide: hideResults}">
          <h4>Tip per person: ${{ tipPerPerson }}</h4>
          <h4>Total per person: ${{ totalPerPerson }}</h4>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
    import * as math from 'mathjs';
    export default {
        name: "app",
        data() {
            return {
                numPeople: 1,
                hidePeopleField: true,
                hideTipField: true,
                tip: 15,
                billTotal: 0,
                tipPerPerson: 0,
                totalPerPerson: 0,
                activePeopleButton: 0,
                activeTipButton: 0,
                hideResults: true,
            };
        },
        methods: {
            calculate() {
                this.tipPerPerson = math.round(math.eval((this.billTotal * (this.tip / 100)) / this.numPeople), 2);
                this.totalPerPerson = math.round(math.eval((this.billTotal / this.numPeople) + this.tipPerPerson), 2);
                this.hideResults = false;
            },
            setNumPeople(people) {
                this.activePeopleButton = 0;
                this.hidePeopleField = true;
                this.activePeopleButton = people;
                this.numPeople = people;
            },
            showCustomNumPeople() {
                this.activePeopleButton = 0;
                this.activePeopleButton = 5;
                this.hidePeopleField = false;
            },
            showCustomTip() {
                this.activeTipButton = 0;
                this.activeTipButton = 4;
                this.hideTipField = false;
            },
            setTip(tip) {
                this.activeTipButton = 0;
                this.hideTipField = true;
                switch (tip) {
                    case 15:
                        this.activeTipButton = 1;
                        this.tip = tip;
                        break;
                    case 18:
                        this.activeTipButton = 2;
                        this.tip = tip;
                        break;
                    case 20:
                        this.activeTipButton = 3;
                        this.tip = tip;
                        break;
                    default:
                        this.activeTipButton = 4;
                        this.tip = tip;
                        break;
                }
            }
        }
    };
</script>

<style>
  body {
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    background: #f5f5f5;
  }

  div {
    padding: 10px 0;
  }

  a.button:hover {
    background-color: #b9ca4d;
  }

  .calculator {
    width: 425px;
    margin: 20px auto;
    padding: 35px;
    background-color: #fff;
  }

  .hide {
    display: none;
  }

  .selected {
    background-color: #cab34d
  }

  .flex {
    display: flex;
    justify-content: space-between;
    align-content: center;
    width: 80%;
  }

  input.total {
    width: 50%;
  }

  input.people {
    width: 50%;
  }

  input.tip {
    width: 50%;
  }
</style>
