<template>
    <div class="areaproduct">
        <div class="product" v-for="(item, index) in atualpage" :key="index">
            
                <div >
                    <img :src="item.image" alt="image" />
                    <h5> {{ item.title }}</h5>
                    <h4> {{ item.price }}</h4>
                </div>
        </div>
    </div>
    <div class="buttons">
            <div class="buttonchange" @click="changepage('back')"> voltar </div>
            <div v-for="(item, index) in pages" :key="index">
                <div class="buttonnumber" @click="gotopagenumber(index)"> {{ index + 1 }} </div>
            </div>
            <div class="buttonchange" @click="changepage('next')"> avançar</div>
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
            allproducts: [] as typeof IProduct[], //tipagem: array de IProduto
            pages: [] as typeof IProduct[][], // tipagem: Array de Arrays de IProduto
            atualpage: [] as typeof IProduct[], //tipagem: array de IProduto
            numberpage: 0

        }
    },
    async created() {
        await axios.get("https://fakestoreapi.com/products").then((response) => {
            this.allproducts = response.data
        });
       
        while (this.allproducts.length > 0) {
            if (this.allproducts.length > 6) {
                let arraytemp = this.allproducts.splice(0, 6);
                this.pages.push(arraytemp);

            } else {
                this.pages.push(this.allproducts);
                break;
            }
        }
      
        this.atualpage = this.pages[this.numberpage]

    },
    methods: {
        changepage(direction: string) {
            if (direction == "next" && this.numberpage < this.pages.length - 1) {
                this.numberpage++
                this.atualpage = this.pages[this.numberpage]
            } else if (direction == "back" && this.numberpage > 0) {
                this.numberpage--
                this.atualpage = this.pages[this.numberpage]
            }
        },
        gotopagenumber(pagina: number) {
            this.numberpage = pagina
            this.atualpage = this.pages[pagina]
        }
    }
});
</script>

<style scoped>


.areaproduct {
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

.buttonnumber,
.buttonchange {
    margin-right: 10px;
    background-color: black;
    color: white;
    padding: 10px;
    cursor: pointer;
    user-select: none;
}
</style>
