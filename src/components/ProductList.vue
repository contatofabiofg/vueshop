<template>
    <div class="productarea">
        <div class="page" v-for="(arrayproduct, index) in atualpage" :key="index">
            <div v-for="(product, index) in arrayproduct" :key="index">
            <div class="product">
                <img :src="product.image" alt="image" />
               <h5> {{product.title}}</h5>
               <h4> {{product.price}}</h4>
            </div>
            
            </div>       
        </div>
        <div class="buttons"> 
        <div class="buttonchange" @click="changepage('back')"> voltar </div>
        <div v-for="(item, index) in productspage" :key="index">
        <div class="buttonnumber" @click="gotopagenumber(index)"> {{index +1}} </div>
        </div>
        <div class="buttonchange" @click="changepage('next')"> avançar</div>
        </div>
    </div>
</template>


<script lang="ts">
import { defineComponent } from 'vue';
import axios from "axios";
import IProduct from "@/interfaces/IProduct.vue"

export default defineComponent({
    name: 'ProductlistC',
    data() {
        return {
            products: [] as typeof IProduct[],
            productspage: [{}],
            atualpage: [{}] ,
            numberpage: 0
            
        }
    },
    async created() {
       await axios.get("https://fakestoreapi.com/products").then((response) => {
        this.products = response.data
        });
        let list = this.products;
        this.productspage.splice(0,1);
        this.atualpage.splice(0,1);
        while (list.length > 0) {
            if (list.length > 6) {
                let arraytemp = list.splice(0, 6);
                this.productspage.push(arraytemp);
                
            } else {
                this.productspage.push(list);
                break;
            }
        }
        
        this.atualpage.push(this.productspage[this.numberpage])

    },
    computed: {
       
    },
    components: {
        
    },
    methods: {
       changepage(direction: string) {
        if (direction == "next" && this.numberpage < this.productspage.length - 1 ) {
            this.numberpage++
            this.atualpage.splice(0,1);
            this.atualpage.push(this.productspage[this.numberpage])
        } else if ( direction == "back" && this.numberpage > 0 ) {
            this.numberpage--
            this.atualpage.splice(0,1);
            this.atualpage.push(this.productspage[this.numberpage])
        }
       },
       gotopagenumber(pagina: number) {
            this.numberpage = pagina
            this.atualpage.splice(0,1);
            this.atualpage.push(this.productspage[pagina])
       }
    }
});
</script>

<style scoped>
.productarea {
    margin: 10px
}

.page {
    display: flex;
    flex-direction: row;
    width: 450px;
    height: 550px;
    flex-wrap: wrap;
}

.product {
    width: 100px;
    height: 130px;
    margin: 20px;
}

img {
    width: 75px;
    height: 100px;
    background-size: cover;
    }
.buttons {
    height: 50px;
    display: flex;
    flex-direction: row;
    margin-top: 20px;
    align-items: center;
 
}
.buttonnumber, .buttonchange  {
    margin-right: 10px;
    background-color: black;
    color: white;
    padding: 10px;
    cursor: pointer;
    user-select: none;
}

</style>
