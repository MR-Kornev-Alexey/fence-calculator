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
                Крашеная
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
              <v-row class="justify-space-around">
                <button v-for="item in btnBarThickness" :key="item.id"
                        :class="[
                                { 'btn-items-size_selected': item.selected },
                                { 'btn-items-size': !item.selected }
              ]"
                        @click="changeBarThickness(item.id)"
                  >
                    {{item.name}}
                </button>
              </v-row>
            </v-row>
            <v-row class="justify-space-around mb-4">
              <v-row>
                Размер секции ( высота x ширина )
              </v-row>
              <v-row class="justify-space-around">
                  <button v-for="item in sizeSection" :key="item.id"
                          :class="[
                                { 'btn-items-section_selected': item.selected },
                                { 'btn-items-section': !item.selected }
              ]"
                          @click="changeSizeSection(item.id)"
                  >
                      {{item.name}}
                  </button>
              </v-row>
              <v-row>
                <button class="btn-items_selected">
                  Далее
                </button>
              </v-row>
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
  name: "HelloWorld",

  data: () => ({
      // если выбрано цвет то правда , если галваник то ложь
    flagSection: true,
   // толщина прутка
      flagBarThickness: "",
      flagSizeSection: "",
      sizeSection:[
          {   id: 1,
              name: "1,03 м x 2,5 м",
              section: "",
              selected: false
          },
          {   id: 2,
              name: "1,53 м x 2,5 м",
              section: "",
              selected: false
          },
          {   id: 3,
              name: "1,73 м x 2,5 м",
              section: "",
              selected: false
          },
          {   id: 4,
              name: "2,03 м x 2,5 м",
              section: "",
              selected: false
          },
          {   id: 5,
              name: "2,4 м x 2,5 м",
              section: "",
              selected: false
          },
      ],
      btnBarThickness:[
          {
              id: 1,
              name: "3 мм",
              section: "",
              selected: false
          },
          {
              id: 2,
              name: "3-3,8 мм",
              section: "",
              selected: false
          },
          {
              id: 3,
              name: "3,8 мм",
              section: "",
              selected: false
          },
          {
              id: 4,
              name: "4,8 мм",
              section: "",
              selected: false
          },
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
 methods: {
     changeGalvanic(){
         return  this.flagSection = false;
     },
     changeColor() {
      return  this.flagSection = true;
    },
     clearAllButton(array){
         for(let i=0; i< array.length; i++){
             array[i].selected = false
         }
     },
     changeSizeSection(sizeBar){
         this.clearAllButton(this.sizeSection)
         this.flagSizeSection=this.sizeSection[sizeBar-1].name
         return this.sizeSection[sizeBar-1].selected = true
     },
     changeBarThickness(sizeBar){
         this.clearAllButton(this.btnBarThickness)
         this.flagBarThickness=this.btnBarThickness[sizeBar-1].name
         return this.btnBarThickness[sizeBar-1].selected = true
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
.btn-items-size_selected{
    background-color: #5cb071;
    font-size: 12px;
    color: white;
    padding: 6px;
    width: 90px;
    text-transform: uppercase;
    border-radius: 5px;
}
.btn-items-size {
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
</style>
