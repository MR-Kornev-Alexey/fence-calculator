<template>
  <v-row class="mt-4">
    <div
      class="col-md-4 d-flex justify-start text-left justify-sm-center  align-content-start"
    >
      <img
        alt="wicket"
        class=" d-flex align-self-start justify-start"
        height="160px"
        src="https://calc-fense.firebaseapp.com/assets/img/top.jpg"
      />
    </div>
    <div class="col-md-8 d-block ">
      <v-row class="my-4">
        <v-col class="ml-2">
          Наконечник Y- образный
        </v-col>
      </v-row>

      <v-row>
        <div class="wrapper d-block">
          <label class="mb-4">
            <input
              @change="calculatingTop()"
              class="my-5"
              max="100"
              min="0"
              step="1"
              type="range"
              v-model="valueTop"
            />
          </label>
          <label>
            <input
              @change="calculatingTop()"
              class="input-number"
              type="number"
              v-model="valueTop"
            />
          </label>
          <span>Итого {{ valueTop }} шт. </span>
          <div class="my-4 justify-start text-left">
            <p>Цена:{{ priceOneTop }} руб./шт.</p>
            <p>Общая цена: {{ priceTotalTop }} руб.</p>
          </div>
        </div>
      </v-row>
    </div>
  </v-row>
</template>

<script>
export default {
  name: "v-top",
  methods: {
    sentToParentTop() {
      this.$emit("sentToParentEventTop", {
        id: "topY",
        name: "Наконечник Y - образный",
        number: this.valueTop,
        price: this.priceTotalTop
      });
    },

    calculatingTop() {
      this.priceTotalTop = this.priceOneTop * this.valueTop;
      if (this.priceTotalTop > 0) {
        this.sentToParentTop();
      }
    }
  },
  data: () => ({
    valueTop: 0,
    priceOneTop: 450,
    priceTotalTop: 0
  })
};
</script>

<style scoped></style>
