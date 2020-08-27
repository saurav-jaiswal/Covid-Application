<template>
  <q-page padding>
    <div class="row">
      <div class="col-lg-8 col-sm-12 col-xs-12 col-md-8">
        <!--        <p class="text-overline q-ml-md" style="font-size: 20px; width: 100%">Covid Tracker</p>-->
      </div>
      <div class="col-lg-4 col-sm-12 col-xs-12 col-md-4">
        <q-select
          outlined
          v-model="country"
          :options="this.countryList"
          @input="getData"
          input-debounce="0"
          label="Select Country"
        >
          <template v-slot:option="scope">
            <q-item
              v-bind="scope.itemProps"
              v-on="scope.itemEvents"
            >
              <q-item-section>
                <q-item-label v-html="scope.opt"/>
              </q-item-section>
            </q-item>
          </template>
        </q-select>
      </div>
    </div>
    <div class="row">
      <div class="col-lg-4 col-sm-12 col-xs-12 col-md-4">
        <q-card class="my-card q-ma-lg" flat bordered>
          <q-card-section style="padding-top: 0px;margin-top: -14px;">
            <img src="../assets/11.jpg" style="width: 100%;height: 206px" class="shadow-2 rounded-borders">
          </q-card-section>
          <q-card-section style="margin-top: -30px">
            <div class="text-h5 q-mt-sm q-mb-xs">Covid Cases</div>
          </q-card-section>
          <q-separator/>

          <q-card-actions>
            <q-icon name="confirmation_number" class="q-pa-md"/>
            <q-badge color="blue">{{cases}} {{cases.length}}</q-badge>
            <q-space/>

          </q-card-actions>

        </q-card>
      </div>
      <div class="col-lg-4 col-sm-12 col-xs-12 col-md-4">
        <q-card class="my-card q-ma-lg" flat bordered>
          <q-card-section style="padding-top: 0px;margin-top: -14px;">
            <img src="../assets/12.jpg" style="width: 100%;height: 206px" class="shadow-2 rounded-borders">
          </q-card-section>
          <q-card-section style="margin-top: -30px">
            <div class="text-h5 q-mt-sm q-mb-xs">Covid Deaths</div>
          </q-card-section>
          <q-separator/>

          <q-card-actions>
            <q-icon name="confirmation_number" class="q-pa-md"/>
            <q-badge color="blue">{{death}} {{death.length}}</q-badge>
            <q-space/>

          </q-card-actions>

        </q-card>
      </div>
      <div class="col-lg-4 col-sm-12 col-xs-12 col-md-4">
        <q-card class="my-card q-ma-lg" flat bordered>
          <q-card-section style="padding-top: 0px;margin-top: -14px;">
            <img border="" src="../assets/14.jpg" style="width: 100%;height: 206px" class="shadow-2 rounded-borders">
          </q-card-section>
          <q-card-section style="margin-top: -30px">
            <div class="text-h5 q-mt-sm q-mb-xs">Covid Recovered</div>
          </q-card-section>
          <q-separator/>

          <q-card-actions>
            <q-icon name="confirmation_number" class="q-pa-md"/>
            <span>
            <q-badge color="blue">{{recovered}} {{recovered.length}}</q-badge>
            </span>
            <q-space/>

          </q-card-actions>
        </q-card>
      </div>
    </div>
    <div class="row" v-if="this.cases || this.death || this.recovered">
      <div class="col" style=" width: 100%;height: 500px">
        <IEcharts
          :option="option"
        />
      </div>

    </div>
  </q-page>
</template>

<script>
    import IEcharts from 'vue-echarts-v3/src/full.js';

    export default {
        name: 'PageIndex',
        components: {
            IEcharts
        },
        data() {
            return {
                country: '',
                cases: '',
                death: '',
                recovered: '',
                countryList: [],
                filterOptions: this.countryList,


            }
        },
        methods: {
            getCountry() {
                fetch("https://covid-193.p.rapidapi.com/countries", {
                    "method": "GET",
                    "headers": {
                        "x-rapidapi-host": "covid-193.p.rapidapi.com",
                        "x-rapidapi-key": "920bddb8e1msha1b5fda7205ccd8p16e7a7jsn15a05d576b9a"
                    }
                })
                    .then(response => response.json()).then(data => {
                    this.countryList = data.response;
                })
            },
            getData() {
                fetch("https://covid-193.p.rapidapi.com/statistics?country=" + this.country, {
                    "method": "GET",
                    "headers": {
                        "x-rapidapi-host": "covid-193.p.rapidapi.com",
                        "x-rapidapi-key": "920bddb8e1msha1b5fda7205ccd8p16e7a7jsn15a05d576b9a"
                    }
                }).then(response => response.json()).then(data => {
                    data = data.response[0];
                    this.cases = data.cases.total;
                    this.death = data.deaths.total;
                    this.recovered = data.cases.recovered;

                })
            },
            closeState() {
                this.countryList = []
            }
        },
        mounted() {
            this.getCountry();
        },
        computed: {
            option() {
                return {
                    backgroundColor: '#2c343c',
                    title: {
                        text: 'Covid Graph',
                        left: 'center',
                        top: 20,
                        textStyle: {
                            color: '#ccc'
                        }
                    },

                    tooltip: {
                        trigger: 'item',
                        formatter: '{a} <br/>{b}: {c} ({d}%)'
                    },
                    legend: {
                        orient: 'vertical',
                        left: 10,
                        data: ['Cases', 'Death', 'Recovered'],
                        textStyle: {
                            color: '#ccc'
                        }
                    },
                    series: [
                        {
                            name: 'Covid',
                            type: 'pie',
                            radius: ['50%', '70%'],
                            avoidLabelOverlap: false,
                            label: {
                                show: false,
                                position: 'center'
                            },
                            emphasis: {
                                label: {
                                    show: true,
                                    fontSize: '30',
                                    fontWeight: 'bold'
                                }
                            },
                            labelLine: {
                                show: false
                            },
                            data: [
                                {value: this.cases, name: 'Cases'},
                                {value: this.death, name: 'Death'},
                                {value: this.recovered, name: 'Recovered'},

                            ]
                        }
                    ]
                }
            },
        }

    }
</script>
