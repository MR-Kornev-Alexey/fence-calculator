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
      <!--    конец расчетного блока -->
    </div>
  </v-row>
</template>

<script>
export default {
  name: "v-section",
  data: () => ({
    flagPillar: true,
    flagLengthPillar: "",
    btnLengthPillar: [
      { id: 1, innId: "2m", name: "2 м.", selected: false },
      { id: 2, innId: "2.2m", name: "2,2 м.", selected: false },
      { id: 3, innId: "2.5m", name: "2,5 м.", selected: false },
      { id: 4, innId: "3m", name: "3 м.", selected: false },
      { id: 5, innId: "f1.5m", name: "1,5 м.", selected: false },
      { id: 6, innId: "f1.7m", name: "1,7 м.", selected: false },
      { id: 7, innId: "f2m", name: "2,0 м.", selected: false }
    ]
  }),
  methods: {
    clearAllButtonPillar(array) {
      for (let i = 0; i < array.length; i++) {
        array[i].selected = false;
      }},
    changeColorPillar() {
      return (this.flagPillar = true);
    },
    changeGalvanicPillar() {
      return (this.flagPillar = false);
    },
    changeLengthPillar(item) {
      this.clearAllButtonPillar(this.btnLengthPillar);
      this.flagLengthPillar = this.btnLengthPillar[item - 1].innId;
      this.btnLengthPillar[item - 1].selected = true;
    }
  }
};
</script>

<style scoped></style>
