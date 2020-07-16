<template>
  <v-row class="mt-4">
    <div
      class="col-md-4 d-flex justify-start text-left justify-sm-center  align-content-start"
    >
      <img
        class=" d-flex align-self-start justify-start"
        src="https://calc-fense.firebaseapp.com/assets/img/wicket.png"
        height="160px"
        alt="wicket"
      />
    </div>
    <div class="col-md-8 d-block ">
      <v-row>
        <v-col class="ml-2">
          Размер калитки ( высота x ширина )
        </v-col>
      </v-row>
      <v-row class="justify-space-around mb-4 mt-4">
        <button
          v-for="item in btnLengthWicket"
          :key="item.id"
          :class="[
            { 'btn-items-size_selected': item.selected },
            { 'btn-items-size': !item.selected }
          ]"
          @click="changeLengthWicket(item.id)"
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
              v-model="valueWicket"
              @change="calculatingWicket()"
            />
          </label>
          <label>
            <input
              class="input-number"
              type="number"
              v-model="valueWicket"
              @change="calculatingWicket()"
            />
          </label>
          <span>Итого {{ valueWicket }} калиток </span>
          <div class="my-4 justify-start text-left">
            <p>Цена:{{ priceOneWicket }} руб./калитку</p>
            <p>Общая цена: {{ priceTotalWicket }} руб.</p>
          </div>
        </div>
      </v-row>
    </div>
  </v-row>
</template>

<script>
export default {
  name: "v-wicket",
  methods: {
    clearAllButton(array) {
      for (let i = 0; i < array.length; i++) {
        array[i].selected = false;
      }
    },
    calculatingWicket() {
      this.priceTotalWicket = this.priceOneWicket * this.valueWicket;
      if (this.priceTotalWicket > 0) {
        this.sentToParentWicket();
      }
    },
    changeLengthWicket(item) {
      this.clearAllButton(this.btnLengthWicket);
      this.flagLengthWicket = this.btnLengthWicket[item].innId;
      this.btnLengthWicket[item].selected = true;
      this.priceOneWicket = this.btnLengthWicket[item].price;
      this.flagLength = this.btnLengthWicket[item].name;
      this.calculatingWicket();
    },

    sentToParentWicket() {
      this.$emit("sentToParentEventWicket", {
        id: "wicket" + this.flagLengthWicket,
        name: this.flagWicketName,
        size: this.flagLength,
        number: this.valueWicket,
        price: this.priceTotalWicket
      });
    }
  },
  data: () => ({
    flagLengthWicket: "",
    valueWicket: 0,
    flagWicketName:
      "Калитка Классик (каркас 60*40 ZN, сетка Ø 4, столб 60*60, ручка-замок)",
    priceOneWicket: 11500,
    priceTotalWicket: 0,
    btnLengthWicket: [
      {
        id: 0,
        innID: "1.53m",
        name: "1,53 м. х 1 м.",
        selected: true,
        price: 11500
      },
      {
        id: 1,
        innID: "1.73m",
        name: "1,73 м. х 1 м.",
        selected: false,
        price: 12100
      },
      {
        id: 2,
        innID: "2.03m",
        name: "2,03 м. х 1 м.",
        selected: false,
        price: 1287
      },
      {
        id: 3,
        innID: "2.43m",
        name: "2,43 м. х 1 м.",
        selected: false,
        price: 13970
      }
    ]
  })
};
</script>

<style scoped></style>
