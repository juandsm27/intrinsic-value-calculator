<template>
  
  <v-app>
    <v-main>
      <v-container class="mt-16">
        <v-row>
          <v-col cols="12" md="6">
            <v-col class="col-style" cols="12">
                <v-col class="col-style">
                  <v-text-field density="compact" type="text" v-model.number="name" label="Nombre" variant="outlined" @input="onChange($event)"></v-text-field>
                </v-col>
            </v-col>
            <v-col class="col-style" cols="12">
                <v-col class="col-style">
                  <v-text-field :prefix="currency" density="compact" type="number" v-model.number="precio" label="Precio" variant="outlined" @input="onChange($event)"></v-text-field>
                </v-col>
            </v-col>
            <v-col class="col-style" cols="12">
                <v-col class="col-style">
                  <v-text-field density="compact" type="number" v-model.number="metrica" label="Métrica" variant="outlined" @input="onChange($event)"></v-text-field>
                </v-col>
            </v-col>
            <v-col class="col-style" cols="12">
                <v-col class="col-style">
                  <v-text-field :prefix="percent" density="compact" type="number" v-model.number="descuento" label="Descuento" variant="outlined" @input="onChange($event)"></v-text-field>
                </v-col>
            </v-col>
          </v-col>
          <v-col cols="12" md="6">
            <v-row class="px-md-10">
              <v-col cols="12" md="6" class="calculator-investment">
                <div class="monthly-payment-mount">{{ currency }}{{ formattedNumber(this.valorEstimadoShow.toFixed(2)) }}</div>
                <v-divider class="monthly-payment-line"></v-divider>
                <h1 class="underline monthly-payment-title">valor estimado</h1>
              </v-col>
              <v-col v-if="this.valoracionTotalShow > 0" cols="12" md="6" class="calculator-investment">
                <div class="derecha monthly-payment-mount">{{ (formattedNumber(this.valoracionTotalShow.toFixed(2))) }}{{ percent }}</div>
                <v-divider class="monthly-payment-line"></v-divider>
                <h1 class="derecha underline2 monthly-payment-title">Sobrevalorado</h1>
              </v-col>
              <v-col v-if="this.valoracionTotalShow < 0" cols="12" md="6" class="calculator-investment">
                <div class="derecha monthly-payment-mount">{{ formattedNumber((this.valoracionTotalShow.toFixed(2))*(-1)) }}{{ percent }}</div>
                <v-divider class="monthly-payment-line"></v-divider>
                <h1 class="derecha underline2 monthly-payment-title">infravalorado</h1>
              </v-col>
              <v-col v-show="this.valorEstimadoShow >= this.precio && this.valorEstimadoShow > 0 " cols="12">
                <div>
                  <div id="chartdivPrincipal" style="width: 100%;"></div>
                </div>
              </v-col>
              <v-col v-show="this.valorEstimadoShow <= this.precio && this.valorEstimadoShow > 0 " cols="12">
                <div>
                  <div id="chartdivNegPrincipal" style="width: 100%;"></div>
                </div>
              </v-col>
            </v-row>
          </v-col>
        </v-row>
        
        <!-- CASO BASE -->
        <v-alert            
          color="error"
          icon="$error"
          title="Probabilidad Incorrecta"
          text="Ajusta tus probabilidades hasta llegar a 100%"
          v-if="this.validationError"
          >
        </v-alert>
        <h1 class="titlepie mt-10"> CASO BASE </h1>
        <v-divider class="monthly-payment-line-title"></v-divider>
        <v-row>
          <v-col cols="12" md="6">
            <v-col class="col-style" cols="12">
                <v-col class="col-style">
                  <v-text-field :prefix="percent" density="compact" type="text" v-model.number="crecimientoBase" label="Crecimiento 1-5 años" variant="outlined" @input="onChange($event)"></v-text-field>
                </v-col>
            </v-col>
            <v-col class="col-style" cols="12">
                <v-col class="col-style">
                  <v-text-field :prefix="percent" density="compact" type="number" v-model.number="crecimientoBaseB" label="Crecimiento 6-10 años" variant="outlined" @input="onChange($event)"></v-text-field>
                </v-col>
            </v-col>
            <v-col class="col-style" cols="12">
                <v-col class="col-style">
                  <v-text-field density="compact" type="number" v-model.number="multiploBase" label="Múltiplo Terminal" variant="outlined" @input="onChange($event)"></v-text-field>
                </v-col>
            </v-col>
            <v-col class="col-style" cols="12">
                <v-col class="col-style">
                  <v-text-field v-if="this.validationError == false" :prefix="percent" density="compact" type="number" v-model.number="probabilidadBase" label="Probabilidad" variant="outlined" @input="onChange($event)"></v-text-field>
                  <v-text-field v-else error :prefix="percent" density="compact" type="number" v-model.number="probabilidadBase" label="Probabilidad" variant="outlined" @input="onChange($event)"></v-text-field>
                </v-col>
            </v-col>
          </v-col>
          <v-col cols="12" md="6">
            <v-row class="px-md-10">
              <v-col cols="12" md="6" class="calculator-investment">
                <div class="monthly-payment-mount">{{ currency }}{{ formattedNumber(this.casoBaseTotalShow.toFixed(2)) }}</div>
                <v-divider class="monthly-payment-line"></v-divider>
                <h1 class="underline monthly-payment-title">caso base</h1>
              </v-col>
              <v-col v-if="this.valoracionBaseShow > 0" cols="12" md="6" class="calculator-investment">
                <div class="derecha monthly-payment-mount">{{ (formattedNumber(this.valoracionBaseShow.toFixed(2))) }}{{ percent }}</div>
                <v-divider class="monthly-payment-line"></v-divider>
                <h1 class="derecha underline2 monthly-payment-title">Sobrevalorado</h1>
              </v-col>
              <v-col v-if="this.valoracionBaseShow < 0" cols="12" md="6" class="calculator-investment">
                <div class="derecha monthly-payment-mount">{{ formattedNumber((this.valoracionBaseShow.toFixed(2))*(-1)) }}{{ percent }}</div>
                <v-divider class="monthly-payment-line"></v-divider>
                <h1 class="derecha underline2 monthly-payment-title">infravalorado</h1>
              </v-col>
              <v-col v-show="this.casoBaseTotalShow >= this.precio && this.casoBaseTotalShow > 0" cols="12">
                <div>
                  <div id="chartdiv" style="width: 100%;"></div>
                </div>
              </v-col>
              <v-col v-show="this.casoBaseTotalShow <= this.precio && this.casoBaseTotalShow > 0" cols="12">
                <div>
                  <div id="chartdivNeg" style="width: 100%;"></div>
                </div>
              </v-col>
            </v-row>
          </v-col>
        </v-row>
        <!-- CASO BASE -->
        
        <!-- CASO MEJOR -->
        <h1 class="titlepie mt-10"> MEJOR CASO </h1>
        <v-divider class="monthly-payment-line-title"></v-divider>
        <v-row>
          <v-col cols="12" md="6">
            <v-col class="col-style" cols="12">
                <v-col class="col-style">
                <v-text-field :prefix="percent" density="compact" type="text" v-model.number="crecimientoMejor" label="Crecimiento 1-5 años" variant="outlined" @input="onChange($event)"></v-text-field>
                </v-col>
            </v-col>
            <v-col class="col-style" cols="12">
                <v-col class="col-style">
                <v-text-field :prefix="percent" density="compact" type="number" v-model.number="crecimientoMejorB" label="Crecimiento 6-10 años" variant="outlined" @input="onChange($event)"></v-text-field>
                </v-col>
            </v-col>
            <v-col class="col-style" cols="12">
                <v-col class="col-style">
                <v-text-field density="compact" type="number" v-model.number="multiploMejor" label="Múltiplo Terminal" variant="outlined" @input="onChange($event)"></v-text-field>
                </v-col>
            </v-col>
            <v-col class="col-style" cols="12">
                <v-col class="col-style">
                <v-text-field v-if="this.validationError == false" :prefix="percent" density="compact" type="number" v-model.number="probabilidadMejor" label="Probabilidad" variant="outlined" @input="onChange($event)"></v-text-field>
                <v-text-field v-else error :prefix="percent" density="compact" type="number" v-model.number="probabilidadMejor" label="Probabilidad" variant="outlined" @input="onChange($event)"></v-text-field>
                </v-col>
            </v-col>
          </v-col>
          <v-col cols="12" md="6">
            <v-row class="px-md-10">
              <v-col cols="12"  md="6" class="calculator-investment">
                <div class="monthly-payment-mount">{{ currency }}{{ formattedNumber(this.casoMejorTotalShow.toFixed(2)) }}</div>
                <v-divider class="monthly-payment-line"></v-divider>
                <h1 class="underline monthly-payment-title">MEJOR CASO</h1>
              </v-col>
              <v-col v-if="this.valoracionMejorShow > 0" cols="12" md="6" class="calculator-investment">
                <div class="derecha monthly-payment-mount">{{ (formattedNumber(this.valoracionMejorShow.toFixed(2))) }}{{ percent }}</div>
                <v-divider class="monthly-payment-line"></v-divider>
                <h1 class="derecha underline2 monthly-payment-title">Sobrevalorado</h1>
              </v-col>
              <v-col v-if="this.valoracionMejorShow < 0" cols="12" md="6" class="calculator-investment">
                <div class="derecha monthly-payment-mount">{{ formattedNumber((this.valoracionMejorShow.toFixed(2))*(-1)) }}{{ percent }}</div>
                <v-divider class="monthly-payment-line"></v-divider>
                <h1 class="derecha underline2 monthly-payment-title">infravalorado</h1>
              </v-col>
              <v-col v-show="this.casoMejorTotalShow >= this.precio && this.casoMejorTotalShow > 0" cols="12">
                <div>
                  <div id="chartdivMejor" style="width: 100%;"></div>
                </div>
              </v-col>
              <v-col v-show="this.casoMejorTotalShow <= this.precio && this.casoMejorTotalShow > 0" cols="12">
                <div>
                  <div id="chartdivNegMejor" style="width: 100%;"></div>
                </div>
              </v-col>
            </v-row>
          </v-col>
        </v-row>
        <!-- CASO MEJOR -->
        
        <!-- CASO PEOR -->
        <h1 class="titlepie mt-10"> PEOR CASO </h1>
        <v-divider class="monthly-payment-line-title"></v-divider>
        <v-row>
          <v-col cols="12" md="6">
            <v-col class="col-style" cols="12">
                <v-col class="col-style">
                <v-text-field :prefix="percent" density="compact" type="text" v-model.number="crecimientoPeor" label="Crecimiento 1-5 años" variant="outlined" @input="onChange($event)"></v-text-field>
                </v-col>
            </v-col>
            <v-col class="col-style" cols="12">
                <v-col class="col-style">
                <v-text-field :prefix="percent" density="compact" type="number" v-model.number="crecimientoPeorB" label="Crecimiento 6-10 años" variant="outlined" @input="onChange($event)"></v-text-field>
                </v-col>
            </v-col>
            <v-col class="col-style" cols="12">
                <v-col class="col-style">
                <v-text-field density="compact" type="number" v-model.number="multiploPeor" label="Múltiplo TerminalMúltiplo Terminal" variant="outlined" @input="onChange($event)"></v-text-field>
                </v-col>
            </v-col>
            <v-col class="col-style" cols="12">
                <v-col class="col-style">
                <v-text-field v-if="this.validationError == false" :prefix="percent" density="compact" type="number" v-model.number="probabilidadPeor" label="Probabilidad" variant="outlined" @input="onChange($event)"></v-text-field>
                <v-text-field v-else error :prefix="percent" density="compact" type="number" v-model.number="probabilidadPeor" label="Probabilidad" variant="outlined" @input="onChange($event)"></v-text-field>
                </v-col>
            </v-col>
          </v-col>
          <v-col cols="12" md="6">
            <v-row class="px-md-10">
              <v-col cols="12" md="6" class="calculator-investment">
                <div class="monthly-payment-mount">{{ currency }}{{ formattedNumber(this.casoPeorTotalShow.toFixed(2)) }}</div>
                <v-divider class="monthly-payment-line"></v-divider>
                <h1 class="underline monthly-payment-title">PEOR CASO</h1>
              </v-col>
              <v-col v-if="this.valoracionPeorShow > 0" cols="12" md="6" class="calculator-investment">
                <div class="derecha monthly-payment-mount">{{ (formattedNumber(this.valoracionPeorShow.toFixed(2))) }}{{ percent }}</div>
                <v-divider class="monthly-payment-line"></v-divider>
                <h1 class="derecha underline2 monthly-payment-title">Sobrevalorado</h1>
              </v-col>
              <v-col v-if="this.valoracionPeorShow < 0" cols="12" md="6" class="calculator-investment">
                <div class="derecha monthly-payment-mount">{{ formattedNumber((this.valoracionPeorShow.toFixed(2))*(-1)) }}{{ percent }}</div>
                <v-divider class="monthly-payment-line"></v-divider>
                <h1 class="derecha underline2 monthly-payment-title">infravalorado</h1>
              </v-col>
              <v-col v-show="this.casoPeorTotalShow >= this.precio && this.casoPeorTotalShow > 0" cols="12">
                <div>
                  <div id="chartdivPeor" style="width: 100%;"></div>
                </div>
              </v-col>
              <v-col v-show="this.casoPeorTotalShow <= this.precio && this.casoPeorTotalShow > 0" cols="12">
                <div>
                  <div id="chartdivNegPeor" style="width: 100%;"></div>
                </div>
              </v-col>
            </v-row>
          </v-col>
        </v-row>
        <!-- CASO PEOR -->
        
        <!-- COLUMNA 3 -->
        <v-row>
          <v-col cols="12">
            <v-card>
              <v-tabs class="tabtext" v-model="tab">
                <v-tab value="casoBase">Caso Base</v-tab>
              </v-tabs>
              <v-card-text>
                <v-window v-model="tab"> 
                  <v-window-item value="casoBase">
                    <v-table width="100">
                      <tbody>
                        <th>
                          <tr>Año</tr>
                          <tr>Metrica</tr>
                          <tr>PV</tr>
                        </th>
                        <td v-for="item in tablaAnual" :key="item.name">
                          <th>{{ item.year }}</th>
                          <tr>{{ item.metrica.toFixed(2) }}</tr>
                          <tr>{{ item.pv.toFixed(2) }}</tr>
                        </td>
                        <td v-for="item in tablaAnual" :key="item.name">
                          <tr style="font-weight: 700;">{{ item.lastyear }}</tr>
                          <tr>{{ item.metrica_terminal }}</tr>
                          <tr>{{ item.pv_Terminal }}</tr>
                        </td>
                      </tbody>
                    </v-table>
                  </v-window-item>
                </v-window>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>       

        <v-row>
          <v-col cols="12">
            <v-card>
              <v-tabs class="tabtext" v-model="tab2">
                <v-tab value="mejorCaso">Mejor Caso</v-tab>
              </v-tabs>
              <v-card-text>
                <v-window v-model="tab2"> 
                  <v-window-item value="mejorCaso">
                    <v-table height="80px">
                      <tbody>
                        <th>
                          <tr>Año</tr>
                          <tr>Metrica</tr>
                          <tr>PV</tr>
                        </th>
                        <td v-for="item in tablaAnualMejor" :key="item.name">
                          <th>{{ item.yearMejor }}</th>
                          <tr>{{ item.metrica_mejor.toFixed(2) }}</tr>
                          <tr>{{ item.pv_mejor.toFixed(2) }}</tr>
                        </td>
                        <td v-for="item in tablaAnualMejor" :key="item.name">
                          <tr style="font-weight: 700;">{{ item.lastyear }}</tr>
                          <tr>{{ item.metrica_terminal_mejor }}</tr>
                          <tr>{{ item.pv_terminal_mejor }}</tr>
                        </td>
                      </tbody>
                    </v-table>
                  </v-window-item>
                </v-window>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>             
        
        <v-row>
          <v-col cols="12">
            <v-card>
              <v-tabs class="tabtext" v-model="tab3">
                <v-tab value="peorCaso">Peor Caso</v-tab>
              </v-tabs>
              <v-card-text>
                <v-window v-model="tab3"> 
                  <v-window-item value="peorCaso">
                    <v-table height="80px">
                      <tbody>
                        <th>
                          <tr>Año</tr>
                          <tr>Metrica</tr>
                          <tr>PV</tr>
                        </th>
                        <td v-for="item in tablaAnualPeor" :key="item.name">
                          <th>{{ item.yearPeor }}</th>
                          <tr>{{ item.metrica_peor.toFixed(2) }}</tr>
                          <tr>{{ item.pv_peor.toFixed(2) }}</tr>
                        </td>
                        <td v-for="item in tablaAnualPeor" :key="item.name">
                          <tr style="font-weight: 700;">{{ item.lastyear }}</tr>
                          <tr>{{ item.metrica_terminal_peor }}</tr>
                          <tr>{{ item.pv_terminal_peor }}</tr>
                        </td>
                      </tbody>
                    </v-table>
                  </v-window-item>
                </v-window>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>       

      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import * as am4core from "@amcharts/amcharts4/core";
