<template>
    <div class="conversor">
        <h2>{{moedaA}} Para {{moedaB}}</h2>
        <input type="text" v-model="moedaA_value" v-bind:placeholder="moedaA">
        <input type="button" value="Converter" v-on:click="converter">
        <h2>{{moedaB_value}}</h2>
    </div>
</template>

<script>
 export default {
     name: "conversor",
     props: ["moedaA","moedaB"],
    data(){
        return {
            moedaA_value : "",
            moedaB_value : 0
        }
    },
    methods:{
        converter(){
            let moedaDe = this.moedaA;
            let moedaPara = this.moedaB;
            let url = "https://freecurrencyapi.net/api/v2/latest?apikey=c9be1010-4167-11ec-bfb2-f94baa142890&base_currency="+moedaDe;

            fetch(url)
                .then(res=>{return res.json()})
                .then(json=>{
                    let array_currencys = new Array();
                    let key;
                    for(key in json.data){
                        let obj_moeda = new Object();
                        obj_moeda.moeda = key;
                        obj_moeda.valor = json.data[key];
                        array_currencys.push(obj_moeda);
                    }
                    
                    let valCot;
                    array_currencys.forEach( (item) => {
                        // se o id do array de objetos estiver em a1
                        if (moedaPara.indexOf(item.moeda) > -1)
                            valCot = item.valor // entao remove
                    });
                    console.log(valCot);
                    this.moedaB_value = (valCot * parseFloat(this.moedaA_value)).toFixed(2);

                })
        }
    }
}
    
</script>

<style scoped>
    .conversor{
        padding: 20px;
        max-width: 300px;
        box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);

    }
</style>