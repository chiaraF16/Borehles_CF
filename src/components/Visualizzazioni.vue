<template>
    <div style="position: static; padding: 5px; width: 80%; height: 400px;">
        <div style="position: relative; width: 80%; padding:10px; border: 1px solidblue;" class="graph" id="locChart">Età relativa dei materiali litologici</div>
            <div style="margin: 5px;" v-bind="selected" id="select1">
                <div style="display: felx:">
                    <a class='reset'
                        href='javascript:select1.filterAll();dc.redrawAll();'
                        style='visibility: hidden;'>reset</a>
                </div>
            </div> 
            <div style="border: 1px solidblue;">
                <div style="display: flex;">
                    <div id="test">Litologie rilevate</div> 
                    <div>
                        <div id="select2">
                            <div>
                            <a class='reset'
                                href='javascript:select2.filterAll();dc.redrawAll();'
                                style='visibility: hidden;'>reset</a>
                            </div>
                        </div> 
                        <div style="display: flex;">  
                            <div id="pie2">Temperature rilevate</div>
                            <div style="padding: 10px; width: 50%;" class="graph" id="dataTable"></div>
                        </div>
                    </div>
                </div>
            </div> 
        <div style="display: flex; border: 1px solidblue;">
           <div id="select3">
                        <div>
                        <a class='reset'
                            href='javascript:select3.filterAll();dc.redrawAll();'
                            style='visibility: hidden;'>reset</a>
                        </div>
            </div>    
            <div id="pie">Esiti dei pozzi in base al tipo</div>
            <div id="bar">Bar Plot</div>
        </div> 
        
    </div>
</template>

<script>
import data from '../data/pozzi4326.json'
import litodata from '../data/litologia.json'
import tempdata from '../data/temperature.json'
import crossfilter from 'crossfilter'
import * as d3 from 'd3'
import * as dc from 'dc';

