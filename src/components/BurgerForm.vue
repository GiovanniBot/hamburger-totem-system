<template>
    <div>
        <Message :msg="msg" v-show="msg" />
        <div>
            <form id="burger-form" @submit.prevent="createBurger">

                <div class="input-container">
                    <label for="name">Client's name: </label>
                    <input type="text" id="name" name="name" v-model="name" placeholder="Insert your name." autocomplete="off" required>
                </div>

                <div class="input-container">
                    <label for="bread">Choose the bread type: </label>
                    <select name="bread" id="bread" v-model="bread" required>
                        <option value="">Select one option</option>
                        <option v-for="bread in breads" :key="bread.id" :value="bread.type">{{ bread.type }}</option>
                    </select>
                </div>

                <div class="input-container">
                    <label for="meat">Select the meat: </label>
                    <select name="meat" id="meat" v-model="meat" required>
                        <option value="">Select one option</option>
                        <option v-for="meat in meats" :key="meat.id" :value="meat.type">{{ meat.type }}</option>
                    </select>
                </div>

                <div class="input-container optionals-container">
                    <label id="optionals-title" for="optionals">Select the optionals: </label>
                    <div v-for="optional in optionalsData" :key="optional.id" class="checkbox-container">
                        <input type="checkbox" name="optionals" v-model="optionals" :value="optional.type">
                        <span>{{ optional.type }}</span>
                    </div>
                </div>

                <div class="input-container">
                    <input type="submit" class="submit-btn" value="Create my Burger!">
                </div>

            </form>
        </div>
    </div>
</template>

<script>
import Message from './Message.vue'

export default {
    name: 'BurgerForm',
    components: { Message },

    data() {
        return {
            breads: null,
            meats: null,
            optionalsData: null,

            name: null,
            bread: null,
            meat: null,
            optionals: [],
            msg: null,
        }
    },

    methods: {
        async getIngredients() {
            const req = await fetch("http://localhost:3000/ingredients");
            const data = await req.json();

            this.breads = data.breads;
            this.meats = data.meats;
            this.optionalsData = data.optionals;
        },

        async createBurger() {
            const data = {
                name: this.name,
                bread: this.bread,
                meat: this.meat,
                optionals: Array.from(this.optionals),
                status: 'Requested',
            }

            const dataJson = JSON.stringify(data);

            const req = await fetch("http://localhost:3000/burgers", {
                method: "POST",
                headers: { "Content-Type": "application/json" },
                body: dataJson,
            });

            const res = await req.json();

            this.msg = `Your order N˚${res.id} was created succesfully!`;
            setTimeout(() => this.msg = '', 6000);

            this.name = '';
            this.bread = '';
            this.meat = '';
            this.optionals = '';
        },
    },

    mounted() {
        this.getIngredients();
    }
}
</script>

<style scoped>
    #burger-form {
        max-width: 400px;
        margin: 0 auto;
    }

    .input-container {
        display: flex;
        flex-direction: column;
        margin-bottom: 20px;
    }

    label {
        font-weight: bold;
        margin-bottom: 15px;
        color: #222;
        padding: 5px 10px;
        border-left: 4px solid #FCBA03;
    }

    input, select {
        padding: 5px 10px;
        width: 300px;
    }

    #optionals-container {
        flex-direction: row;
        flex-wrap: wrap;
    }

    #optionals-title {
        width: 100%;
    }

    .checkbox-container {
        display: flex;
        align-items: flex-start;
        width: 50%;
        margin-bottom: 20px;
    }

    .checkbox-container span,
    .checkbox-container input {
        width: auto;
    }

    .checkbox-container span {
        margin-left: 6px;
    }

    .submit-btn {
        background-color: #222;
        color: #FCBA03;
        font-weight: bold;
        border: 2px solid #222;
        padding: 10px;
        font-size: 16px;
        margin: 0 auto;
        cursor: pointer;
        transition: .5s;
    }

    .submit-btn:hover {
        background-color: transparent;
        color: #222;
    }
</style>