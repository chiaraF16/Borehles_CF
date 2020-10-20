<template>
    <div style="position: static; padding: 5px;">
        <div style="height: 600px;position: relative; width:100%;" id="maps">
            <Graphics 
                :keyData=0
                @openGraphics="openGraphics"
            />
            <Information 
                :detailInformation="this.detailInformation"
            />

            <Maps
                :dataMaps="this.dataMaps"
                :keyData="this.keyMaps"
                :centerCoord = "this.centerCoord"
                :detailInformation = 'this.detailInformation'
                @infoData="infoData"
            />
        </div>
        <div style="display: flex; width: 80%; height: 400px; margin: 0 auto;">
            <div style="padding: 10px;" id="select1"></div>
            <div style="padding: 10px;" id="table"></div>
        </div>

        
         
    </div>
</template>

<script>
import Maps from './Maps.vue'
//import Visualizzazioni from './Visualizzazioni.vue'
import data from '../data/pozzi4326.json'
import litodata from '../data/litologia.json'
import tempdata from '../data/temperature.json'
//import Table from './Table.vue'
import Information from './Information.vue'
import Graphics from './Graphics.vue'
import crossfilter from 'crossfilter'
//import * as d3 from 'd3'
import * as dc from 'dc';


let cf
let dPozzo
//let dDaprof
let gPozzo
//let gDaprof
export default {
    components: {
        Maps,
        //Visualizzazioni,
        //Table,
        Information,
        Graphics
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
            selected : '',
            select : [],    //NB qui
            ciao: '',
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
                    console.log(this.dataMaps[i]['nome'])
                    this.selected = this.dataMaps[i]['nome']
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
                    console.log(this.dataMaps[i]['nome'])
                    this.selected = this.dataMaps[i]['nome']
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
            var vm = this
            this.select = dc.selectMenu('#select1').dimension(dPozzo).group(gPozzo).on('filtered', function() {vm.ciao = this._filters[0]})
            this.tabella1 = dc.dataTable('#table').dimension(dPozzo).columns(['daprof' , 'aprof' , 'litologia'])
        },
        prova(nome) {
            console.log(nome)
            for(var i=0; i< this.dataMaps.length; i++){
                if(this.dataMaps[i]['nome'] == nome){
                    this.centerCoord = this.dataMaps[i]['latLan']
                    this.infoData(this.dataMaps[i]['key'], true)
                    console.log(this.dataMaps[i]['key'])
                }
            }
        },
        infoData(val, eye){
            this.detailInformation = []
            this.keyMaps = val
            //console.log('ciao')
            for(var i=0; i< this.dataMaps.length; i++){
                if(this.dataMaps[i]['key'] == val){
                    //console.log(this.dataMaps[i]['nome'])
                    //console.log(this.dataMaps[i]['key'])
                    this.detailInformation.push(this.dataMaps[i])
                    break
                }
            }
            if(eye){
                this.openNav()
            }
        },
    },
    created: function() {
    },
    computed:{

    },
    mounted: function(){
        cf=crossfilter(this.litoMaps)
        dPozzo=cf.dimension(d=>d.nome)
        //dDaprof=cf.dimension(d=>d.daprof)
        gPozzo=dPozzo.group()
        var a = gPozzo.top(10)
        console.log(a)
        this.creategraph()
        console.log(this.tabella1)
        dc.renderAll()
    },
    watch:{
        centerCoord: function(){
            this.centerCoord
        },
        selected: function(){
            this.selected
            console.log('ciao')
            console.log(this.select)
            console.log(this.tabella1)
            this.select.filter(null)
            this.select.filter(this.selected)
            this.select.redraw()
            this.tabella1.filter(null)
            this.tabella1.filter(this.selected)
            this.tabella1.redraw()
            console.log(this.select._filters[0])
        },
        detailInformation: function(){
            this.detailInformation
        },
        ciao: function(){
            this.ciao
            this.prova(this.ciao)
            console.log(this.ciao)
        },
        keyMaps: function(){
            this.keyMaps
        },
        openNavGraphics: function(){
            this.openNavGraphics
        }
    }
}
</script>