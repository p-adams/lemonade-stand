<template>
    <div class="container" id="report">
        <div v-if="storm">
            meow
            <storm></storm>
        </div>
        <div v-if="!storm">
            <h3>$$$ Habib's Kebab Stand Daily Financial Report $$$</h3>
            <table class="table borderless">
                <thead>
                    <tr>
                        <th>Day: {{day}}</th>
                        <th>Stand: {{stand}}</th>
                    </tr>
                </thead>
                <tbody>
                    <!-- handle singular/plural cases -->
                    <tr>
                        <td>{{sold}} {{sold <= 1 ? 'kebab' : 'kebabs'}} sold</td>
                    </tr>
                    <tr>
                        <td>${{price}} per sandwich</td>
                        <td>Income ${{income}}</td>
                    </tr>
                    <tr>
                        <td>{{sandwiches}} {{sandwiches <= 1 ? 'sandwich' : 'sandwiches'}} made</td>
                    </tr>
                    <tr>
                        <td>{{signs}} {{signs <= 1 ? 'sign' : 'signs'}} made</td>
                        <td>Expenses ${{expenses}}</td>         
                    </tr>
                    <tr>
                        <td>Profits ${{profits}}</td>
                    </tr>
                    <tr>
                        <td>Assets ${{assets}}</td>
                    </tr>
                    <tr>                
                        <td>
                            <span>Press next to continue
                            <button @click="next">next</button>
                            </span>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>
<script>
import storm from './Tstorm.vue'
export default {
    name: 'report',
    data(){
        return{
            storm: false,
            sales: ''
        }
    },
    created(){
        if(this.$store.state.weather === 'tstorm'){
            this.storm = true
            setTimeout(this.hideStorm, 1000)
        }
    },
    mounted(){
        this.loadSalesData()
    },
    methods:{
        loadSalesData(){
            this.sales = this.$store.state.salesData["sales"]
        },
        hideStorm(){
            this.storm = false
        },
        next(){
            this.$store.dispatch('nextState', {next: 'weather'})
            this.$store.dispatch('clearSales')
        }
    },
    computed:{
        day() {
            return this.$store.getters.showDay
        },
        stand() {
            return this.$store.state.numStands[0].s1
        },
        sold() {
            return this.$store.state.salesData.sales
        },
        price() {
            return this.$store.state.salesData.price
        },
        income() {
            return this.$store.getters.income
        },
        sandwiches() {
            return this.$store.state.assets.sandwiches
        },
        signs() {
            return this.$store.state.assets.adverts
        },
        expenses() {
            return this.$store.getters.expenses
        },
        profits() {
            return this.$store.getters.profits
        },
        assets() {
            return this.$store.state.assets.cash.toFixed(2)
        }
    },
    components:{
        storm
    }
}
</script>
<style>
.borderless td, .borderless th {
    border: none !important;
}
h3{
    font-size: 30px;
    margin-bottom: 30px;
    text-align: center;
}
</style>