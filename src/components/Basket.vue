<template>
    <div id = "basket">
        <span>Menu:</span><br>
        <ul>
            <li v-for="item in itemsSelected" :key = "item[0]">
                <p>{{item[0]}} x {{item[1]}}</p>
            </li>
        </ul>

        <button v-on:click="findTotal()">Calculate Total</button> <br>
        <p v-show="buttonClicked">Subtotal: ${{subTotalPrice}}</p> 
        <p v-show="buttonClicked">Grand Total: ${{grandTotal}}</p>

    </div>
</template>

<script>
export default {
    data() {
        return {
            subTotalPrice: 0,
            buttonClicked: false
        }
    },
    props: {
        itemsSelected: Array
    },
    methods: {
        findTotal : function() {
            var total = 0;
            for (let i = 0; i < this.itemsSelected.length; i++) {
                const curr = this.itemsSelected[i];
                total += curr[1] * curr[2];
            }
            this.subTotalPrice = total;
            this.buttonClicked = true;
        }
    },
    computed: {
        grandTotal : function() {
            return (this.subTotalPrice * 1.07).toLocaleString("en-US", { maximumFractionDigits: 2, minimumFractionDigits: 2 });
        }
    }
}
</script>

<style scoped>
    span {
        float: left;
    }


    button {
        background-color: #ffc0cb;
        border: 1px solid black;
        border-radius: 5px;
        float: left;
    }

    p {
        text-align: left;
    }

</style>
