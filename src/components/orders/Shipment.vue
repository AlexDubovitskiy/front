<template>
    <div>
        <div @click = "addLine" class = "btn btn-info pointer mb-3">Add product</div>
        <div class = "row line" v-for = "(line, index) in lines">
            <div class = "form-group col-6">
                <select v-model = "line.product" name = 'products[]' class = "form-control">
                    <option v-if = "products.length === 0" value = "" disabled>There is no product to sell</option>
                    <option v-if = " products.length !== 0" value = "" disabled>Select product</option>
                    <option
                        v-if = " products.length !== 0"
                        v-bind:value = "product.id" v-for = "product in products">{{product.name}}
                    </option>
                </select>
            </div>
            <div class = "form-group col-5">
                <input name = 'sellingPrices[]' v-model = "line.price" class = "form-control">
            </div>
            <div class = "col-1">
                <div @click = "removeLine(index)" class = "btn btn-danger pointer">x</div>
            </div>
        </div>
        <div class = " modal-footer">
            <button type = "submit" class = "btn btn-primary">Continue</button>
        </div>
    </div>
</template>

<script>


    export default {
        name      : "product-line",
        components: {
        },

        data: function () {
            return {
                products: [],
                lines   : [],
                deal    : []

            }
        },

        methods: {
            addLine      : function () {
                this.lines.push({
                    product: "",
                    price  : "",
                });
                this.counter++;
            },
            removeLine   : function (index) {
                this.lines.splice(index, 1)
            },
            optionChanged: function (contractor) {
                axios.get("api/products-in-stock-by-contractor", {
                    params: {contractor: contractor}
                }).then(response => {
                    this.products.splice(0, this.products.length);
                    for (let item of response.data) {
                        let product = {
                            id   : item.id,
                            name : item.name,
                            price: item.price,
                        };
                        this.products.push(product);
                    }
                });
            },
        }
    }
</script>

<style scoped>

    .pointer {
        cursor: pointer
    }

</style>

