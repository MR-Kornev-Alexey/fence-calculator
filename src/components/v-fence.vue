<template>
  <v-container>
    <v-row>
      <h1 class="mt-4">Расчет забора</h1>
    </v-row>
    <v-row class="text-center justify-start mt-4">
      <v-col class="justify-start d-block" cols="12" md="8">
        <v-row class="justify-start">
          <div v-for="item in commodityItems" :key="item.id">
            <button
              :class="[
                { 'btn-items_selected ': item.selected },
                { 'btn-items ': !item.selected }
              ]"
            >
              {{ item.name }}
            </button>
          </div>
        </v-row>
        <v-row class="mt-4">
          <div
            class="col-md-4 d-flex justify-start text-left align-content-start"
          >
            <img
              v-if="flagSection"
              class=" d-flex align-self-start justify-start"
              src="assets/img/section-colored.png"
              height="160px"
              alt=""
            />
            <img
              v-if="!flagSection"
              class=" d-flex align-self-start justify-start"
              src="assets/img/section-galv.png"
              height="160px"
              alt=""
            />
          </div>
          <div class="col-md-8 d-block">
            <v-row class="justify-space-around mb-4">
              <button
                :class="[
                  { 'btn-items-choice_selected': flagSection },
                  { 'btn-items-choice': !flagSection }
                ]"
                @click="changeColor()"
              >
                Полимерная
              </button>
              <button
                :class="[
                  { 'btn-items-choice_selected': !flagSection },
                  { 'btn-items-choice': flagSection }
                ]"
                @click="changeGalvanic()"
              >
                Оцинкованая
              </button>
            </v-row>
            <v-row class="justify-space-around d-block mb-4">
              <v-row>
                Толщина прутка
              </v-row>
              <v-row  v-if="!flagSection" class="justify-space-between ">
                <button
                        v-for="item in btnBarThicknessPain"
                        :key="item.id"
                        :class="[
                    { 'btn-items-size_selected': item.selected },
                    { 'btn-items-size': !item.selected }
                  ]"
                        @click="changeBarThickness(item.id)"
                >
                  {{ item.name }}
                </button>
              </v-row>

              <v-row  v-if="flagSection" class="justify-space-between ">
                <button
                  v-for="item in btnBarThickness"
                  :key="item.id"
                  :class="[
                    { 'btn-items-size_selected': item.selected },
                    { 'btn-items-size': !item.selected }
                  ]"
                  @click="changeBarThickness(item.id)"
                >
                  {{ item.name }}
                </button>
              </v-row>
            </v-row>
            <v-row class="justify-space-around mb-4">
              <v-row>
                Размер секции ( высота x ширина )
              </v-row>
              <v-row class="justify-space-around">
                <button
                  v-for="item in sizeSection"
                  :key="item.id"
                  :class="[
                    { 'btn-items-section_selected': item.selected },
                    { 'btn-items-section': !item.selected }
                  ]"
                  @click="changeSizeSection(item.id)"
                >
                  {{ item.name }}
                </button>
              </v-row>
            </v-row>
            <v-row>
              <div class="wrapper d-block">
                <label class="mb-4">
                  <input
                    type="range"
                    min="0"
                    max="100"
                    step="1"
                    v-model="value"
                    @change="calculating(value)"
                  />
                </label>
                <label>
                  <input class="input-number" type="number" v-model="value"   @change="calculating()"/>
                </label>
                <span>Вы выбрали {{ total }} секций </span>

                <div class="my-4 justify-start text-left">
                  <p>Цена:{{ oneSection }} руб./секция</p>
                  <p>Общая цена: {{ resultSection }} руб.</p>
                </div>
              </div>
            </v-row>
            <v-row>
              <button class="btn-items_selected">
                Далее
              </button>
            </v-row>
          </div>
        </v-row>
      </v-col>
      <v-col cols="12" md="4" class="justify-center align-center">
        <v-flex class="choice-title ">
          Вы выбрали:
        </v-flex>
        <table class="table-out " id="customers">
          <tr v-for="item in commodityItems" :key="item.id">
            <td>{{ item.name }}</td>
            <td>шт.</td>
            <td>руб.</td>
          </tr>
        </table>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: "v-fence",

  data: () => ({
    value: 0,
    oneSection: 0,
    // если выбрано цвет то правда , если галваник то ложь
    flagSection: true,
    // толщина прутка
    resultSection: 0,
    flagBarThickness: "",
    flagSizeSection: "",
    priceGalvanic: {
      "1030x2500": {
        "3": 676,
        "3,5": 852,
        "3/3,8": 850,
        "3,8": 1090,
        "4,0": 988,
        "4,5": 1164,
        "4,8": 1450,
        "5,0": 1495
      },
      "1530x2500": {
        "3": 995,
        "3,5": 1255,
        "3/3,8": 1150,
        "3,8": 1490,
        "4,0": 1443,
        "4,5": 1710,
        "4,8": 2110,
        "5,0": 2171
      },
      "1730x2500": {
        "3": 1086,
        "3,5": 1372,
        "3/3,8": 1240,
        "3,8": 1610,
        "4,0": 1586,
        "4,5": 1872,
        "4,8": 2350,
        "5,0": 2386
      },
      "2030x2500": {
        "3": 1313,
        "3,5": 1651,
        "3/3,8": 1420,
        "3,8": 1788,
        "4,0": 1905,
        "4,5": 2249,
        "4,8": 2550,
        "5,0": 2867
      },
      "2430x2500": {
        "3": 1495,
        "3,5": 1885,
        "3/3,8": 1710,
        "3,8": 2290,
        "4,0": 2184,
        "4,5": 2587,
        "4,8": 3250,
        "5,0": 3296
      }
    },
    sizeSection: [
      { id: 1, innId:"1030x2500", name: "1,03 м x 2,5 м",  selected: false },
      { id: 2, innId:"1530x2500",name: "1,53 м x 2,5 м",  selected: false },
      { id: 3,innId:"1730x2500", name: "1,73 м x 2,5 м",  selected: false },
      { id: 4, innId:"2030x2500", name: "2,03 м x 2,5 м",  selected: false },
      { id: 5,innId:"2430x2500", name: "2,4 м x 2,5 м",  selected: false }
    ],
    btnBarThicknessPain: [
      {
        id: 1,
        innId:"3,2",
        name: "3,2 мм",
        selected: false
      },
      {
        id: 2,
        innId:"3,7",
        name: "3,7 мм",
        selected: false
      },
      {
        id: 3,
        innId:"3,2/4",
        name: "3,2/8 мм",
        selected: false
      },
      {
        id: 4,
        innId:"4",
        name: "4 мм",
        selected: false
      },
      {
        id: 5,
        innId:"4,2",
        name: "4,2 мм",
        selected: false
      },
      {
        id: 6,
        innId:"4,7",
        name: "4,7 мм",
        selected: false
      },
      {
        id: 7,
        innId:"5",
        name: "5 мм",
        selected: false
      },
      {
        id: 8,
        innId:"5,2",
        name: "5,2 мм",
        selected: false
      }
    ],
    btnBarThickness: [
      {
        id: 1,
        innId:"3",
        name: "3 мм",
        section: "",
        selected: false
      },
      {
        id: 2,
        innId:"3,5",
        name: "3,5 мм",
        section: "",
        selected: false
      },
      {
        id: 3,
        innId:"3/3,8",
        name: "3-3,8 мм",
        section: "",
        selected: false
      },
      {
        id: 4,
        innId:"3,8",
        name: "3,8 мм",
        section: "",
        selected: false
      },
      {
        id: 5,
        innId:"4,0",
        name: "4,0 мм",
        section: "",
        selected: false
      },
      {
        id: 6,
        innId:"4,5",
        name: "4,5 мм",
        section: "",
        selected: false
      },
      {
        id: 7,
        innId:"4,8",
        name: "4,8 мм",
        section: "",
        selected: false
      },
      {
        id: 8,
        innId:"5,0",
        name: "5 мм",
        section: "",
        selected: false
      }
    ],
    commodityItems: [
      {
        id: 1,
        name: "Секция",
        vModel: "section",
        section: "",
        selected: true
      },
      {
        id: 2,
        name: "Столб",
        vModel: "pillar",
        pillar: "",
        selected: false
      },
      {
        id: 3,
        name: "Ворота",
        vModel: "gate",
        gate: "",
        selected: false
      },
      {
        id: 4,
        name: "Калитка",
        vModel: "wicket",
        wicket: "",
        selected: false
      },
      {
        id: 5,
        name: "Крепления",
        vModel: "binding",
        binding: "",
        selected: false
      },
      {
        id: 6,
        name: "Навершина",
        vModel: "top",
        top: "",
        selected: false
      },
      {
        id: 7,
        name: "Егоза",
        vModel: "wire",
        wire: "",
        selected: false
      }
    ]
  }),
  computed: {
    total: function() {
      return this.value;
    }
  },
  methods: {
    calculating() {
      if (this.flagSection) {
        let result = this.priceGalvanic[this.flagSizeSection]
             result = result[this.flagBarThickness]
        this.oneSection = result
        this.resultSection = this.value*result
      } else {
        this.resultSection = this.value*500
      }
    },
    changeGalvanic() {
       return (this.flagSection = false);
    },
    changeColor() {
      this.calculating()
      return (this.flagSection = true);
    },
    clearAllButton(array) {
        for (let i = 0; i < array.length; i++) {
        array[i].selected = false;
      }
    },
    changeSizeSection(sizeBar) {
      this.clearAllButton(this.sizeSection);
      this.flagSizeSection = this.sizeSection[sizeBar - 1].innId;
      if(this.btnBarThickness){
        this.calculating()
      }

      return (this.sizeSection[sizeBar - 1].selected = true);
    },
    changeBarThickness(sizeBar) {
      this.clearAllButton(this.btnBarThickness);
     this.flagBarThickness = this.btnBarThickness[sizeBar - 1].innId;
      if(this.flagSizeSection){
        this.calculating()
      }
      return (this.btnBarThickness[sizeBar - 1].selected = true);
    }
  }
};
</script>
<style lang="scss">
.table-out {
  width: 100%;
  tr td {
    padding: 8px 0;
    border-bottom: 1px solid #5cb071;
    width: 33%;
    text-align: center;
  }
}
.btn-items_selected {
  background-color: #5cb071;
  padding: 6px;
  width: 110px;
  text-transform: uppercase;
  color: white;
  text-align: center;
  margin: 0 1px;
}
.btn-items {
  background-color: white;
  padding: 6px;
  width: 110px;
  text-transform: uppercase;
  color: #5cb071;
  text-align: center;
  margin: 0 1px;
}
.btn-items:hover {
  background-color: white;
  color: #5cb071;
  padding: 5px;
  border: 1px solid #5cb071;
}
.choice-title {
  background-color: #5cb071;
  text-transform: uppercase;
  color: white;
  height: 36px;
  padding-top: 6px;
}
.btn-items-choice_selected {
  background-color: #5cb071;
  font-size: 12px;
  color: white;
  padding: 6px;
  width: 140px;
  text-transform: uppercase;
  border-radius: 5px;
}
.btn-items-choice {
  background-color: white;
  font-size: 12px;
  color: #5cb071;
  padding: 4px;
  width: 140px;
  text-transform: uppercase;
  border: 2px solid #5cb071;
  border-radius: 5px;
}
.btn-items-choice:hover,
.btn-items-size:hover,
.btn-items-section:hover {
  background-color: #5cb071;
  color: white;
}
.btn-items-size_selected {
  margin: 6px 18px;
  background-color: #5cb071;
  font-size: 12px;
  color: white;
  padding: 6px;
  width: 90px;
  text-transform: uppercase;
  border-radius: 5px;
}
.btn-items-size {
  margin: 6px 18px;
  background-color: white;
  font-size: 12px;
  color: #5cb071;
  padding: 4px;
  width: 90px;
  text-transform: uppercase;
  border: 2px solid #5cb071;
  border-radius: 5px;
}
.btn-items-section {
  background-color: white;
  font-size: 12px;
  color: #5cb071;
  padding: 4px;
  width: 120px;
  text-transform: uppercase;
  border: 2px solid #5cb071;
  border-radius: 5px;
  margin: 6px 0;
}
.btn-items-section_selected {
  background-color: #5cb071;
  font-size: 12px;
  color: white;
  padding: 6px;
  width: 120px;
  text-transform: uppercase;
  border-radius: 5px;
  margin: 6px 0;
}
.wrapper {
  label {
    margin: 20px 20px;
  }
  span {
    margin: 20px 20px;
  }
  .input-number {
    width: 50px;
  }
}
</style>
