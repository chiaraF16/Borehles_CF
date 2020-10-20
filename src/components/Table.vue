<template class="text-left">
    <div class="text-left" style="font-size:14px;">
        <vue-bootstrap4-table
            :classes="classes"
            :rows="rows" 
            :columns="columns" 
            :config="config"
            >
                <template slot="refresh-button-text">
                    <i class="fa fa-refresh"></i> Refresh
                </template>
                <template slot="reset-button-text">
                    <i class="fa fa-undo"></i> Reset
                </template>
                <template slot="key" style="width:100px;" slot-scope="props">
                    <div style="width:100px;">
                        <button type="button" class="btn btn-outline-primary btn-sm" @click='spesificData(props.cell_value, true)'>
                            <i class="fa fa-eye"></i>
                        </button> &nbsp;
                        <button type="button" class="btn btn-outline-secondary btn-sm" @click='spesificData(props.cell_value, false)'>
                            <i class="fa fa-search"></i>
                        </button>
                    </div>
                </template>
        </vue-bootstrap4-table>
    </div>
</template>
<script>
    import VueBootstrap4Table from 'vue-bootstrap4-table'
    export default {
        props: ['dataMaps', 'username'],
        data: function() {
            return {
                rows: this.dataMaps,
                columns: [
                    {
                        label: "Nome",
                        name: "nome",
                        filter: {
                            type: "simple",
                            placeholder: "cerca per nome"
                        },
                        sort: true,
                    },
                    {
                        label: "Visualizza sulla mappa",
                        name: "key",
                    },
                ],

                config: {
                    rows_selectable: true,
                    card_title: "Elenco dei pozzi di trivellazione",
                    pagination_info: true, // default true
                    num_of_visibile_pagination_buttons: 5, // default 5
                    per_page: 10, // default 10,
                    per_page_options: [10, 20, 30],
                    multi_column_sort: true,
                },
                classes: {
                    tableWrapper: "outer-table-div-class wrapper-class-two text-left",
                    table : {
                        
                    },
                    row : {
                        
                    },
                    cell : {
                        "text-left": true,                        
                        "text-center": false,
                    }
                }
                           
            }
        },
        components: {
            VueBootstrap4Table
        },

        methods: {
            spesificData(val, eye) {
                this.$emit('spesificData', val, eye)
            },
        },
        mounted(){
        }
    }
</script>
<style>
.left{
    text-align:left !important;
}
</style>