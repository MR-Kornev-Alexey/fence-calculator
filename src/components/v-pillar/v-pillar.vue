<template>
  <v-row class="mt-4">
    <div class="col-md-4 d-flex justify-start text-left align-content-start">
      <img
        v-if="flagPillar"
        class=" d-flex align-self-start justify-start"
        src="assets/img/pillar-colored.png"
        height="160px"
        alt=""
      />
      <img
        v-if="!flagPillar"
        class=" d-flex align-self-start justify-start"
        src="assets/img/pillar-galv.png"
        height="160px"
        alt=""
      />
    </div>
    <div class="col-md-8 d-block">
      <v-row class="justify-space-around mb-4">
        <button
          :class="[
            { 'btn-items-choice_selected': flagPillar },
            { 'btn-items-choice': !flagPillar }
          ]"
          @click="changeColorPillar()"
        >
          Полимерный
        </button>
        <button
          :class="[
            { 'btn-items-choice_selected': !flagPillar },
            { 'btn-items-choice': flagPillar }
          ]"
          @click="changeGalvanicPillar()"
        >
          Оцинкованый
        </button>
      </v-row>
      <v-row class="justify-space-around d-block mb-4">
        <v-row>
          Длина столба
        </v-row>
        <v-row class="justify-space-between ">
          <button
            v-for="item in btnLengthPillar"
            :key="item.id"
            :class="[
              { 'btn-items-size_selected': item.selected },
              { 'btn-items-size': !item.selected }
            ]"
            @click="changeLengthPillar(item.id)"
          >
            {{ item.name }}
          </button>
        </v-row>
      </v-row>
      <v-row v-if="error">
        <span class="error">Такой столб не изготавливается! </span>
      </v-row>
      <v-row>
        <div class="wrapper d-block">
          <label class="mb-4">
            <input
              type="range"
              min="0"
              max="100"
              step="1"
              v-model="valuePillar"
              @change="calculatingPillar()"
            />
          </label>
          <label>
            <input
              class="input-number"
              type="number"
              v-model="valuePillar"
              @change="calculatingPillar()"
            />
          </label>
          <span>Итого {{ valuePillar }} столбов </span>

          <div class="my-4 justify-start text-left">
            <p>Цена:{{ onePillar }} руб./столб</p>
            <p>Общая цена: {{ resultPillar }} руб.</p>
          </div>
        </div>
      </v-row>
      <!--    конец расчетного блока -->
    </div>
  </v-row>
</template>

<script>
export default {
  name: "v-section",
  props: {
    estimatePillar: {
      type: Object,
      default() {
        return {};
      }
    }
  },
  data: () => ({
    error: false,
    onePillar: 0,
    resultPillar: 0,
    totalPillar: 0,
    valuePillar: 0,
    flagPillar: true,
    flagLengthPillar: "",
    flagPillarName: "",
    flagColor: "",
    pricePillar: {
      "2000": {
        ZN: 790,
        RAL: 830
      },
      "2200": {
        ZN: 850,
        RAL: 885
      },
      "2500": {
        ZN: 949,
        RAL: 955
      },
      "3000": {
        ZN: 1080,
        RAL: 1090
      },
      f1500: {
        ZN: 0,
        RAL: 1114
      },
      f1700: {
        ZN: 0,
        RAL: 1215
      },
      f2000: {
        ZN: 0,
        RAL: 1397
      }
    },

    btnLengthPillar: [
      { id: 0, innId: "2000", name: "2 м.", selected: false },
      { id: 1, innId: "2200", name: "2,2 м.", selected: false },
      { id: 2, innId: "2500", name: "2,5 м.", selected: false },
      { id: 3, innId: "3000", name: "3 м.", selected: false },
      { id: 4, innId: "f1500", name: "1,5 м.", selected: false },
      { id: 5, innId: "f1700", name: "1,7 м.", selected: false },
      { id: 6, innId: "f2000", name: "2,0 м.", selected: false }
    ]
  }),
  methods: {
    calcTotalPillar(type) {
      this.error = false
      if (this.flagLengthPillar) {
        this.onePillar = this.pricePillar[this.flagLengthPillar][type];
        if(type==="ZN"&& this.onePillar === 0 ){
          this.error = true
          return "";
        }
        this.resultPillar = this.onePillar * this.valuePillar;
        this.flagColor = type;
        if (this.resultPillar > 0) {
          this.sentToParent();
        }
      } else {
        return "";
      }
    },
    sentToParent() {
      this.$emit("sentToParentEvent", {
        id: this.flagColor + this.flagLengthPillar,
        name: this.flagPillarName,
        size: this.flagLengthPillar,
        number: this.valuePillar,
        price: this.resultPillar
      });
    },
    calculatingPillar() {
      if (this.flagPillar) {
        this.calcTotalPillar("RAL");
      } else {
        this.calcTotalPillar("ZN");
      }
    },
    clearAllButtonPillar(array) {
      for (let i = 0; i < array.length; i++) {
        array[i].selected = false;
      }
    },
    changeColorPillar() {
      this.flagPillar = true;
      this.flagPillarName = "Полимерный столб";
      this.calcTotalPillar("RAL");
      this.calculatingPillar();
    },
    changeGalvanicPillar() {
      this.flagPillar = false;
      this.flagPillarName = "Оцинкованый столб";
      this.calcTotalPillar("ZN");
      this.calculatingPillar();
    },
    changeLengthPillar(item) {
      this.clearAllButtonPillar(this.btnLengthPillar);
      this.flagLengthPillar = this.btnLengthPillar[item].innId;
      this.btnLengthPillar[item].selected = true;
      this.calculatingPillar();
    }
  }
};
</script>

<style scoped>
  .error {
    color: darkred;
  }
</style>