import * as am4charts from "@amcharts/amcharts4/charts";
import am4themes_animated from "@amcharts/amcharts4/themes/animated";
am4core.useTheme(am4themes_animated);

export default {
  name: 'App',

  data() {
    return {
      percent: "%",
      currency: "$",
      validationError: false,
      tab: null,
      tab2: null,
      tab3: null,
      name: '',
      casoBaseTotalShow: 0,
      casoBaseTotal: 0,
      precio: 0, 
      metrica: 0, 
      descuento: 0, 
      crecimientoBase: 15, 
      crecimientoBaseB: 10, 
      multiploBase: 20, 
      probabilidadBase: 50, 
      crecimientoMejor: 25, 
      crecimientoMejorB: 20, 
      multiploMejor: 30, 
      probabilidadMejor: 25, 
      crecimientoPeor: 10, 
      crecimientoPeorB: 5, 
      multiploPeor: 15, 
      probabilidadPeor: 25, 
      tablaAnual: [],
      tablaAnualMejor: [],
      tablaAnualPeor: [],
      pv: 0,
      valoracionBaseShow: 0,
      pvTerminalShow: 0,
      metricaTerminalShow: 0,
      metricaTerminalMejorShow: 0,
      pvTerminalMejorShow: 0,
      casoMejorTotalShow: 0,
      valoracionMejorShow: 0,
      valoracionTotalShow: 0,
      metricaTerminalPeorShow: 0,
      pvTerminalPeorShow: 0,
      casoPeorTotalShow: 0,
      valoracionPeorShow: 0,
      valorEstimadoShow: 0,
      chartData: [],
      chartDataNeg: [],
      chartDataMejor: [],
      chartDataNegMejor: [],
      chartDataPeor: [],
      chartDataNegPeor: [],
      chartDataPrincipal: [],
      chartDataNegPrincipal: [],
    };
  },
  methods: {
    
    onChange() {
      if (
        this.precio !== "" &&
        this.precio > 0 &&
        this.metrica !== "" &&
        this.metrica > 0 &&
        this.descuento !== "" &&
        this.descuento > 0
      ) {
        this.validationError1 = false
        this.calculate2();
      } else {
        this.validationError1 = true
      }
    },
    calculate2(){
      if ((this.probabilidadBase + this.probabilidadMejor + this.probabilidadPeor) === 100){
        this.validationError = false
        this.calculate();
      } else {
        this.validationError = true
      }
    },
    calculate() {
      const currentDate = new Date();
      let actYear = currentDate.getFullYear();
      let crecimientoBaseTabla = this.crecimientoBase/100;
      let crecimientoBaseBTabla = this.crecimientoBaseB/100;
      let descuentoTabla = this.descuento/100;
      // let probabilidadBaseTabla = this.probabilidadBase/100;
      let crecimientoMejorTabla = this.crecimientoMejor/100;
      let crecimientoMejorBTabla = this.crecimientoMejorB/100;
      // let probabilidadMejorTabla = this.probabilidadMejor/100;
      let crecimientoPeorTabla = this.crecimientoPeor/100;
      let crecimientoPeorBTabla = this.crecimientoPeorB/100;
      // let probabilidadPeorTabla = this.probabilidadPeor/100;

      let metricaTabla = this.metrica*(1+crecimientoBaseTabla);
      let metricaTablaMejor = this.metrica*(1+crecimientoMejorTabla);
      let metricaTablaPeor = this.metrica*(1+crecimientoPeorTabla);
      let primerYear = actYear;
      let pv = metricaTabla*Math.pow((1+descuentoTabla), (primerYear-actYear-1))
      let pvMejor = metricaTablaMejor*Math.pow((1+descuentoTabla), (primerYear-actYear-1))
      let pvPeor = metricaTablaPeor*Math.pow((1+descuentoTabla), (primerYear-actYear-1))
      this.tablaAnual = [];
      this.tablaAnualMejor = [];
      this.tablaAnualPeor = [];
      let casoBaseTotal = pv;
      let casoMejorTotal = pvMejor;
      let casoPeorTotal = pvPeor;

      for (let index = 1; index <= 10; index++) {
        if (index == 1) {
          this.tablaAnual.push({ year: (actYear+1).toString(), metrica: metricaTabla, pv: pv, }); 
          this.tablaAnualMejor.push({ yearMejor: (actYear+1).toString(), metrica_mejor: metricaTablaMejor, pv_mejor: pvMejor, }); 
          this.tablaAnualPeor.push({ yearPeor: (actYear+1).toString(), metrica_peor: metricaTablaPeor, pv_peor: pvPeor, }); 
        } else if (index <= 5) {
          //BASE
          metricaTabla = metricaTabla*(1+crecimientoBaseTabla);
          pv = metricaTabla*Math.pow((1+descuentoTabla), (primerYear-actYear-1))
          casoBaseTotal = pv+casoBaseTotal;
          this.tablaAnual.push({ year: (actYear+1).toString(), metrica: metricaTabla, pv: pv, }); 
          //MEJOR
          metricaTablaMejor = metricaTablaMejor*(1+crecimientoMejorTabla);
          pvMejor = metricaTablaMejor*Math.pow((1+descuentoTabla), (primerYear-actYear-1))
          casoMejorTotal = pvMejor+casoMejorTotal;
          this.tablaAnualMejor.push({ yearMejor: (actYear+1).toString(), metrica_mejor: metricaTablaMejor, pv_mejor: pvMejor, }); 
          //PEOR
          metricaTablaPeor = metricaTablaPeor*(1+crecimientoPeorTabla);
          pvPeor = metricaTablaPeor*Math.pow((1+descuentoTabla), (primerYear-actYear-1))
          casoPeorTotal = pvPeor+casoPeorTotal;
          this.tablaAnualPeor.push({ yearPeor: (actYear+1).toString(), metrica_peor: metricaTablaPeor, pv_peor: pvPeor, }); 
        } else {
          //BASE
          metricaTabla = metricaTabla*(1+crecimientoBaseBTabla);
          pv = metricaTabla*Math.pow((1+descuentoTabla), (primerYear-actYear-1))
          casoBaseTotal = pv+casoBaseTotal;
          if (index >= 10) {
            let penultimoAno = this.tablaAnual[8].metrica;
            let lastYear = this.tablaAnual[8].year;
            let metricaTerminal = penultimoAno*this.multiploBase;
            console.log(penultimoAno);
            let pvTerminal = metricaTerminal*Math.pow((1+descuentoTabla), (primerYear-lastYear-1));
            console.log(pvTerminal);
            casoBaseTotal = casoBaseTotal+pvTerminal;
            this.tablaAnual.push({ year: (actYear+1).toString(), metrica: metricaTabla, pv: pv, metrica_terminal: metricaTerminal.toFixed(2), pv_Terminal: pvTerminal.toFixed(2), lastyear: (actYear+1).toString(), });
            this.metricaTerminalShow = metricaTerminal;
            this.pvTerminalShow = pvTerminal;
          } else {
            this.tablaAnual.push({ year: (actYear+1).toString(), metrica: metricaTabla, pv: pv, });
            }
          this.casoBaseTotalShow = casoBaseTotal;
          let valoracionBase = (casoBaseTotal-this.precio)/this.precio;
          this.valoracionBaseShow = valoracionBase*100;
          //MEJOR
          metricaTablaMejor = metricaTablaMejor*(1+crecimientoMejorBTabla);
          pvMejor = metricaTablaMejor*Math.pow((1+descuentoTabla), (primerYear-actYear-1))
          casoMejorTotal = pvMejor+casoMejorTotal;
          if (index >= 10) {
            let penultimoAnoMejor = this.tablaAnualMejor[8].metrica_mejor;
            let lastYearMejor = this.tablaAnualMejor[8].yearMejor;
            const lastYearTabla = (actYear+1).toString();
            let metricaTerminalMejor = penultimoAnoMejor*this.multiploMejor;
            let pvTerminalMejor = metricaTerminalMejor*Math.pow((1+descuentoTabla), (primerYear-lastYearMejor-1));
            casoMejorTotal = casoMejorTotal+pvTerminalMejor;
            this.tablaAnualMejor.push({ yearMejor: (actYear+1).toString(), metrica_mejor: metricaTablaMejor, pv_mejor: pvMejor, metrica_terminal_mejor: metricaTerminalMejor.toFixed(2), pv_terminal_mejor: pvTerminalMejor.toFixed(2), lastyear: lastYearTabla, });
            this.metricaTerminalMejorShow = metricaTerminalMejor;
            this.pvTerminalMejorShow = pvTerminalMejor;
          } else {
            this.tablaAnualMejor.push({ yearMejor: (actYear+1).toString(), metrica_mejor: metricaTablaMejor, pv_mejor: pvMejor, });
            }
          this.casoMejorTotalShow = casoMejorTotal;
          let valoracionMejor = (casoMejorTotal-this.precio)/this.precio
          this.valoracionMejorShow = valoracionMejor*100;
          //PEOR
          metricaTablaPeor = metricaTablaPeor*(1+crecimientoPeorBTabla);
          pvPeor = metricaTablaPeor*Math.pow((1+descuentoTabla), (primerYear-actYear-1))
          casoPeorTotal = pvPeor+casoPeorTotal;
          if (index >= 10) {
            let penultimoAnoPeor = this.tablaAnualPeor[8].metrica_peor;
            let lastYearPeor = this.tablaAnualPeor[8].yearPeor;
            let metricaTerminalPeor = penultimoAnoPeor*this.multiploPeor;
            let pvTerminalPeor = metricaTerminalPeor*Math.pow((1+descuentoTabla), (primerYear-lastYearPeor-1));
            casoPeorTotal = casoPeorTotal+pvTerminalPeor;
            this.tablaAnualPeor.push({ yearPeor: (actYear+1).toString(), metrica_peor: metricaTablaPeor, pv_peor: pvPeor, metrica_terminal_peor: metricaTerminalPeor.toFixed(2), pv_terminal_peor: pvTerminalPeor.toFixed(2), lastyear: (actYear+1).toString(), });
            this.metricaTerminalPeorShow = metricaTerminalPeor;
            this.pvTerminalPeorShow = pvTerminalPeor;
          } else {
            this.tablaAnualPeor.push({ yearPeor: (actYear+1).toString(), metrica_peor: metricaTablaPeor, pv_peor: pvPeor, });
            }
          this.casoPeorTotalShow = casoPeorTotal;
          let valoracionPeor = (casoPeorTotal-this.precio)/this.precio
          this.valoracionPeorShow = valoracionPeor*100;
        }
        let valorEstimado = ((casoPeorTotal*this.probabilidadPeor) + (casoMejorTotal*this.probabilidadMejor) + (casoBaseTotal*this.probabilidadBase))/100;
        this.valorEstimadoShow = valorEstimado;
        let valoracionTotal = ((valorEstimado-this.precio)/this.precio)*100;
        this.valoracionTotalShow = valoracionTotal;

        //BASE
        this.chartData = [];
        this.chartData.push ({
          "blank": "",
          "Actual": this.precio, // Precio
          "Descontado": this.precio, // Precio
          "Diferencia": (this.casoBaseTotalShow - this.precio), // Valor estimado - Precio
          "Target": this.casoBaseTotalShow, // Valor estimado
          "Tooltip": this.valoracionBaseShow,
          "color": am4core.color("#659F6B")
            }),
        this.chart.data = this.chartData;
        
        this.chartDataNeg = [];
        this.chartDataNeg.push ({
          "blank": "",
          "Actual": this.precio, // Precio
          "Descontado": this.precio, // Precio
          "Diferencia": (this.precio - this.casoBaseTotalShow), // Precio - Valor estimado
          "Target": this.casoBaseTotalShow, // Valor estimado
          "Tooltip": this.valoracionBaseShow,
          "color": am4core.color("#A63838")
            }),
        this.chartNeg.data = this.chartDataNeg;

        //MEJOR
        this.chartDataMejor = [];
        this.chartDataMejor.push ({
          "blank": "",
          "Actual": this.precio, // Precio
          "Descontado": this.precio, // Precio
          "Diferencia": (this.casoMejorTotalShow - this.precio), // Valor estimado - Precio
          "Target": this.casoMejorTotalShow, // Valor estimado
          "Tooltip": this.valoracionMejorShow,
          "color": am4core.color("#659F6B")
            }),
        this.chartMejor.data = this.chartDataMejor;
        
        this.chartDataNegMejor = [];
        this.chartDataNegMejor.push ({
          "blank": "",
          "Actual": this.precio, // Precio
          "Descontado": this.precio, // Precio
          "Diferencia": (this.precio - this.casoMejorTotalShow), // Valor estimado - Precio
          "Target": this.casoMejorTotalShow, // Valor estimado
          "Tooltip": this.valoracionMejorShow,
          "color": am4core.color("#A63838")
            }),
        this.chartNegMejor.data = this.chartDataNegMejor;

        //PEOR
        this.chartDataPeor = [];
        this.chartDataPeor.push ({
          "blank": "",
          "Actual": this.precio, // Precio
          "Descontado": this.precio, // Precio
          "Diferencia": (this.casoPeorTotalShow - this.precio), // Valor estimado - Precio
          "Target": this.casoPeorTotalShow, // Valor estimado
          "Tooltip": this.valoracionPeorShow,
          "color": am4core.color("#659F6B")
            }),
        this.chartPeor.data = this.chartDataPeor;

        this.chartDataNegPeor = [];
        this.chartDataNegPeor.push ({
          "blank": "",
          "Actual": this.precio, // Precio
          "Descontado": this.precio, // Precio
          "Diferencia": (this.precio - this.casoPeorTotalShow), // Valor estimado - Precio
          "Target": this.casoPeorTotalShow, // Valor estimado
          "Tooltip": this.valoracionPeorShow,
          "color": am4core.color("#A63838")
            }),
        this.chartNegPeor.data = this.chartDataNegPeor;

        //PRINCIPAL
        this.chartDataPrincipal = [];
        this.chartDataPrincipal.push ({
          "blank": "",
          "Actual": this.precio, // Precio
          "Descontado": this.precio, // Precio
          "Diferencia": (valorEstimado - this.precio), // Valor estimado - Precio
          "Target": valorEstimado, // Valor estimado
          "Tooltip": this.valoracionTotalShow,
          "color": am4core.color("#659F6B")
            }),
        this.chartPrincipal.data = this.chartDataPrincipal;
        
        this.chartDataNegPrincipal = [];
        this.chartDataNegPrincipal.push ({
          "blank": "",
          "Actual": this.precio, // Precio
          "Descontado": this.precio, // Precio
          "Diferencia": (this.precio - valorEstimado), // Valor estimado - Precio
          "Target": valorEstimado, // Valor estimado
          "Tooltip": this.valoracionTotalShow,
          "color": am4core.color("#A63838")
            }),
        this.chartNegPrincipal.data = this.chartDataNegPrincipal;

        actYear++;
      }
    },
    
    formattedNumber(number) {
      return number.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    },

  },
  
  mounted() {
    this.chart = am4core.create("chartdiv", am4charts.XYChart);
    this.chart.padding(0, 0, 0, 0);
      if (this.chart.logo) {
        this.chart.logo.dispose()
      }
      this.chart.data = [{
          "blank": "",
          "Actual": 0, // Precio
          "Descontado": 0, // Precio
          "Diferencia": 0, // Valor estimado - Precio
          "Target": 0, // Valor estimado
          "Tooltip": 0,
          "color": am4core.color("#659F6B")
        }];

      let categoryAxis = this.chart.yAxes.push(new am4charts.CategoryAxis());
      categoryAxis.renderer.grid.template.location = 0;
      categoryAxis.dataFields.category = "blank";
      categoryAxis.renderer.minGridDistance = 1;
      categoryAxis.renderer.inversed = true;
      categoryAxis.renderer.grid.template.disabled = true;
      categoryAxis.numberFormatter.numberFormat = "$#,###.##";

      let valueAxis = this.chart.xAxes.push(new am4charts.ValueAxis());
      valueAxis.strictMinMax = true;
      valueAxis.extraMax = 0.05;
      valueAxis.min = 0;
      valueAxis.renderer.opposite = true;
      //valueAxis.renderer.grid.template.disabled = true;
      valueAxis.renderer.minGridDistance = 50;

      //ACTUAL
      let series = this.chart.series.push(new am4charts.ColumnSeries());
      series.dataFields.categoryY = "blank";
      series.dataFields.valueX = "Actual";
      series.name = "Actual";
      series.columns.template.fill = "#123B4F";
      series.columns.template.strokeOpacity = 0;
      series.columns.template.height = am4core.percent(100);

      let labelBullet = series.bullets.push(new am4charts.LabelBullet())
      labelBullet.label.horizontalCenter = "left";
      labelBullet.label.dx = 20;
      labelBullet.label.text = "{name}: [bold]{Actual}";
      labelBullet.locationX = 1;
      labelBullet.label.fill = "#fff";


      //DIFERENCIA
      let series3 = this.chart.series.push(new am4charts.ColumnSeries());
      series3.dataFields.categoryY = "blank";
      series3.dataFields.valueX = "Diferencia";
      series3.name = "Diferencia";
      series3.columns.template.fill = "#659F6B";
      series3.columns.template.strokeOpacity = 0;
      series3.columns.template.background.fillOpacity = 0.2;
      series3.columns.template.height = am4core.percent(100);
      series3.stacked = true;

      series3.columns.template.adapter.add("fill", function(fill, target) {
        let pattern = new am4core.LinePattern();
        pattern.width = 10;
        pattern.height = 10;
        pattern.strokeWidth = 1;
        pattern.stroke = target.dataItem.dataContext.color;
        pattern.rotation = 45;
        return pattern;
      });

      series3.columns.template.background.adapter.add("fill", function(fill, target) {
        return target.dataItem ? target.dataItem.dataContext.color : fill;
      });


      //DESCONTADO
      let series2 = this.chart.series.push(new am4charts.ColumnSeries());
      series2.dataFields.categoryY = "blank";
      series2.dataFields.valueX = "Target";
      series2.name = "Descontado";
      series2.columns.template.fill = "#1D5F80";
      series2.columns.template.strokeOpacity = 0;
      series2.columns.template.height = am4core.percent(100);

      labelBullet = series2.bullets.push(new am4charts.LabelBullet())
      labelBullet.label.horizontalCenter = "left";
      labelBullet.label.dx = 20;
      labelBullet.label.text = "{name}: [bold]{Target}";
      labelBullet.locationX = 1;
      labelBullet.label.fill = "#fff";


      //DIFERENCIA (RANGOS)
      let series4 = this.chart.series.push(new am4charts.StepLineSeries());
      series4.dataFields.valueX = "Actual";
      series4.dataFields.categoryY = "blank";
      series4.strokeWidth = 5;
      series4.startLocation = 0;
      series4.endLocation = 1;
      series4.stroke = "#659F6B";

      let series5 = this.chart.series.push(new am4charts.StepLineSeries());
      series5.dataFields.valueX = "Target";
      series5.dataFields.categoryY = "blank";
      series5.strokeWidth = 5;
      series5.startLocation = 0;
      series5.endLocation = 1;
      series5.stroke = "#659F6B";
      series5.tooltipText = "[bold, #496548, font-size:25px]{Tooltip.formatNumber('#,###.##s')}%";
      series5.tooltip.getFillFromObject = false;
      series5.tooltip.background.fill = "#fff";
      series5.tooltip.background.stroke = "#496548";
      series5.tooltip.background.filters.clear();


      // CURSOR
      this.chart.cursor = new am4charts.XYCursor()
      this.chart.cursor.behavior = "none";
      this.chart.cursor.lineX.disabled = true;
      this.chart.cursor.lineY.disabled = true;
      valueAxis.cursorTooltipEnabled = false;

      //NEG
      
      this.chartNeg = am4core.create("chartdivNeg", am4charts.XYChart);
      this.chartNeg.padding(0, 0, 0, 0);
        if (this.chartNeg.logo) {
          this.chartNeg.logo.dispose()
        }
        this.chartNeg.data = [{
            "blank": "",
            "Actual": 0, // Precio
            "Descontado": 0, // Precio
            "Diferencia": 0, //  Precio - Valor estimado
            "Target": 0, // Valor estimado
            "Tooltip": 0,
            "color": am4core.color("#659F6B")
          }];

        var categoryAxisNeg = this.chartNeg.yAxes.push(new am4charts.CategoryAxis());
        categoryAxisNeg.renderer.grid.template.location = 0;
        categoryAxisNeg.dataFields.category = "blank";
        categoryAxisNeg.renderer.minGridDistance = 1;
        categoryAxisNeg.renderer.inversed = true;
        categoryAxisNeg.renderer.grid.template.disabled = true;
        categoryAxisNeg.numberFormatter.numberFormat = "$#,###.##";

        var valueAxisNeg = this.chartNeg.xAxes.push(new am4charts.ValueAxis());
        valueAxisNeg.strictMinMax = true;
        valueAxisNeg.extraMax = 0.05;
        valueAxisNeg.min = 0;
        valueAxisNeg.renderer.opposite = true;
        //valueAxis.renderer.grid.template.disabled = true;
        valueAxisNeg.renderer.minGridDistance = 50;

        //ACTUAL
        var seriesNeg = this.chartNeg.series.push(new am4charts.ColumnSeries());
        seriesNeg.dataFields.categoryY = "blank";
        seriesNeg.dataFields.valueX = "Actual";
        seriesNeg.name = "Actual";
        seriesNeg.columns.template.fill = "#123B4F";
        seriesNeg.columns.template.strokeOpacity = 0;
        seriesNeg.columns.template.height = am4core.percent(100);

        var labelBulletNeg = seriesNeg.bullets.push(new am4charts.LabelBullet())
        labelBulletNeg.label.horizontalCenter = "left";
        labelBulletNeg.label.dx = 20;
        labelBulletNeg.label.text = "{name}: [bold]{Actual}";
        labelBulletNeg.locationX = 1;
        labelBulletNeg.label.fill = "#fff";


        //DESCONTADO
        var series2Neg = this.chartNeg.series.push(new am4charts.ColumnSeries());
        series2Neg.dataFields.categoryY = "blank";
        series2Neg.dataFields.valueX = "Target";
        series2Neg.name = "Descontado";
        series2Neg.columns.template.fill = "#1D5F80";
        series2Neg.columns.template.strokeOpacity = 0;
        series2Neg.columns.template.height = am4core.percent(100);

        labelBulletNeg = series2Neg.bullets.push(new am4charts.LabelBullet())
        labelBulletNeg.label.horizontalCenter = "left";
        labelBulletNeg.label.dx = 20;
        labelBulletNeg.label.text = "{name}: [bold]{Target}";
        labelBulletNeg.locationX = 1;
        labelBulletNeg.label.fill = "#fff";


        //DIFERENCIA
        var series3Neg = this.chartNeg.series.push(new am4charts.ColumnSeries());
        series3Neg.dataFields.categoryY = "blank";
        series3Neg.dataFields.valueX = "Diferencia";
        series3Neg.name = "Diferencia";
        series3Neg.columns.template.fill = "#A63838";
        series3Neg.columns.template.strokeOpacity = 0;
        series3Neg.columns.template.background.fillOpacity = 0.2;
        series3Neg.columns.template.height = am4core.percent(100);
        series3Neg.stacked = true;

        series3Neg.columns.template.adapter.add("fill", function(fill, target) {
          var pattern = new am4core.LinePattern();
          pattern.width = 10;
          pattern.height = 10;
          pattern.strokeWidth = 1;
          pattern.stroke = target.dataItem.dataContext.color;
          pattern.rotation = 45;
          return pattern;
        });

        series3Neg.columns.template.background.adapter.add("fill", function(fill, target) {
          return target.dataItem ? target.dataItem.dataContext.color : fill;
        });

        //DIFERENCIA (RANGOS)
        var series4Neg = this.chartNeg.series.push(new am4charts.StepLineSeries());
        series4Neg.dataFields.categoryY = "blank";
        series4Neg.dataFields.valueX = "Target";
        series4Neg.strokeWidth = 5;
        series4Neg.startLocation = 0;
        series4Neg.endLocation = 1;
        series4Neg.stroke = "#A63838";

        var series5Neg = this.chartNeg.series.push(new am4charts.StepLineSeries());
        series5Neg.dataFields.categoryY = "blank";
        series5Neg.dataFields.valueX = "Actual";
        series5Neg.strokeWidth = 5;
        series5Neg.startLocation = 0;
        series5Neg.endLocation = 1;
        series5Neg.stroke = "#A63838";
        series5Neg.tooltipText = "[bold, #6A2C2A, font-size:25px]{Tooltip.formatNumber('#,###.##s')}%";
        series5Neg.tooltip.getFillFromObject = false;
        series5Neg.tooltip.background.fill = "#fff";
        series5Neg.tooltip.background.stroke = "#6A2C2A";
        series5Neg.tooltip.background.filters.clear();


        // CURSOR
        this.chartNeg.cursor = new am4charts.XYCursor()
        this.chartNeg.cursor.behavior = "none";
        this.chartNeg.cursor.lineX.disabled = true;
        this.chartNeg.cursor.lineY.disabled = true;
        valueAxisNeg.cursorTooltipEnabled = false;

      // MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR 
      // MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR 
      // MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR 
      // MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR 
      // MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR MEJOR 
      
      this.chartMejor = am4core.create("chartdivMejor", am4charts.XYChart);
      this.chartMejor.padding(0, 0, 0, 0);
        if (this.chartMejor.logo) {
          this.chartMejor.logo.dispose()
        }
        this.chartMejor.data = [{
            "blank": "",
            "Actual": 0, // Precio
            "Descontado": 0, // Precio
            "Diferencia": 0, // Valor estimado - Precio
            "Target": 0, // Valor estimado
            "Tooltip": 0,
            "color": am4core.color("#659F6B")
          }];

      let categoryAxisMejor = this.chartMejor.yAxes.push(new am4charts.CategoryAxis());
      categoryAxisMejor.renderer.grid.template.location = 0;
      categoryAxisMejor.dataFields.category = "blank";
      categoryAxisMejor.renderer.minGridDistance = 1;
      categoryAxisMejor.renderer.inversed = true;
      categoryAxisMejor.renderer.grid.template.disabled = true;
      categoryAxisMejor.numberFormatter.numberFormat = "$#,###.##";

      let valueAxisMejor = this.chartMejor.xAxes.push(new am4charts.ValueAxis());
      valueAxisMejor.strictMinMax = true;
      valueAxisMejor.extraMax = 0.05;
      valueAxisMejor.min = 0;
      valueAxisMejor.renderer.opposite = true;
      //valueAxis.renderer.grid.template.disabled = true;
      valueAxis.renderer.minGridDistance = 50;

      //ACTUAL
      let seriesMejor = this.chartMejor.series.push(new am4charts.ColumnSeries());
      seriesMejor.dataFields.categoryY = "blank";
      seriesMejor.dataFields.valueX = "Actual";
      seriesMejor.name = "Actual";
      seriesMejor.columns.template.fill = "#123B4F";
      seriesMejor.columns.template.strokeOpacity = 0;
      seriesMejor.columns.template.height = am4core.percent(100);

      let labelBulletMejor = seriesMejor.bullets.push(new am4charts.LabelBullet())
      labelBulletMejor.label.horizontalCenter = "left";
      labelBulletMejor.label.dx = 20;
      labelBulletMejor.label.text = "{name}: [bold]{Actual}";
      labelBulletMejor.locationX = 1;
      labelBulletMejor.label.fill = "#fff";


      //DIFERENCIA
      let series3Mejor = this.chartMejor.series.push(new am4charts.ColumnSeries());
      series3Mejor.dataFields.categoryY = "blank";
      series3Mejor.dataFields.valueX = "Diferencia";
      series3Mejor.name = "Diferencia";
      series3Mejor.columns.template.fill = "#659F6B";
      series3Mejor.columns.template.strokeOpacity = 0;
      series3Mejor.columns.template.background.fillOpacity = 0.2;
      series3Mejor.columns.template.height = am4core.percent(100);
      series3Mejor.stacked = true;

      series3Mejor.columns.template.adapter.add("fill", function(fill, target) {
        let pattern = new am4core.LinePattern();
        pattern.width = 10;
        pattern.height = 10;
        pattern.strokeWidth = 1;
        pattern.stroke = target.dataItem.dataContext.color;
        pattern.rotation = 45;
        return pattern;
      });

      series3Mejor.columns.template.background.adapter.add("fill", function(fill, target) {
        return target.dataItem ? target.dataItem.dataContext.color : fill;
      });


      //DESCONTADO
      let series2Mejor = this.chartMejor.series.push(new am4charts.ColumnSeries());
      series2Mejor.dataFields.categoryY = "blank";
      series2Mejor.dataFields.valueX = "Target";
      series2Mejor.name = "Descontado";
      series2Mejor.columns.template.fill = "#1D5F80";
      series2Mejor.columns.template.strokeOpacity = 0;
      series2Mejor.columns.template.height = am4core.percent(100);

      labelBullet = series2Mejor.bullets.push(new am4charts.LabelBullet())
      labelBullet.label.horizontalCenter = "left";
      labelBullet.label.dx = 20;
      labelBullet.label.text = "{name}: [bold]{Target}";
      labelBullet.locationX = 1;
      labelBullet.label.fill = "#fff";


      //DIFERENCIA (RANGOS)
      let series4Mejor = this.chartMejor.series.push(new am4charts.StepLineSeries());
      series4Mejor.dataFields.valueX = "Actual";
      series4Mejor.dataFields.categoryY = "blank";
      series4Mejor.strokeWidth = 5;
      series4Mejor.startLocation = 0;
      series4Mejor.endLocation = 1;
      series4Mejor.stroke = "#659F6B";

      let series5Mejor = this.chartMejor.series.push(new am4charts.StepLineSeries());
      series5Mejor.dataFields.valueX = "Target";
      series5Mejor.dataFields.categoryY = "blank";
      series5Mejor.strokeWidth = 5;
      series5Mejor.startLocation = 0;
      series5Mejor.endLocation = 1;
      series5Mejor.stroke = "#659F6B";
      series5Mejor.tooltipText = "[bold, #496548, font-size:25px]{Tooltip.formatNumber('#,###.##s')}%";
      series5Mejor.tooltip.getFillFromObject = false;
      series5Mejor.tooltip.background.fill = "#fff";
      series5Mejor.tooltip.background.stroke = "#496548";
      series5Mejor.tooltip.background.filters.clear();


      // CURSOR
      this.chartMejor.cursor = new am4charts.XYCursor()
      this.chartMejor.cursor.behavior = "none";
      this.chartMejor.cursor.lineX.disabled = true;
      this.chartMejor.cursor.lineY.disabled = true;
      valueAxis.cursorTooltipEnabled = false;

      //NEG
      
      this.chartNegMejor = am4core.create("chartdivNegMejor", am4charts.XYChart);
      this.chartNegMejor.padding(0, 0, 0, 0);
        if (this.chartNegMejor.logo) {
          this.chartNegMejor.logo.dispose()
        }
        this.chartNegMejor.data = [{
            "blank": "",
            "Actual": 0, // Precio
            "Descontado": 0, // Precio
            "Diferencia": 0, //  Precio - Valor estimado
            "Target": 0, // Valor estimado
            "Tooltip": 0,
            "color": am4core.color("#659F6B")
          }];

        var categoryAxisNegMejor = this.chartNegMejor.yAxes.push(new am4charts.CategoryAxis());
        categoryAxisNegMejor.renderer.grid.template.location = 0;
        categoryAxisNegMejor.dataFields.category = "blank";
        categoryAxisNegMejor.renderer.minGridDistance = 1;
        categoryAxisNegMejor.renderer.inversed = true;
        categoryAxisNegMejor.renderer.grid.template.disabled = true;
        categoryAxisNegMejor.numberFormatter.numberFormat = "$#,###.##";

        var valueAxisNegMejor = this.chartNegMejor.xAxes.push(new am4charts.ValueAxis());
        valueAxisNegMejor.strictMinMax = true;
        valueAxisNegMejor.extraMax = 0.05;
        valueAxisNegMejor.min = 0;
        valueAxisNegMejor.renderer.opposite = true;
        //valueAxis.renderer.grid.template.disabled = true;
        valueAxisNegMejor.renderer.minGridDistance = 50;

        //ACTUAL
        var seriesNegMejor = this.chartNegMejor.series.push(new am4charts.ColumnSeries());
        seriesNegMejor.dataFields.categoryY = "blank";
        seriesNegMejor.dataFields.valueX = "Actual";
        seriesNegMejor.name = "Actual";
        seriesNegMejor.columns.template.fill = "#123B4F";
        seriesNegMejor.columns.template.strokeOpacity = 0;
        seriesNegMejor.columns.template.height = am4core.percent(100);

        var labelBulletNegMejor = seriesNegMejor.bullets.push(new am4charts.LabelBullet())
        labelBulletNegMejor.label.horizontalCenter = "left";
        labelBulletNegMejor.label.dx = 20;
        labelBulletNegMejor.label.text = "{name}: [bold]{Actual}";
        labelBulletNegMejor.locationX = 1;
        labelBulletNegMejor.label.fill = "#fff";


        //DESCONTADO
        var series2NegMejor = this.chartNegMejor.series.push(new am4charts.ColumnSeries());
        series2NegMejor.dataFields.categoryY = "blank";
        series2NegMejor.dataFields.valueX = "Target";
        series2NegMejor.name = "Descontado";
        series2NegMejor.columns.template.fill = "#1D5F80";
        series2NegMejor.columns.template.strokeOpacity = 0;
        series2NegMejor.columns.template.height = am4core.percent(100);

        labelBulletNegMejor = series2NegMejor.bullets.push(new am4charts.LabelBullet())
        labelBulletNegMejor.label.horizontalCenter = "left";
        labelBulletNegMejor.label.dx = 20;
        labelBulletNegMejor.label.text = "{name}: [bold]{Target}";
        labelBulletNegMejor.locationX = 1;
        labelBulletNegMejor.label.fill = "#fff";


        //DIFERENCIA
        var series3NegMejor = this.chartNegMejor.series.push(new am4charts.ColumnSeries());
        series3NegMejor.dataFields.categoryY = "blank";
        series3NegMejor.dataFields.valueX = "Diferencia";
        series3NegMejor.name = "Diferencia";
        series3NegMejor.columns.template.fill = "#A63838";
        series3NegMejor.columns.template.strokeOpacity = 0;
        series3NegMejor.columns.template.background.fillOpacity = 0.2;
        series3NegMejor.columns.template.height = am4core.percent(100);
        series3NegMejor.stacked = true;

        series3NegMejor.columns.template.adapter.add("fill", function(fill, target) {
          var pattern = new am4core.LinePattern();
          pattern.width = 10;
          pattern.height = 10;
          pattern.strokeWidth = 1;
          pattern.stroke = target.dataItem.dataContext.color;
          pattern.rotation = 45;
          return pattern;
        });

        series3NegMejor.columns.template.background.adapter.add("fill", function(fill, target) {
          return target.dataItem ? target.dataItem.dataContext.color : fill;
        });

        //DIFERENCIA (RANGOS)
        var series4NegMejor = this.chartNegMejor.series.push(new am4charts.StepLineSeries());
        series4NegMejor.dataFields.categoryY = "blank";
        series4NegMejor.dataFields.valueX = "Target";
        series4NegMejor.strokeWidth = 5;
        series4NegMejor.startLocation = 0;
        series4NegMejor.endLocation = 1;
        series4NegMejor.stroke = "#A63838";

        var series5NegMejor = this.chartNegMejor.series.push(new am4charts.StepLineSeries());
        series5NegMejor.dataFields.categoryY = "blank";
        series5NegMejor.dataFields.valueX = "Actual";
        series5NegMejor.strokeWidth = 5;
        series5NegMejor.startLocation = 0;
        series5NegMejor.endLocation = 1;
        series5NegMejor.stroke = "#A63838";
        series5NegMejor.tooltipText = "[bold, #6A2C2A, font-size:25px]{Tooltip.formatNumber('#,###.##s')}%";
        series5NegMejor.tooltip.getFillFromObject = false;
        series5NegMejor.tooltip.background.fill = "#fff";
        series5NegMejor.tooltip.background.stroke = "#6A2C2A";
        series5NegMejor.tooltip.background.filters.clear();


        // CURSOR
        this.chartNegMejor.cursor = new am4charts.XYCursor()
        this.chartNegMejor.cursor.behavior = "none";
        this.chartNegMejor.cursor.lineX.disabled = true;
        this.chartNegMejor.cursor.lineY.disabled = true;
        valueAxisNegMejor.cursorTooltipEnabled = false;
      
      // PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR
      // PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR
      // PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR
      // PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR
      // PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR PEOR
      this.chartPeor = am4core.create("chartdivPeor", am4charts.XYChart);
      this.chartPeor.padding(0, 0, 0, 0);
        if (this.chartPeor.logo) {
          this.chartPeor.logo.dispose()
        }
        this.chartPeor.data = [{
            "blank": "",
            "Actual": 0, // Precio
            "Descontado": 0, // Precio
            "Diferencia": 0, // Valor estimado - Precio
            "Target": 0, // Valor estimado
            "Tooltip": 0,
            "color": am4core.color("#659F6B")
          }];

      let categoryAxisPeor = this.chartPeor.yAxes.push(new am4charts.CategoryAxis());
      categoryAxisPeor.renderer.grid.template.location = 0;
      categoryAxisPeor.dataFields.category = "blank";
      categoryAxisPeor.renderer.minGridDistance = 1;
      categoryAxisPeor.renderer.inversed = true;
      categoryAxisPeor.renderer.grid.template.disabled = true;
      categoryAxisPeor.numberFormatter.numberFormat = "$#,###.##";

      let valueAxisPeor = this.chartPeor.xAxes.push(new am4charts.ValueAxis());
      valueAxisPeor.strictMinMax = true;
      valueAxisPeor.extraMax = 0.05;
      valueAxisPeor.min = 0;
      valueAxisPeor.renderer.opposite = true;
      //valueAxis.renderer.grid.template.disabled = true;
      valueAxis.renderer.minGridDistance = 50;

      //ACTUAL
      let seriesPeor = this.chartPeor.series.push(new am4charts.ColumnSeries());
      seriesPeor.dataFields.categoryY = "blank";
      seriesPeor.dataFields.valueX = "Actual";
      seriesPeor.name = "Actual";
      seriesPeor.columns.template.fill = "#123B4F";
      seriesPeor.columns.template.strokeOpacity = 0;
      seriesPeor.columns.template.height = am4core.percent(100);

      let labelBulletPeor = seriesPeor.bullets.push(new am4charts.LabelBullet())
      labelBulletPeor.label.horizontalCenter = "left";
      labelBulletPeor.label.dx = 20;
      labelBulletPeor.label.text = "{name}: [bold]{Actual}";
      labelBulletPeor.locationX = 1;
      labelBulletPeor.label.fill = "#fff";


      //DIFERENCIA
      let series3Peor = this.chartPeor.series.push(new am4charts.ColumnSeries());
      series3Peor.dataFields.categoryY = "blank";
      series3Peor.dataFields.valueX = "Diferencia";
      series3Peor.name = "Diferencia";
      series3Peor.columns.template.fill = "#659F6B";
      series3Peor.columns.template.strokeOpacity = 0;
      series3Peor.columns.template.background.fillOpacity = 0.2;
      series3Peor.columns.template.height = am4core.percent(100);
      series3Peor.stacked = true;

      series3Peor.columns.template.adapter.add("fill", function(fill, target) {
        let pattern = new am4core.LinePattern();
        pattern.width = 10;
        pattern.height = 10;
        pattern.strokeWidth = 1;
        pattern.stroke = target.dataItem.dataContext.color;
        pattern.rotation = 45;
        return pattern;
      });

      series3Peor.columns.template.background.adapter.add("fill", function(fill, target) {
        return target.dataItem ? target.dataItem.dataContext.color : fill;
      });


      //DESCONTADO
      let series2Peor = this.chartPeor.series.push(new am4charts.ColumnSeries());
      series2Peor.dataFields.categoryY = "blank";
      series2Peor.dataFields.valueX = "Target";
      series2Peor.name = "Descontado";
      series2Peor.columns.template.fill = "#1D5F80";
      series2Peor.columns.template.strokeOpacity = 0;
      series2Peor.columns.template.height = am4core.percent(100);

      labelBullet = series2Peor.bullets.push(new am4charts.LabelBullet())
      labelBullet.label.horizontalCenter = "left";
      labelBullet.label.dx = 20;
      labelBullet.label.text = "{name}: [bold]{Target}";
      labelBullet.locationX = 1;
      labelBullet.label.fill = "#fff";


      //DIFERENCIA (RANGOS)
      let series4Peor = this.chartPeor.series.push(new am4charts.StepLineSeries());
      series4Peor.dataFields.valueX = "Actual";
      series4Peor.dataFields.categoryY = "blank";
      series4Peor.strokeWidth = 5;
      series4Peor.startLocation = 0;
      series4Peor.endLocation = 1;
      series4Peor.stroke = "#659F6B";

      let series5Peor = this.chartPeor.series.push(new am4charts.StepLineSeries());
      series5Peor.dataFields.valueX = "Target";
      series5Peor.dataFields.categoryY = "blank";
      series5Peor.strokeWidth = 5;
      series5Peor.startLocation = 0;
      series5Peor.endLocation = 1;
      series5Peor.stroke = "#659F6B";
      series5Peor.tooltipText = "[bold, #496548, font-size:25px]{Tooltip.formatNumber('#,###.##s')}%";
      series5Peor.tooltip.getFillFromObject = false;
      series5Peor.tooltip.background.fill = "#fff";
      series5Peor.tooltip.background.stroke = "#496548";
      series5Peor.tooltip.background.filters.clear();


      // CURSOR
      this.chartPeor.cursor = new am4charts.XYCursor()
      this.chartPeor.cursor.behavior = "none";
      this.chartPeor.cursor.lineX.disabled = true;
      this.chartPeor.cursor.lineY.disabled = true;
      valueAxis.cursorTooltipEnabled = false;
      
      //NEG
      this.chartNegPeor = am4core.create("chartdivNegPeor", am4charts.XYChart);
      this.chartNegPeor.padding(0, 0, 0, 0);
        if (this.chartNegPeor.logo) {
          this.chartNegPeor.logo.dispose()
        }
        this.chartNegPeor.data = [{
            "blank": "",
            "Actual": 0, // Precio
            "Descontado": 0, // Precio
            "Diferencia": 0, //  Precio - Valor estimado
            "Target": 0, // Valor estimado
            "Tooltip": 0,
            "color": am4core.color("#659F6B")
          }];

        var categoryAxisNegPeor = this.chartNegPeor.yAxes.push(new am4charts.CategoryAxis());
        categoryAxisNegPeor.renderer.grid.template.location = 0;
        categoryAxisNegPeor.dataFields.category = "blank";
        categoryAxisNegPeor.renderer.minGridDistance = 1;
        categoryAxisNegPeor.renderer.inversed = true;
        categoryAxisNegPeor.renderer.grid.template.disabled = true;
        categoryAxisNegPeor.numberFormatter.numberFormat = "$#,###.##";

        var valueAxisNegPeor = this.chartNegPeor.xAxes.push(new am4charts.ValueAxis());
        valueAxisNegPeor.strictMinMax = true;
        valueAxisNegPeor.extraMax = 0.05;
        valueAxisNegPeor.min = 0;
        valueAxisNegPeor.renderer.opposite = true;
        //valueAxis.renderer.grid.template.disabled = true;
        valueAxisNegPeor.renderer.minGridDistance = 50;

        //ACTUAL
        var seriesNegPeor = this.chartNegPeor.series.push(new am4charts.ColumnSeries());
        seriesNegPeor.dataFields.categoryY = "blank";
        seriesNegPeor.dataFields.valueX = "Actual";
        seriesNegPeor.name = "Actual";
        seriesNegPeor.columns.template.fill = "#123B4F";
        seriesNegPeor.columns.template.strokeOpacity = 0;
        seriesNegPeor.columns.template.height = am4core.percent(100);

        var labelBulletNegPeor = seriesNegPeor.bullets.push(new am4charts.LabelBullet())
        labelBulletNegPeor.label.horizontalCenter = "left";
        labelBulletNegPeor.label.dx = 20;
        labelBulletNegPeor.label.text = "{name}: [bold]{Actual}";
        labelBulletNegPeor.locationX = 1;
        labelBulletNegPeor.label.fill = "#fff";


        //DESCONTADO
        var series2NegPeor = this.chartNegPeor.series.push(new am4charts.ColumnSeries());
        series2NegPeor.dataFields.categoryY = "blank";
        series2NegPeor.dataFields.valueX = "Target";
        series2NegPeor.name = "Descontado";
        series2NegPeor.columns.template.fill = "#1D5F80";
        series2NegPeor.columns.template.strokeOpacity = 0;
        series2NegPeor.columns.template.height = am4core.percent(100);

        labelBulletNegPeor = series2NegPeor.bullets.push(new am4charts.LabelBullet())
        labelBulletNegPeor.label.horizontalCenter = "left";
        labelBulletNegPeor.label.dx = 20;
        labelBulletNegPeor.label.text = "{name}: [bold]{Target}";
        labelBulletNegPeor.locationX = 1;
        labelBulletNegPeor.label.fill = "#fff";


        //DIFERENCIA
        var series3NegPeor = this.chartNegPeor.series.push(new am4charts.ColumnSeries());
        series3NegPeor.dataFields.categoryY = "blank";
        series3NegPeor.dataFields.valueX = "Diferencia";
        series3NegPeor.name = "Diferencia";
        series3NegPeor.columns.template.fill = "#A63838";
        series3NegPeor.columns.template.strokeOpacity = 0;
        series3NegPeor.columns.template.background.fillOpacity = 0.2;
        series3NegPeor.columns.template.height = am4core.percent(100);
        series3NegPeor.stacked = true;

        series3NegPeor.columns.template.adapter.add("fill", function(fill, target) {
          var pattern = new am4core.LinePattern();
          pattern.width = 10;
          pattern.height = 10;
          pattern.strokeWidth = 1;
          pattern.stroke = target.dataItem.dataContext.color;
          pattern.rotation = 45;
          return pattern;
        });

        series3NegPeor.columns.template.background.adapter.add("fill", function(fill, target) {
          return target.dataItem ? target.dataItem.dataContext.color : fill;
        });

        //DIFERENCIA (RANGOS)
        var series4NegPeor = this.chartNegPeor.series.push(new am4charts.StepLineSeries());
        series4NegPeor.dataFields.categoryY = "blank";
        series4NegPeor.dataFields.valueX = "Target";
        series4NegPeor.strokeWidth = 5;
        series4NegPeor.startLocation = 0;
        series4NegPeor.endLocation = 1;
        series4NegPeor.stroke = "#A63838";

        var series5NegPeor = this.chartNegPeor.series.push(new am4charts.StepLineSeries());
        series5NegPeor.dataFields.categoryY = "blank";
        series5NegPeor.dataFields.valueX = "Actual";
        series5NegPeor.strokeWidth = 5;
        series5NegPeor.startLocation = 0;
        series5NegPeor.endLocation = 1;
        series5NegPeor.stroke = "#A63838";
        series5NegPeor.tooltipText = "[bold, #6A2C2A, font-size:25px]{Tooltip.formatNumber('#,###.##s')}%";
        series5NegPeor.tooltip.getFillFromObject = false;
        series5NegPeor.tooltip.background.fill = "#fff";
        series5NegPeor.tooltip.background.stroke = "#6A2C2A";
        series5NegPeor.tooltip.background.filters.clear();


        // CURSOR
        this.chartNegPeor.cursor = new am4charts.XYCursor()
        this.chartNegPeor.cursor.behavior = "none";
        this.chartNegPeor.cursor.lineX.disabled = true;
        this.chartNegPeor.cursor.lineY.disabled = true;
        valueAxisNegPeor.cursorTooltipEnabled = false;

      // PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL
      // PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL
      // PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL
      // PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL PRINCIPAL
      
      this.chartPrincipal = am4core.create("chartdivPrincipal", am4charts.XYChart);
      this.chartPrincipal.padding(0, 0, 0, 0);
        if (this.chartPrincipal.logo) {
          this.chartPrincipal.logo.dispose()
        }
        this.chartPrincipal.data = [{
            "blank": "",
            "Actual": 0, // Precio
            "Descontado": 0, // Precio
            "Diferencia": 0, // Valor estimado - Precio
            "Target": 0, // Valor estimado
            "Tooltip": 0,
            "color": am4core.color("#659F6B")
          }];

      let categoryAxisPrincipal = this.chartPrincipal.yAxes.push(new am4charts.CategoryAxis());
      categoryAxisPrincipal.renderer.grid.template.location = 0;
      categoryAxisPrincipal.dataFields.category = "blank";
      categoryAxisPrincipal.renderer.minGridDistance = 1;
      categoryAxisPrincipal.renderer.inversed = true;
      categoryAxisPrincipal.renderer.grid.template.disabled = true;
      categoryAxisPrincipal.numberFormatter.numberFormat = "$#,###.##";

      let valueAxisPrincipal = this.chartPrincipal.xAxes.push(new am4charts.ValueAxis());
      valueAxisPrincipal.strictMinMax = true;
      valueAxisPrincipal.extraMax = 0.05;
      valueAxisPrincipal.min = 0;
      valueAxisPrincipal.renderer.opposite = true;
      //valueAxis.renderer.grid.template.disabled = true;
      valueAxis.renderer.minGridDistance = 50;

      //ACTUAL
      let seriesPrincipal = this.chartPrincipal.series.push(new am4charts.ColumnSeries());
      seriesPrincipal.dataFields.categoryY = "blank";
      seriesPrincipal.dataFields.valueX = "Actual";
      seriesPrincipal.name = "Actual";
      seriesPrincipal.columns.template.fill = "#123B4F";
      seriesPrincipal.columns.template.strokeOpacity = 0;
      seriesPrincipal.columns.template.height = am4core.percent(100);

      let labelBulletPrincipal = seriesPrincipal.bullets.push(new am4charts.LabelBullet())
      labelBulletPrincipal.label.horizontalCenter = "left";
      labelBulletPrincipal.label.dx = 20;
      labelBulletPrincipal.label.text = "{name}: [bold]{Actual}";
      labelBulletPrincipal.locationX = 1;
      labelBulletPrincipal.label.fill = "#fff";


      //DIFERENCIA
      let series3Principal = this.chartPrincipal.series.push(new am4charts.ColumnSeries());
      series3Principal.dataFields.categoryY = "blank";
      series3Principal.dataFields.valueX = "Diferencia";
      series3Principal.name = "Diferencia";
      series3Principal.columns.template.fill = "#659F6B";
      series3Principal.columns.template.strokeOpacity = 0;
      series3Principal.columns.template.background.fillOpacity = 0.2;
      series3Principal.columns.template.height = am4core.percent(100);
      series3Principal.stacked = true;

      series3Principal.columns.template.adapter.add("fill", function(fill, target) {
        let pattern = new am4core.LinePattern();
        pattern.width = 10;
        pattern.height = 10;
        pattern.strokeWidth = 1;
        pattern.stroke = target.dataItem.dataContext.color;
        pattern.rotation = 45;
        return pattern;
      });

      series3Principal.columns.template.background.adapter.add("fill", function(fill, target) {
        return target.dataItem ? target.dataItem.dataContext.color : fill;
      });


      //DESCONTADO
      let series2Principal = this.chartPrincipal.series.push(new am4charts.ColumnSeries());
      series2Principal.dataFields.categoryY = "blank";
      series2Principal.dataFields.valueX = "Target";
      series2Principal.name = "Descontado";
      series2Principal.columns.template.fill = "#1D5F80";
      series2Principal.columns.template.strokeOpacity = 0;
      series2Principal.columns.template.height = am4core.percent(100);

      labelBullet = series2Principal.bullets.push(new am4charts.LabelBullet())
      labelBullet.label.horizontalCenter = "left";
      labelBullet.label.dx = 20;
      labelBullet.label.text = "{name}: [bold]{Target}";
      labelBullet.locationX = 1;
      labelBullet.label.fill = "#fff";


      //DIFERENCIA (RANGOS)
      let series4Principal = this.chartPrincipal.series.push(new am4charts.StepLineSeries());
      series4Principal.dataFields.valueX = "Actual";
      series4Principal.dataFields.categoryY = "blank";
      series4Principal.strokeWidth = 5;
      series4Principal.startLocation = 0;
      series4Principal.endLocation = 1;
      series4Principal.stroke = "#659F6B";

      let series5Principal = this.chartPrincipal.series.push(new am4charts.StepLineSeries());
      series5Principal.dataFields.valueX = "Target";
      series5Principal.dataFields.categoryY = "blank";
      series5Principal.strokeWidth = 5;
      series5Principal.startLocation = 0;
      series5Principal.endLocation = 1;
      series5Principal.stroke = "#659F6B";
      series5Principal.tooltipText = "[bold, #496548, font-size:25px]{Tooltip.formatNumber('#,###.##s')}%";
      series5Principal.tooltip.getFillFromObject = false;
      series5Principal.tooltip.background.fill = "#fff";
      series5Principal.tooltip.background.stroke = "#496548";
      series5Principal.tooltip.background.filters.clear();


      // CURSOR
      this.chartPrincipal.cursor = new am4charts.XYCursor()
      this.chartPrincipal.cursor.behavior = "none";
      this.chartPrincipal.cursor.lineX.disabled = true;
      this.chartPrincipal.cursor.lineY.disabled = true;
      valueAxis.cursorTooltipEnabled = false;
      
      //NEG
      
      this.chartNegPrincipal = am4core.create("chartdivNegPrincipal", am4charts.XYChart);
      this.chartNegPrincipal.padding(0, 0, 0, 0);
        if (this.chartNegPrincipal.logo) {
          this.chartNegPrincipal.logo.dispose()
        }
        this.chartNegPrincipal.data = [{
            "blank": "",
            "Actual": 0, // Precio
            "Descontado": 0, // Precio
            "Diferencia": 0, //  Precio - Valor estimado
            "Target": 0, // Valor estimado
            "Tooltip": 0,
            "color": am4core.color("#659F6B")
          }];

        var categoryAxisNegPrincipal = this.chartNegPrincipal.yAxes.push(new am4charts.CategoryAxis());
        categoryAxisNegPrincipal.renderer.grid.template.location = 0;
        categoryAxisNegPrincipal.dataFields.category = "blank";
        categoryAxisNegPrincipal.renderer.minGridDistance = 1;
        categoryAxisNegPrincipal.renderer.inversed = true;
        categoryAxisNegPrincipal.renderer.grid.template.disabled = true;
        categoryAxisNegPrincipal.numberFormatter.numberFormat = "$#,###.##";

        var valueAxisNegPrincipal = this.chartNegPrincipal.xAxes.push(new am4charts.ValueAxis());
        valueAxisNegPrincipal.strictMinMax = true;
        valueAxisNegPrincipal.extraMax = 0.05;
        valueAxisNegPrincipal.min = 0;
        valueAxisNegPrincipal.renderer.opposite = true;
        //valueAxis.renderer.grid.template.disabled = true;
        valueAxisNegPrincipal.renderer.minGridDistance = 50;

        //ACTUAL
        var seriesNegPrincipal = this.chartNegPrincipal.series.push(new am4charts.ColumnSeries());
        seriesNegPrincipal.dataFields.categoryY = "blank";
        seriesNegPrincipal.dataFields.valueX = "Actual";
        seriesNegPrincipal.name = "Actual";
        seriesNegPrincipal.columns.template.fill = "#123B4F";
        seriesNegPrincipal.columns.template.strokeOpacity = 0;
        seriesNegPrincipal.columns.template.height = am4core.percent(100);

        var labelBulletNegPrincipal = seriesNegPrincipal.bullets.push(new am4charts.LabelBullet())
        labelBulletNegPrincipal.label.horizontalCenter = "left";
        labelBulletNegPrincipal.label.dx = 20;
        labelBulletNegPrincipal.label.text = "{name}: [bold]{Actual}";
        labelBulletNegPrincipal.locationX = 1;
        labelBulletNegPrincipal.label.fill = "#fff";


        //DESCONTADO
        var series2NegPrincipal = this.chartNegPrincipal.series.push(new am4charts.ColumnSeries());
        series2NegPrincipal.dataFields.categoryY = "blank";
        series2NegPrincipal.dataFields.valueX = "Target";
        series2NegPrincipal.name = "Descontado";
        series2NegPrincipal.columns.template.fill = "#1D5F80";
        series2NegPrincipal.columns.template.strokeOpacity = 0;
        series2NegPrincipal.columns.template.height = am4core.percent(100);

        labelBulletNegPrincipal = series2NegPrincipal.bullets.push(new am4charts.LabelBullet())
        labelBulletNegPrincipal.label.horizontalCenter = "left";
        labelBulletNegPrincipal.label.dx = 20;
        labelBulletNegPrincipal.label.text = "{name}: [bold]{Target}";
        labelBulletNegPrincipal.locationX = 1;
        labelBulletNegPrincipal.label.fill = "#fff";


        //DIFERENCIA
        var series3NegPrincipal = this.chartNegPrincipal.series.push(new am4charts.ColumnSeries());
        series3NegPrincipal.dataFields.categoryY = "blank";
        series3NegPrincipal.dataFields.valueX = "Diferencia";
        series3NegPrincipal.name = "Diferencia";
        series3NegPrincipal.columns.template.fill = "#A63838";
        series3NegPrincipal.columns.template.strokeOpacity = 0;
        series3NegPrincipal.columns.template.background.fillOpacity = 0.2;
        series3NegPrincipal.columns.template.height = am4core.percent(100);
        series3NegPrincipal.stacked = true;

        series3NegPrincipal.columns.template.adapter.add("fill", function(fill, target) {
          var pattern = new am4core.LinePattern();
          pattern.width = 10;
          pattern.height = 10;
          pattern.strokeWidth = 1;
          pattern.stroke = target.dataItem.dataContext.color;
          pattern.rotation = 45;
          return pattern;
        });

        series3NegPrincipal.columns.template.background.adapter.add("fill", function(fill, target) {
          return target.dataItem ? target.dataItem.dataContext.color : fill;
        });

        //DIFERENCIA (RANGOS)
        var series4NegPrincipal = this.chartNegPrincipal.series.push(new am4charts.StepLineSeries());
        series4NegPrincipal.dataFields.categoryY = "blank";
        series4NegPrincipal.dataFields.valueX = "Target";
        series4NegPrincipal.strokeWidth = 5;
        series4NegPrincipal.startLocation = 0;
        series4NegPrincipal.endLocation = 1;
        series4NegPrincipal.stroke = "#A63838";

        var series5NegPrincipal = this.chartNegPrincipal.series.push(new am4charts.StepLineSeries());
        series5NegPrincipal.dataFields.categoryY = "blank";
        series5NegPrincipal.dataFields.valueX = "Actual";
        series5NegPrincipal.strokeWidth = 5;
        series5NegPrincipal.startLocation = 0;
        series5NegPrincipal.endLocation = 1;
        series5NegPrincipal.stroke = "#A63838";
        series5NegPrincipal.tooltipText = "[bold, #6A2C2A, font-size:25px]{Tooltip.formatNumber('#,###.##s')}%";
        series5NegPrincipal.tooltip.getFillFromObject = false;
        series5NegPrincipal.tooltip.background.fill = "#fff";
        series5NegPrincipal.tooltip.background.stroke = "#6A2C2A";
        series5NegPrincipal.tooltip.background.filters.clear();


        // CURSOR
        this.chartNegPrincipal.cursor = new am4charts.XYCursor()
        this.chartNegPrincipal.cursor.behavior = "none";
        this.chartNegPrincipal.cursor.lineX.disabled = true;
        this.chartNegPrincipal.cursor.lineY.disabled = true;
        valueAxisNegPrincipal.cursorTooltipEnabled = false;
      
  },
}

