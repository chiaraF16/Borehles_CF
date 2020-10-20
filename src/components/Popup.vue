<template>
    <div>
            <div><b>Nome:</b> {{ this.nome }} </div>
            <div><b>Provincia:</b> {{ this.provincia }} </div>
            <div><b>Quota:</b> {{ this.quota }} </div>
            <div><b>Data:</b> {{ this.datacomp }} </div>
            <div>
                <button type="button" class="btn btn-outline-primary btn-sm" style="float:right;" @click="openNav()">
                    <i class="fa fa-eye"></i>
                </button>
            </div>
            <br/>
            <br/>
    </div>
</template>

<script>
export default {
    props: ['keyData','nome', 'locgeo', 'provincia', 'quota', 'datacomp'],
    
    components: {
        
    },
    data () {
        return {
            keyCoord : this.keyData,
            nomeData : this.nome,
            locgeoData : this.locgeo,
            provinciaData : this.provincia,
            quotaData : this.quota,
            datacompData : this.datacomp,
        }
    },
    methods: {
        visibleClass() {
            this.visible = !this.visible
            console.log(this.visible) 
        },
        openNav(){
            this.$emit('infoData', this.keyCoord, true)
        }
    },
    created: function(){

    },
    computed:{

    },
    mounted: function(){

    },
    watch:{
        keyCoord: function(){
            this.keyData = this.keyCoord
        }, 
    }
}
</script>