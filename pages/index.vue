<template>
  <main>
    <div class="main__titleContainer"><h1>SPLITTER</h1></div>
    <section class="main__appContainer">
      <div class="appContainer__BillContainer">
        <h2>Bill</h2>
        <div class="xd">
          <span></span><input type="number" placeholder="0.00" v-model="inputBill" />
        </div>
      </div>
      <div class="appContainer__percentajeContainer">
        <h2>Select Tip %</h2>
        <div>
          <button @click="setpercentaje" value="5">5%</button
          ><button @click="setpercentaje" value="10">10%</button>
          <button @click="setpercentaje" value="15">15%</button
          ><button @click="setpercentaje" value="25">25%</button>
          <button @click="setpercentaje" value="50">50%</button
          ><input
            type="number"
            class="custom"
            placeholder="Custom"
            @keyup="customPercentaje($event)"
          />
        </div>
      </div>
      <div class="appContainer__peopleNumber">
        <section><h2>Number of People</h2><pre v-show="danger">Cant't be zero</pre></section>
        <div class="xd" :class="{ danger: danger }" >
          <span></span><input type="number" placeholder="0" @keyup="peopleformatter($event)" v-model="inputPeople"  />
        </div>
      </div>
      <div class="appContainer__results">
        <div class="results">
          <article>
            <p>Tip Amount</p>
            <p>/ Person</p>
          </article>
          <p class="results__value">${{ tipCalculator }}</p>
        </div>
        <div class="results">
          <article>
            <p>Total</p>
            <p>/ Person</p>
          </article>
          <p class="results__value">${{ totalCalculator }}</p>
        </div>
        <button class="results__button" @click="reset">RESET</button>
      </div>
    </section>
  </main>
</template>
<script>
export default {
  data() {
    return {
      percentaje: "",
      inputBill: "",
      inputPeople: "",
      tipAmount: 0,
      danger: false,
    };
  },
  computed: {
    tipCalculator() {
      if (
        this.inputPeople &&
        this.inputBill &&
        this.percentaje &&
        this.inputPeople != 0
      ) {
        this.tipAmount =
          (this.inputBill * (this.percentaje / 100)) / this.inputPeople;
        this.tipAmount = parseFloat(this.tipAmount).toFixed(2);
        return this.tipAmount;
      } else {
        return 0;
      }
    },
    totalCalculator() {
      if (
        this.inputBill &&
        this.inputPeople &&
        this.percentaje &&
        this.inputPeople != 0
      ) {
        const total =
          (parseFloat(this.inputBill) + parseFloat(this.tipAmount)*this.inputPeople) /
          this.inputPeople;
        return total.toFixed(2);
      } else {
        return 0;
      }
    },
  },
  watch:{
    inputPeople(value){
      if (parseInt(value)===0) {
        this.danger=true
      }else{
        this.danger=false
      }
    }
  },
  methods: {
    peopleformatter(event){
      const input=event.target
      input.value=input.value.replace('.','')

    },
    reset(){
      this.percentaje = ""
      this.inputBill = ""
      this.inputPeople = ""
      this.tipAmount = 0
      document.querySelector('.custom').value=''
      this.resetpercentajes()
    },
    resetpercentajes(){
      const pastValueSelected = document.querySelector(".papa");
      if (pastValueSelected) {
        pastValueSelected.classList.remove("papa");
      }
    },
    setpercentaje(event) {
      const button = event.target;
      this.percentaje = event.target.value;
      this.resetpercentajes()
      button.classList.add("papa");
    },
    customPercentaje(event) {
      const value = event.target.valueAsNumber;
      if (event.target.value != "") {
        if (!value) {
          event.target.value = 0;
        } else if (value > 99) {
          event.target.value = 99;
        } else if (value < 0) {
          event.target.value = 0;
        } else if (("" + value).length > 2) {
          event.target.value = 0;
        }
        this.setpercentaje(event);
      }
    },
  },
};
</script>
<style>
@import url("https://fonts.googleapis.com/css2?family=Space+Mono:wght@400;700&display=swap");
:root {
  --StrongCyan: hsl(172, 67%, 45%);
  --Verydarkcyan: hsl(183, 100%, 15%);
  --Darkgrayishcyan1: hsl(186, 14%, 43%);
  --Darkgrayishcyan2: hsl(184, 14%, 56%);
  --Lightgrayishcyan1: hsl(185, 41%, 84%);
  --Lightgrayishcyan2: hsl(189, 41%, 97%);
  --White: hsl(0, 0%, 100%);
}
* {
  box-sizing: border-box;
  margin: unset;
  font-family: "Space Mono", monospace;
}
html {
  font-size: 62.5%;
}
input[type="number"]::-webkit-inner-spin-button,
input[type="number"]::-webkit-outer-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
input[type="number"] {
  -moz-appearance: textfield;
}
main {
  background-color: var(--Lightgrayishcyan1);
  width: 100vw;
  height: 100vh;
  overflow: hidden;
}
.main__appContainer {
  background-color: white;
}
h2 , pre {
  color: var(--Darkgrayishcyan1);
  font-weight: 700;
  font-size: 1.5rem;
  margin-bottom: 0.8rem;
}
pre{
  color: orange;
}
.main__titleContainer {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 15%;
  width: 100%;
  text-align: right;
}
.main__titleContainer h1 {
  font-size: 2.4rem;
  width: 34%;
  max-width: 11rem;
  color: var(--Verydarkcyan);
  font-weight: 700;
  letter-spacing: 0.5em;
  word-wrap: break-word;
  text-align: justify;
}
.main__appContainer {
  height: 85%;
  border-top-left-radius: 2rem;
  border-top-right-radius: 2rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-evenly;
}
.main__appContainer > div {
  width: 85%;
}
.appContainer__BillContainer div {
  height: 4rem;
  border-radius: 0.5rem;
  margin-top: 0.5rem;
  background-color: var(--Lightgrayishcyan2);
  display: flex;
  align-items: center;
}
.appContainer__BillContainer span {
  display: inline-block;
  height: 40%;
  width: 10%;
  background-image: url("@/assets/images/icon-dollar.svg");
  background-position: 50%;
  background-size: contain;
  background-repeat: no-repeat;
  margin-left: 1rem;
}
input:not(.custom) {
  background-color: var(--Lightgrayishcyan2);
  border: none;
  outline: none;
  height: 100%;
  width: 90%;
  margin-right: 1rem;
  text-align: right;
  color: var(--Verydarkcyan);
  font-size: 2.6rem;
  font-weight: 700;
}
.xd:focus-within {
  border: solid 2px var(--StrongCyan);
}
.appContainer__percentajeContainer {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  height: 32%;
}
.appContainer__percentajeContainer div {
  margin-top: 1rem;
  display: flex;
  height: 85%;
  flex-wrap: wrap;
  justify-content: space-between;
}
.appContainer__percentajeContainer div button {
  color: white;
  font-weight: 700;
  font-size: 2.1rem;
  background-color: var(--Verydarkcyan);
  min-width: 45%;
  height: 25%;
  border-radius: 0.6rem;
  border: none;
}
.appContainer__percentajeContainer div button:hover{
  filter: brightness(120%);
}
.appContainer__percentajeContainer div .custom {
  background-color: var(--Lightgrayishcyan2);
  color: var(--Darkgrayishcyan1);
  border-radius: 0.6rem;
  min-width: 45%;
  width: 45%;
  height: 25%;
  border: none;
  outline: none;
  text-align: right;
  color: var(--Verydarkcyan);
  font-size: 2.1rem;
  font-weight: 700;
}
.appContainer__percentajeContainer div .custom::placeholder {
  text-align: center;
  color: var(--Darkgrayishcyan2);
}

