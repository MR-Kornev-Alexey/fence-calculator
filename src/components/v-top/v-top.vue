<template>
  <v-row class="mt-4">
    <div class="col-md-4 d-flex justify-start text-left align-content-start">
      <img
        class=" d-flex align-self-start justify-start"
        src="assets/img/top.jpg"
        height="160px"
        alt="wicket"
      />
    </div>
    <div class="col-md-8 d-block ">
      <v-row class="my-4">
        Наконечник Y- образный
      </v-row>

      <v-row>
        <div class="wrapper d-block">
          <label class="mb-4">
            <input
              type="range"
              min="0"
              max="100"
              step="1"
              v-model="valueTop"
              @change="calculatingTop()"
            />
          </label>
          <label>
            <input
              class="input-number"
              type="number"
              v-model="valueTop"
              @change="calculatingTop()"
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
