<template>
  <v-row class="mt-4">
    <div class="col-md-4 d-flex justify-start text-left align-content-start">
      <img
        class=" d-flex align-self-start justify-start"
        src="assets/img/bracing.jpg"
        height="160px"
        alt="wicket"
      />
    </div>
    <div class="col-md-8 d-block ">
      <v-row class="my-4">
        Набор крепежа (фиксатор + саморез)
      </v-row>

      <v-row>
        <div class="wrapper d-block">
          <label class="mb-4">
            <input
              type="range"
              min="0"
              max="300"
              step="1"
              v-model="valueBracing"
              @change="calculatingBracing()"
            />
          </label>
          <label>
            <input
              class="input-number"
              type="number"
              v-model="valueBracing"
              @change="calculatingBracing()"
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