.appContainer__peopleNumber div {
  height: 4rem;
  border-radius: 0.5rem;
  margin-top: 0.5rem;
  background-color: var(--Lightgrayishcyan2);
  display: flex;
  align-items: center;
}
.appContainer__peopleNumber section {
  display: flex;
  justify-content: space-between;
}
.appContainer__peopleNumber span {
  display: inline-block;
  height: 40%;
  width: 10%;
  background-image: url("@/assets/images/icon-person.svg");
  background-position: 50%;
  background-size: contain;
  background-repeat: no-repeat;
  margin-left: 1rem;
}
.appContainer__results {
  background-color: var(--Verydarkcyan);
  border-radius: 1rem;
  height: 33%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.appContainer__results .results {
  display: flex;
  justify-content: space-between;
  width: 85%;
  height: 30%;
}
.results__value {
  font-size: 2.4rem;
  overflow: hidden;
  max-width: calc(1.2em * 4);
  font-weight: 700;
}
.appContainer__results article {
  height: 100%;
  widows: 40%;
}
.appContainer__results article p:first-child {
  color: var(--Lightgrayishcyan2);
  font-size: 1.4rem;
}
.appContainer__results article p:nth-child(2) {
  color: var(--Darkgrayishcyan2);
  font-size: 1.2rem;
  font-weight: 700;
}
.appContainer__results {
  color: var(--StrongCyan);
}
.results__button {
  color: var(--Verydarkcyan);
  background-color: var(--StrongCyan);
  width: 85%;
  max-width: 28rem;
  border: none;
  font-weight: 700;
  border-radius: 0.5rem;
  height: 4.2rem;
  font-size: 1.7rem;
}
.results__button:hover {
  filter: brightness(120%);
}
.appContainer__percentajeContainer div .papa:not(input) {
  background-color: var(--StrongCyan);
  color: var(--Verydarkcyan);
}
.appContainer__percentajeContainer div .custom:focus {
  border: solid 2px var(--StrongCyan);
}
.main__appContainer .appContainer__peopleNumber .xd.danger {
  border: solid 2px orange;
}
@media (min-width:768px) {
  .main__appContainer{
    width:90%;
    height: 80%;
    margin-left: auto;
    margin-right: auto;
    border-radius: 0.8rem;
  }
  .main__appContainer > div {
    width: 75%;
  }
}
@media (min-width: 1440px){
  html{
    font-size: 80%;
  }
  .main__appContainer{
    width:70%;
    height: 65%;
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-template-areas: 
    "BILL TOTAL"
    "PERCENTAJE TOTAL"
    "PEOPLE TOTAL";
    justify-items: center;
    margin-top: 2rem;
  }
  .main__appContainer > div {
    width: 85%;
  }
  .appContainer__BillContainer{
    grid-area: BILL;
  }
  .appContainer__percentajeContainer{
    grid-area: PERCENTAJE;
    height: 100%;
    justify-content: flex-start;
  }
  .appContainer__peopleNumber{
    align-self: start;
    grid-area: PEOPLE;
  }
  .appContainer__results{
    grid-area: TOTAL;
    max-width: unset;
    height: 85%;
  }
  .appContainer__percentajeContainer div{
    height: 60%;
    margin-top: none;
  }
  .appContainer__percentajeContainer div button{
    min-width: 30%;
    height: 4rem;
  }
  .appContainer__percentajeContainer div .custom{
    min-width: 30%;
    width: 30%;
    height: 4rem;
  }
  .results__value{
    font-size: 3.5rem;
  }
}
</style>