</script>

<style>
  :root {
    --bg: #08080e;
    --bg2: #0f0f18;
    --bg3: #14141e;
    --border: rgba(255,255,255,0.07);
    --border-bright: rgba(255,255,255,0.15);
    --text: #eeeef6;
    --muted: #6b6b80;
    --accent: #7c6af7;
    --accent2: #4ed8c0;
    --accent3: #f06fba;
  }

  body { background: var(--bg) !important; }

  .v-application { background: var(--bg) !important; font-family: 'Syne', sans-serif !important; }

  input {
    font-family: 'Syne', sans-serif !important;
    color: var(--text) !important;
    background-color: transparent !important;
  }

  .v-select .v-select__selection-text { font-family: 'Syne', sans-serif !important; color: var(--text) !important; }

  .monthly-payment-mount {
    font-family: 'Raleway', sans-serif !important;
    font-weight: 800 !important;
    font-size: 2.2rem !important;
    color: var(--text) !important;
    line-height: 1.1 !important;
  }

  .monthly-payment-title {
    font-family: 'Space Mono', monospace !important;
    text-transform: uppercase !important;
    font-weight: 400 !important;
    font-size: 11px !important;
    letter-spacing: 0.12em !important;
    color: var(--muted) !important;
    margin-bottom: 1rem !important;
    margin-top: 4px !important;
  }

  .monthly-payment-line {
    width: 32px !important;
    border-width: 2px !important;
    margin-bottom: 6px !important;
    margin-top: 12px !important;
    background: linear-gradient(90deg, var(--accent), var(--accent2)) !important;
    border-color: transparent !important;
    opacity: 1 !important;
  }

  .monthly-payment-line-title {
    width: 32px !important;
    border-width: 2px !important;
    margin-bottom: 20px !important;
    margin-top: 8px !important;
    background: linear-gradient(90deg, var(--accent), var(--accent2)) !important;
    border-color: transparent !important;
    opacity: 1 !important;
  }

  .titlepie {
    font-family: 'Raleway', sans-serif !important;
    text-transform: uppercase !important;
    font-weight: 800 !important;
    font-size: 12px !important;
    letter-spacing: 0.18em !important;
    color: var(--muted) !important;
    margin-bottom: -20px !important;
  }

  .v-card { background: var(--bg2) !important; border: 1px solid var(--border) !important; box-shadow: none !important; }

  .v-tabs { background: var(--bg3) !important; border-bottom: 1px solid var(--border) !important; }

  .v-tab {
    font-family: 'Space Mono', monospace !important;
    font-size: 11px !important;
    text-transform: uppercase !important;
    letter-spacing: 0.08em !important;
    color: var(--muted) !important;
  }

  .v-tab--selected { color: var(--accent2) !important; }

  .v-tab__slider {
    background: linear-gradient(90deg, var(--accent), var(--accent2)) !important;
    height: 2px !important;
  }

  .v-table { background: var(--bg2) !important; color: var(--text) !important; }

  .v-table thead tr th, .v-table th {
    font-family: 'Space Mono', monospace !important;
    font-size: 10px !important;
    text-transform: uppercase !important;
    letter-spacing: 0.08em !important;
    color: var(--muted) !important;
    background: var(--bg3) !important;
    border-bottom: 1px solid var(--border) !important;
    text-align: center !important;
    width: 150px !important;
  }

  .v-table tbody tr td, .v-table td, .v-table tr {
    font-family: 'Syne', sans-serif !important;
    font-size: 13px !important;
    color: var(--text) !important;
    border-bottom: 1px solid var(--border) !important;
    text-align: center !important;
  }

  .v-table tbody tr:hover td { background: rgba(124,106,247,0.08) !important; }

  .v-table table tr:hover { background-color: rgba(124,106,247,0.08) !important; color: var(--text) !important; }

  .calculator-investment { line-height: 0.5em; }

  .col-style { padding-bottom: 0 !important; padding-top: 0 !important; }

  .v-field__input { max-height: 44px !important; }

  .derecha { text-align: right !important; }

  .underline, .underline2 { text-decoration: none; position: relative; }

  .underline:after {
    position: absolute; content: ''; height: 3px;
    bottom: 12px; left: 0; width: 32px;
    background: linear-gradient(90deg, var(--accent), var(--accent2));
  }

  .underline2:after {
    position: absolute; content: ''; height: 3px;
    bottom: 12px; right: 0; width: 32px;
    background: linear-gradient(90deg, var(--accent3), var(--accent));
  }

  .pegajoso { position: sticky; top: 45px; }

  .botones { margin: auto; }

  .v-table--density-default > .v-table__wrapper > table > tbody > tr > td,
  .v-table--density-default > .v-table__wrapper > table > thead > tr > td,
  .v-table--density-default > .v-table__wrapper > table > tfoot > tr > td {
    height: 40px !important;
  }
</style>