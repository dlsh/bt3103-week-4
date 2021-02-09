<template>
    <div id = "itemsList">
        <ul>
            <li v-for="item in foodList" :key ="item.id">
                <h1 id = "itemName">{{item.name}}</h1> <br>
                <img v-bind:src = "item.imageURL"> <br>
                <p id = "price">${{item.price}}</p>
                <qtyCounter v-bind:item = 'item' v-on:counter="onCounter"></qtyCounter>
                <br>
            </li>
        </ul>
        <basket v-bind:itemsSelected = 'itemsSelected' id = "shoppingBasket"></basket>
    </div>
</template>

<script>

import QuantityCounter from './QuantityCounter.vue'
import Basket from './Basket.vue'

export default {
    data() {
        return {
            itemsSelected: []
        }
    },
    props: {
        foodList: Array
    },
    components: {
        "qtyCounter" : QuantityCounter,
        'basket' : Basket
    },
    methods: {
        onCounter: function (item, count) {
            var inBasket = false;
            var index;
            for (let i = 0; i < this.itemsSelected.length; i++) {
                if (this.itemsSelected[i][0] === item.name) {
                    inBasket = true;
                    index = i;
                    break;
                }
            }

            if (!inBasket && count > 0) {
                this.itemsSelected.push([item.name, count, item.price]);
            } else if (inBasket) {
                if (count === 0) {
                    this.itemsSelected.splice(index, 1);
                } else {
                    this.itemsSelected[index].splice(1, 1, count)
                }
            }
        }
    }
}
</script>

<style scoped>
    #itemsList {
        width: 100%;
        max-width: 70%;
        margin: 0px;
        padding: 0 5px;
        box-sizing: border-box;
    }
    ul {
        display: flex;
        flex-wrap: wrap;
        list-style-type: none;
        padding: 0;
    }
    li {
        flex-grow: 1;
        flex-basis: 300px;
        text-align: center;
        padding: 10px;
        border: 1px solid #222;
        margin: 10px;
    }
    img {
        width: 135px;
        height: 135px;
    }

    #price {
        font-size: 30px;
    }

    #itemName {
        font-size: 30px;
    }

    #shoppingBasket {
        position: absolute;
        top: 15%;
        left: 78%;
        font-size: 30px;
    }
</style>