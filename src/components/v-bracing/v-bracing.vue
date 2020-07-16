<template>
  <v-row class="mt-4">
    <div
      class="col-md-4 d-flex justify-start text-left justify-sm-center  align-content-start"
    >
      <img
        alt="wicket"
        class=" d-flex align-self-start justify-start"
        height="160px"
        src="https://calc-fense.firebaseapp.com/assets/img/bracing.jpg"
      />
    </div>
    <div class="col-md-8 d-block ">
      <v-row class="my-4">
        <v-col class="ml-2">
          Набор крепежа (фиксатор + саморез)
        </v-col>
      </v-row>
      <v-row>
        <div class="wrapper d-block">
          <label class="mb-4">
            <input
              @change="calculatingBracing()"
              class="my-5"
              max="300"
              min="0"
              step="1"
              type="range"
              v-model="valueBracing"
            />
          </label>
          <label>
            <input
              @change="calculatingBracing()"
              class="input-number"
              type="number"
              v-model="valueBracing"
            />
          </label>
          <span>Итого {{ valueBracing }} шт. </span>
          <div class="my-4 justify-start text-left">
            <p>Цена:{{ priceOneBracing }} руб./шт.</p>
            <p>Общая цена: {{ priceTotalBracing }} руб.</p>
          </div>
        </div>
      </v-row>
    </div>
  </v-row>
</template>

<script>
export default {
  name: "v-bracing",
  methods: {
    sentToParentBracing() {
      this.$emit("sentToParentEventBracing", {
        id: "bracing",
        name: "Набор крепежа (фиксатор + саморез)",
        number: this.valueBracing,
        price: this.priceTotalBracing
      });
    },

    calculatingBracing() {
      this.priceTotalBracing = this.priceOneBracing * this.valueBracing;
      if (this.priceTotalBracing > 0) {
        this.sentToParentBracing();
      }
    }
  },
  data: () => ({
    valueBracing: 0,
    priceOneBracing: 52,
    priceTotalBracing: 0
  })
};
</script>

<style scoped></style>