let cf
let cf1
let cf2
let dTipo 
let gTipo 
let dPozzo
let dDaprof
let gPozzo
let gDaprof
let dEsito
let gEsito
let dNome
let gNome
let mirrorNome
let mirrorNome1
let dNome1
let dTemp
let dEtarel
let gEtarel
let gTemp
let dScopo
let gScopo
export default {
    props:['selected'],
    components: {
    },
        
    data () {
        return{
            dataMaps : data, 
            litoMaps : litodata,
            tempMaps : tempdata,
            centerCoord : [],
            detailInformation: [],
            keyMaps : 0,
            keyDetail : 0,
            sel : this.selected,
        }

    },
    methods: {
        openGraphics(val){
          console.log(val)
          document.getElementById("mySidebarLeft").style.display = "unset";
        },
        passData(val, eye){
            this.detailInformation = []
            this.keyMaps = val
            for(var i=0; i< this.dataMaps.length; i++){
                if(this.dataMaps[i]['key'] == val){
                    this.centerCoord = this.dataMaps[i]['latLan']
                    this.detailInformation.push(this.dataMaps[i])
                    break
                }
            }
            if(eye){
                this.openNav()
            }
        },
        passData2(val, eye){
            this.detailInformation = []
            this.keyMaps = val
            for(var i=0; i< this.litoMaps.length; i++){
                if(this.dataMaps[i]['key'] == val){
                    this.centerCoord = this.dataMaps[i]['latLan']
                    this.detailInformation.push(this.dataMaps[i])
                    break
                }
            }
            if(eye){
                this.openNav()
            }
        },
        passData3(val, eye){
            this.detailInformation = []
            this.keyMaps = val
            for(var i=0; i< this.tempMaps.length; i++){
                if(this.dataMaps[i]['key'] == val){
                    this.centerCoord = this.dataMaps[i]['latLan']
                    this.detailInformation.push(this.dataMaps[i])
                    break
                }
            }
            if(eye){
                this.openNav()
            }
        },
        openNav(){
            document.getElementById("mySidebar").style.display = "unset";
        },
        creategraph(){
            this.locChart=dc.rowChart('#locChart')
                .dimension(this.mirror_dimension(dEtarel, mirrorNome1))
                .group(gEtarel)
                .elasticX(true)
                .data((group)=>{return group.top(12)})
            this.dataTable=dc.dataTable('#dataTable')
                .dimension(dEtarel)
                .columns(['nomeunita1'])
            this.select1=dc.selectMenu('#select1')
                .dimension(this.mirror_dimension(dNome,mirrorNome1))
                .group(gNome)
                .controlsUseVisibility(true)
            this.select2=dc.selectMenu('#select2')
                .dimension(this.mirror_dimension(dNome,mirrorNome1))
                .group(gNome)
                .controlsUseVisibility(true)
            this.select3=dc.selectMenu('#select3')
                .dimension(this.mirror_dimension(dTipo))
                .group(gTipo)
                .controlsUseVisibility(true)
            this.chart=dc.pieChart('#test')
                .width(768)
                .height(480)
                .slicesCap(4)
                .innerRadius(100)
                .dimension(dPozzo)
                .group(gPozzo)
                .legend(dc.legend().highlightSelected(true))
            this.chart2=dc.pieChart('#pie2')
                .width(768)
                .height(480)
                .slicesCap(4)
                .innerRadius(100)
                .dimension(dTemp)
                .group(gTemp)
                .legend(dc.legend().highlightSelected(true))
            this.chart1=dc.pieChart('#pie')
                .width(768)
                .height(480)
                .slicesCap(4)
                .innerRadius(100)
                .dimension(dEsito)
                .group(gEsito)
                .legend(dc.legend().highlightSelected(true))
            this.barchart=dc.barChart('#bar')
                .width(768)
                .height(480)
                .x(d3.scaleBand())
                .xUnits(dc.units.ordinal)
                .brushOn(false)
                .yAxisLabel("Utilizzo dei pozzi")
                .dimension(dScopo)
                .group(gScopo)
                
            dc.renderAll()
        
        },
        mirror_dimension() {
             var dims = Array.prototype.slice.call(arguments, 0);
                function mirror(fname) {
                    return function(v) {
                        dims.forEach(function(dim) {
                            dim[fname](v);
                        });
                    };
                }
                return {
                    filter: mirror('filter'),
                    filterExact: mirror('filterExact'),
                    filterRange: mirror('filterRange'),
                    filterFunction: mirror('filterFunction')
                };
            },

        infoData(val, eye){
            this.detailInformation = []
            this.keyMaps = val
            for(var i=0; i< this.dataMaps.length; i++){
                if(this.dataMaps[i]['key'] == val){
                    this.detailInformation.push(this.dataMaps[i])
                    break
                }
            }
            if(eye){
                this.openNav()
            }
        },
    },
    created: function(){
    },
    computed:{

    },
    mounted: function(){
        cf=crossfilter(this.litoMaps)
        cf1=crossfilter(this.dataMaps)
        cf2=crossfilter(this.tempMaps)
        dPozzo=cf.dimension(d=>d.litologia)
        dDaprof=cf.dimension(d=>d.daprof)
        gPozzo=dPozzo.group()
        dEtarel=cf.dimension(d=>d.etarel)
        gEtarel=dEtarel.group()
        gDaprof=dDaprof.group()
        dTipo=cf1.dimension(d=>d.tipo)
        gTipo=dTipo.group()
        dEsito=cf1.dimension(d=>d.esito)
        gEsito=dEsito.group()
        dNome=cf2.dimension(d=>d.nome)
        gNome=dNome.group()
        dScopo=cf1.dimension(d=>d.scopo)
        gScopo=dScopo.group()
        mirrorNome=cf2.dimension(d=>d.nome)
        dNome1=cf1.dimension(d=>d.nome)
        mirrorNome1=cf.dimension(d=>d.nome)
        //mirrorNome2=cf1.dimension(d=>d.nome)
        dTemp=cf2.dimension(d=>d.temp)
        gTemp=dTemp.group()
        this.creategraph()
        console.log(gDaprof)
        console.log(mirrorNome)
        console.log(dNome1)
        console.log(dTipo)
        console.log(gTipo)
        console.log(gEsito)
        console.log(gTemp)
        console.log(gScopo)
        console.log(this.sel)

    },
    watch:{
        centerCoord: function(){
            this.centerCoord
        },
        detailInformation : function(){
            this.detailInformation
        },
        keyMaps: function(){
            this.keyMaps
        },
        openNavGraphics: function(){
            this.openNavGraphics
        },
        sel: function(){
            this.sel
        }
    }
}
</script>