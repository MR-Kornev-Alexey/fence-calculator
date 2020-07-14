<template>
  <v-row class="mt-4">
    <div class="col-md-5 d-flex justify-start text-left align-content-start">
      <img
        class=" d-flex align-self-start justify-start"
        src="assets/img/vorota.png"
        height="160px"
        alt=""
      />
    </div>
    <div class="col-md-7 d-block ">
      <v-row class="justify-space-around mb-4">
        <button
          :class="[
            { 'btn-items-choice_selected': flagGate },
            { 'btn-items-choice': !flagGate }
          ]"
          @click="changeOpenGate()"
        >
          Распашные
        </button>
        <button
          :class="[
            { 'btn-items-choice_selected': !flagGate },
            { 'btn-items-choice': flagGate }
          ]"
          @click="changeRollGate()"
        >
          Откатные
        </button>
      </v-row>
      <v-row>
        Размер ворот ( высота x ширина )
      </v-row>
      <v-row class="justify-space-between " v-if="flagGate">
        <button
          v-for="item in btnLengthOpenGate"
          :key="item.id"
          :class="[
            { 'btn-items-size_selected': item.selected },
            { 'btn-items-size': !item.selected }
          ]"
          @click="changeLengthOpenGate(item.id)"
        >
          {{ item.name }}
        </button>
      </v-row>
      <v-row class="justify-space-between" v-if="!flagGate">
        <button
          v-for="item in btnLengthRollGate"
          :key="item.id"
          :class="[
            { 'btn-items-size_selected': item.selected },
            { 'btn-items-size': !item.selected }
          ]"
          @click="changeLengthRollGate(item.id)"
        >
          {{ item.name }}
        </button>
      </v-row>
      <v-row>
        <div class="wrapper d-block">
          <label class="mb-4">
            <input
              type="range"
              min="0"
              max="100"
              step="1"
              v-model="valueGate"
              @change="calculatingGate()"
            />
          </label>
          <label>
            <input
              class="input-number"
              type="number"
              v-model="valueGate"
              @change="calculatingGate()"
            />
          </label>
          <span>Итого {{ valueGate }} ворот </span>

          <div class="my-4 justify-start text-left">
            <p>Цена:{{ priceOneGate }} руб./ворота</p>
            <p>Общая цена: {{ priceTotalGate }} руб.</p>
          </div>
        </div>
      </v-row>
    </div>
  </v-row>
</template>

<script>
export default {
  name: "v-gate",
  methods: {
    sentToParentGate() {
      this.$emit("sentToParentEventGate", {
        id: this.flagMoveGate + this.flagLengthGate,
        name: this.flagGateName,
        size: this.flagLength,
        number: this.valueGate,
        price: this.priceTotalGate
      });
    },

    clearAllResult() {
      this.priceTotalGate = this.priceOneGate = this.valueGate = 0;
    },
    calculatingGate() {
      this.priceTotalGate = this.priceOneGate * this.valueGate;
      if (this.priceTotalGate > 0) {
        this.sentToParentGate();
      }
    },
    changeRollGate() {
      this.flagGate = false;
      this.flagMoveGate = "ROLL";
      this.flagGateName = "Ворота откатные Классик с аксессуарами";
      this.clearAllResult();
    },
    changeOpenGate() {
      this.flagGate = true;
      this.flagMoveGate = "OPEN";
      this.flagGateName =
        "Ворота распашные Классик (каркас 60*40 | ZN | сетка Ø.4 | столб 80*80 | ушки под подвесной замок";
      this.clearAllResult();
    },
    clearAllButton(array) {
      for (let i = 0; i < array.length; i++) {
        array[i].selected = false;
      }
    },
    changeLengthRollGate(item) {
      this.clearAllButton(this.btnLengthRollGate);
      this.flagLengthGate = this.btnLengthRollGate[item].innId;
      this.btnLengthRollGate[item].selected = true;
      this.priceOneGate = this.btnLengthRollGate[item].price;
      this.flagLength = this.btnLengthRollGate[item].name;
      this.calculatingGate();
    },
    changeLengthOpenGate(item) {
      this.clearAllButton(this.btnLengthOpenGate);
      this.flagLengthGate = this.btnLengthOpenGate[item].innId;
      this.btnLengthOpenGate[item].selected = true;
      this.priceOneGate = this.btnLengthOpenGate[item].price;
      this.flagLength = this.btnLengthOpenGate[item].name;
      this.calculatingGate();
    }
  },
  data: () => ({
    flagMoveGate: "",
    flagLength: "",
    valueGate: 0,
    flagGateName: "",
    flagGate: true,
    flagLengthGate: "",
    priceOneGate: 0,
    priceTotalGate: 0,
    btnLengthRollGate: [
      {
        id: 0,
        innID: "roll2.03m",
        name: "2,03 м. х 4 м.",
        selected: true,
        price: 58320
      },
      {
        id: 1,
        innID: "roll2.43m",
        name: "2,43 м. х 4 м.",
        selected: false,
        price: 62440
      }
    ],
    btnLengthOpenGate: [
      {
        id: 0,
        innID: "open1.53m",
        name: "1,53 м. х 4 м.",
        selected: true,
        price: 30350
      },
      {
        id: 1,
        innID: "open1.73m",
        name: "1,73 м. х 4 м.",
        selected: false,
        price: 30950
      },
      {
        id: 2,
        innID: "open2.03m",
        name: "2,03 м. х 4 м.",
        selected: false,
        price: 33700
      },
      {
        id: 3,
        innID: "open2.43m",
        name: "2,43 м. х 4 м.",
        selected: false,
        price: 42300
      }
    ]
  })
};
</script>

<style scoped></style>
