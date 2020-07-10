<template>
    <v-container>
        <v-row>
            <h1 class="mt-4">Расчет забора</h1>
        </v-row>
        <v-row class="text-center justify-start mt-4">
            <v-col class="justify-start d-block" cols="12" md="8">
                <v-row class="justify-start">
                    <div v-for="item in commodityItems"
                           :key="item.id"
                          >
                        <button class="btn-items">
                            {{item.name}}
                        </button>
                    </div>
                </v-row>
                <v-row class="mt-4">
                    <div class="col-md-4 d-flex justify-start text-left align-content-start">
                        <img v-if="colored" class=" d-flex align-self-start justify-start" src="assets/img/section-colored.png"
                             height="160px"
                         alt="">
                        <img v-if="galvanic" class=" d-flex align-self-start justify-start" src="assets/img/section-galv.png"
                             height="160px"
                             alt="">
                    </div>
                    <div class="col-md-8 d-block">
                        <v-row class="justify-space-around">
                            <button class="btn-items-choice">
                                Крашеная
                            </button>
                            <button class="btn-items-choice">
                                Оцинкованая
                            </button>
                        </v-row>
                        <v-row class="justify-space-around d-block">
                            <v-row>
                                Толщина прутка
                            </v-row>
                            <v-row class="justify-space-around">
                                <button class="btn-items-size">
                                    3 мм
                                </button>
                                <button class="btn-items-size">
                                    3-3.8 мм
                                </button>
                                <button class="btn-items-size">
                                    3.8 мм
                                </button>
                                <button class="btn-items-size">
                                    4.8 мм
                                </button>
                            </v-row>

                        </v-row>
                        <v-row class="justify-space-around">
                            <v-row>
                                Размер секции ( высота x ширина )
                            </v-row>
                            <v-row class="justify-space-around">
                                <button class="btn-items-section">
                                   1,03 м x 2,5 м
                                </button>
                                <button class="btn-items-section">
                                    1,03 м x 2,5 м
                                </button>
                                <button class="btn-items-section">
                                    1,03 м x 2,5 м
                                </button>
                                <button class="btn-items-section">
                                    1,03 м x 2,5 м
                                </button>
                                <button class="btn-items-section">
                                    1,03 м x 2,5 м
                                </button>
                            </v-row>
                            <v-row>
                                <button class="btn-items">
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

                    <tr v-for="item in commodityItems"
                        :key="item.id">
                        <td> {{item.name}}</td>
                        <td> шт.</td>
                        <td> руб.</td>
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
            colored: true,
            galvanic: false,
            flagSection: "",
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
            ],
            })
    };
</script>
<style lang="scss">

    .table-out{
        width: 100%;
        tr td{
            padding: 8px 0;
            border-bottom: 1px solid #5cb071;
            width: 33%;
            text-align: center;
        }

    }
    .btn-items {
        background-color: #5cb071;
        padding: 6px;
        width: 110px;
        text-transform: uppercase;
        color: white;
        text-align: center;
        margin: 0 1px;
         }
    .btn-items_selected {
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
    .choice-title{
        background-color: #5cb071;
        text-transform: uppercase;
        color: white;
        height: 36px;
        padding-top: 6px;

    }
    .btn-items-choice{
        background-color: white;
        font-size: 12px;
        color: #5cb071;
        padding: 4px;
        width: 140px;
        text-transform: uppercase;
        border: 2px solid #5cb071;
        border-radius: 5px;

    }
    .btn-items-choice:hover,.btn-items-size:hover,.btn-items-section:hover{
        background-color: #5cb071;
        color: white;
    }
    .btn-items-size{
        background-color: white;
        font-size: 12px;
        color: #5cb071;
        padding: 4px;
        width: 90px;
        text-transform: uppercase;
        border: 2px solid #5cb071;
        border-radius: 5px;
    }
    .btn-items-section{
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

</style>
