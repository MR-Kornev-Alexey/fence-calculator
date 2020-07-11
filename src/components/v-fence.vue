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
              <v-row  v-if="flagSection" class="justify-space-between ">
                <button
                        v-for="item in btnBarThicknessPaint"
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

              <v-row  v-if="!flagSection" class="justify-space-between ">
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
                <span>Итого {{ total }} секций {{totalLength}} м.</span>

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
        <table class="table-out " id="customers" v-if="tableEstimate">
          <tr v-for="item in outEstimate" :key="item.id">
            <td class="td-icon"><v-icon @click="itemDelete(item.id)">mdi-close</v-icon></td>
            <td class="td-name"> {{ item.name }} {{ item.size }} |  {{ item.thickness }} мм</td>
            <td class="td-number">{{ item.number }} шт.</td>
            <td class="td-number">{{ item.price }} руб.</td>
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
    outEstimate:{},
    tableEstimate: true,
    oneLength:0,
    totalLength:0,
    value: 0,
    oneSection: 0,
    titlePaint:"titlePaint",
    titleGalvanic:"titleGalvanic",
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
    pricePaint: {
      "1030x2500": {
        "3,2": 910,
        "3,7": 981.5,
        "3,2/4": 998,
        "4": 1180,
        "4,2": 1183,
        "4,7": 1397.5,
        "5": 1640,
        "5,2": 1677
      },
      "1530x2500": {
        "3,2": 1110,
        "3,7": 1443,
        "3,2/4": 1349,
        "4": 1625,
        "4,2": 1742,
        "4,7": 2060.5,
        "5": 2278,
        "5,2": 2470
      },
      "1730x2500": {
        "3,2": 1320,
        "3,7": 1605.5,
        "3,2/4": 1480,
        "4": 1799,
        "4,2": 1937,
        "4,7": 2294.5,
        "5": 2530,
        "5,2": 2749.5
      },
      "2030x2500": {
        "3,2": 1443,
        "3,7": 1911,
        "3,2/4": 1810,
        "4": 2070,
        "4,2": 2301,
        "4,7": 2723.5,
        "5": 2996,
        "5,2": 3477.5
      },
      "2430x2500": {
        "3,2": 1822,
        "3,7": 2229.5,
        "3,2/4": 2015,
        "4": 2509,
        "4,2": 2691,
        "4,7": 3185,
        "5": 3529,
        "5,2": 3770
      }
    },
    sizeSection: [
      { id: 1, innId:"1030x2500", name: "1,03 м x 2,5 м", length:1030, selected: false },
      { id: 2, innId:"1530x2500",name: "1,53 м x 2,5 м", length:1530, selected: false },
      { id: 3,innId:"1730x2500", name: "1,73 м x 2,5 м",  length:1730,selected: false },
      { id: 4, innId:"2030x2500", name: "2,03 м x 2,5 м", length:2030, selected: false },
      { id: 5,innId:"2430x2500", name: "2,4 м x 2,5 м", length:2430, selected: false }
    ],
    btnBarThicknessPaint: [
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
        name: "3,2/4 мм",
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
    itemDelete(item){
      this.tableEstimate = false
      delete this.outEstimate[item]
      this.tableEstimate = true
    },
    calculating() {
      this.tableEstimate = false
      if (!this.flagSection) {
        this.totalLength = this.oneLength*this.value/1000
        let result = this.priceGalvanic[this.flagSizeSection]
             result = result[this.flagBarThickness]
        this.oneSection = result
        this.resultSection = this.value*result
        this.outEstimate[this.titleGalvanic+this.flagSizeSection] =
                { id:this.titleGalvanic+this.flagSizeSection,
                  name: "Оцинкованая секция",
                  size: this.flagSizeSection,
                  thickness:this.flagBarThickness,
                  number: this.value,
                  price: this.resultSection
                }
        this.tableEstimate = true
      } else {
        this.totalLength = this.oneLength*this.value/1000
        let result = this.pricePaint[this.flagSizeSection]
        result = result[this.flagBarThickness]
        this.oneSection = result
        this.resultSection = this.value*result
        this.outEstimate[this.titlePaint+this.flagSizeSection] =
                { id:this.titlePaint+this.flagSizeSection,
                  name: "Полимерная секция",
                  size: this.flagSizeSection,
                  thickness:this.flagBarThickness,
                  number: this.value,
                  price: this.resultSection
                }
        this.tableEstimate = true
      }
    },
    clearAllSection(){
      this.resultSection = 0
      this.flagBarThickness = ""
      this.clearAllButton(this.btnBarThickness)
      this.clearAllButton(this.btnBarThicknessPaint);
    },
    changeGalvanic() {
      this.clearAllSection()
      return (this.flagSection = false);
    },
    changeColor() {
      this.clearAllSection()
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
      this.oneLength = this.sizeSection[sizeBar - 1].length;
      if(this.btnBarThickness){
        this.calculating()
      }

      return (this.sizeSection[sizeBar - 1].selected = true);
    },
    changeBarThickness(sizeBar) {
      if(!this.flagSection){
        this.clearAllButton(this.btnBarThickness);
        this.flagBarThickness = this.btnBarThickness[sizeBar - 1].innId;
        if(this.flagSizeSection){
          this.calculating()
        }
        return (this.btnBarThickness[sizeBar - 1].selected = true);
      }else {
        this.clearAllButton(this.btnBarThicknessPaint);
        this.flagBarThickness = this.btnBarThicknessPaint[sizeBar - 1].innId;
        if(this.flagSizeSection){
          this.calculating()
        }
        return (this.btnBarThicknessPaint[sizeBar - 1].selected = true);
      }
    }
  }
};
</script>
<style lang="scss">
  .td-name{
    font-size: 12px;
    width: 44%;

  }
  .td-icon{
    width: 6%;
    cursor: pointer;
  }
  .td-number{
    width: 25%;
  }
.table-out {
  width: 100%;

  tr td {
    padding: 8px 0;
    border-bottom: 1px solid #5cb071;
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
