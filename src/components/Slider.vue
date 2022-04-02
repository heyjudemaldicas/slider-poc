<template>
    <div class="content">
        <div class="wrapper">
            <div class="item">
                <h2>Price : {{ price.value }}</h2>
                <vue-slider v-model="price.value" :min="price.min" :max="price.max" @change="selectPrice" />
            </div>
            <div class="item">
                <h2>Downpayment : {{ downpayment.value }}</h2>
                <vue-slider v-model="downpayment.value" :min="downpayment.min" :max="downpayment.max"  @change="selectDownpayment"/>
            </div>
        </div>
    </div>
</template>
<script>
import VueSlider from 'vue-slider-component';
import 'vue-slider-component/theme/antd.css';

export default {
    name: "SliderComponent",
    components: {
        VueSlider,
    },
    data() {
        return {
            price: {
                value: 0,
                min: 0,
                max: 100,
            },
            downpayment: {
                value: 0,
                min: 0,
                max: 100,
                percentage: 0,
            },
            minCredit: 3000,
            maxPrice: 45000,
            priceData: [],
        };
    },
    methods: {
        selectPrice() {
            this.price.value = this.minMax(this.price.value,this.price.min, this.price.max);
            this.downpayment.max = this.price.value - (this.minCredit + (1 - 0.3));
            this.downpayment.min = Math.round(this.price.value * 0.321);

            if (this.downpayment.min > this.price.value - this.minCredit + 1) {
                this.downpayment.min = this.price.value - this.minCredit + 1;
            }

            if (this.downpayment.percentage > 0) {
                this.downpayment.value = this.downpayment.percentage * this.price.value;
                this.downpayment.percentage = this.downpayment.value  / this.price.value;
                this.downpayment.value = this.minMax(Math.round(this.price.value),this.price.min, this.price.max);
            }
            else {
                this.downpayment.value = this.minMax(this.downpayment.value,this.downpayment.min, this.downpayment.max);
                this.downpayment.percentage = this.downpayment.value  / this.price.value;
            }
        },
        selectDownpayment() {
            this.downpayment.value = this.minMax(this.downpayment.value,this.downpayment.min, this.downpayment.max);
            this.downpayment.percentage = this.downpayment.value  / this.price.value;
        },
        init() {
            this.price.max = this.maxPrice;
            this.price.min = Math.round((this.minCredit * (1 - 0.03)) / (1 - 0.321));
            this.price.value = this.price.min;

            this.downpayment.max = this.price.value - (this.minCredit * (1 - 0.03));
            this.downpayment.min = Math.round(this.price.value * 0.321);
            this.downpayment.value = this.downpayment.min;
        },
        getInterVal(value) {
            return value % 500 === 0 ? 500 : 500 - (value % 500);
        },
        getPriceData() {
            //let data = [];
            let value = this.price.min;
            while(value < this.price.max) {
                this.priceData.push(value);
                value % 500 === 0 ? value += 500 : value += (500 - (value % 500));
            }
            console.log(this.priceData);
        },
        minMax(val, min, max) {
            if (val < min) return min;
            if (val > max) return max;
            return val;
        },
    },
    mounted() {
        this.init();
        //this.getPriceData();
    },
}
</script>
<style lang="scss">
.content {
    width: 600px;
    border: 1px solid black;
    padding: 50px 30px;

    .wrapper {
        .item {
        }
    }
}
</style>