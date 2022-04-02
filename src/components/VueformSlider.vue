<template>
    <div class="content">
        <div class="wrapper">
            <div class="item">
                <h2>Price : {{ price.value }}</h2>
                <Slider v-model="index" :min="0" :max="array.length - 1" :step="1" :format="format" @update="selectPrice"/>
            </div>
            <div class="item">
                <h2>Downpayment : {{ downpayment.value }}</h2>
                <Slider v-model="downpayment.value" :min="downpayment.min" :max="downpayment.max" @update="selectDownpayment"/>
            </div>
        </div>
    </div>
</template>
<script>
import Slider from '@vueform/slider';

export default {
    name: "VueformSlider",
    components: {
        Slider,
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
            format: function() {},
            array: [],
            index: 0,
        };
    },
    methods: {
        selectPrice(e) {
            this.price.value = this.array[e];
            this.price.value = this.minMax(this.price.value,this.price.min, this.price.max);
            this.downpayment.max = this.price.value - (this.minCredit * (1 - 0.03));
            this.downpayment.min = Math.round(this.price.value * 0.321);

            if (this.downpayment.min > this.price.value - this.minCredit + 1) {
                this.downpayment.min = this.price.value - this.minCredit + 1;
            }

            if (this.downpayment.percentage > 0) {
                this.downpayment.value = this.downpayment.percentage * this.price.value;
                this.downpayment.percentage = this.downpayment.value  / this.price.value;
                this.downpayment.value = this.minMax(Math.round(this.downpayment.value),this.downpayment.min, this.downpayment.max);
            }
            else {
                this.downpayment.value = this.minMax(this.downpayment.value,this.downpayment.min, this.downpayment.max);
                this.downpayment.percentage = this.downpayment.value  / this.price.value;
            }

            console.log(this.price.value);
        },
        selectDownpayment() {
            this.downpayment.value = this.minMax(this.downpayment.value,this.downpayment.min, this.downpayment.max);
            this.downpayment.percentage = this.downpayment.value  / this.price.value;
        },
        getPriceData() {
            let value = this.price.min;

            while (value <= this.price.max) {
                this.array.push(value);
                value % 500 === 0 ? (value += 500) : (value += 500 - (value % 500));
            }
            this.format = (index) => this.array[index];
            console.log(this.array);
        },
        init() {
            this.price.max = this.maxPrice;
            this.price.min = Math.round((this.minCredit * (1 - 0.03)) / (1 - 0.321));
            this.price.value = this.price.min;

            this.downpayment.max = this.price.value - (this.minCredit * (1 - 0.03));
            this.downpayment.min = Math.round(this.price.value * 0.321);
            this.downpayment.value = this.downpayment.min;
        },
        minMax(val, min, max) {
            if (val < min) return min;
            if (val > max) return max;
            return val;
        },
    },
    mounted() {
        this.init();
        this.getPriceData();
    },
}
</script>
<style src="@vueform/slider/themes/default.css"></style>
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