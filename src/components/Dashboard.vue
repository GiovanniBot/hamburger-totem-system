<template>
    <div id="burger-table">
        <div>
            <div id="burger-table-heading">
                <div class="order-id">#:</div>
                <div>Client:</div>
                <div>Bread:</div>
                <div>Meat:</div>
                <div>Optionals:</div>
                <div>ACTIONS</div>
            </div>
            <div id="burger-table-rows">
                <div class="burger-table-row" v-for="burger in burgers" :key="burger.id">
                    <div class="order-number">{{ burger.id }}</div>
                    <div>{{ burger.name }}</div>
                    <div>{{ burger.bread }}</div>
                    <div>{{ burger.meat }}</div>
                    <div>
                        <ul>
                            <li v-for="(optional, index) in burger.optionals" :key="index">
                                {{ optional }}
                            </li>
                        </ul>
                    </div>
                    <div>
                        <select name="status" class="status" @change="updateRequestProgressType($event, burger.id)">
                            <option v-for="statu in status" :key="statu.id" :value="statu.type" :selected="burger.status === statu.type">
                                {{ statu.type }}
                            </option>
                        </select>
                        <button class="delete-btn" @click="deleteRequest(burger.id)">Cancel</button>
                    </div>
                </div>
                <Message :msg="msg" v-show="msg" />
            </div>
        </div>
    </div>
</template>

<script>
import Message from './Message.vue'

export default {
    name: 'Dashboard',
    components: { Message },
    data() {
        return {
            burgers: null,
            burger_id: null,
            status: [],
            msg: null,
        }
    },
    methods: {
        async getRequests() {
            const req = await fetch("http://localhost:3000/burgers");
            const data = await req.json();
            this.burgers = data;

            this.getStatus();
        },

        async getStatus() {
            const req = await fetch("http://localhost:3000/status");
            const data = await req.json();

            this.status = data;
        },

        async deleteRequest(id) {
            const req = await fetch(
                `http://localhost:3000/burgers/${id}`, 
                { method: 'DELETE' }
            );

            this.msg = `Order N˚${id} was removed!`;
            setTimeout(() => this.msg = '', 6000);

            this.getRequests(); //this call is making 2 more requests, TODO: just delete the dom node.
        },

        async updateRequestProgressType(event, id) {
            const option = event.target.value;
            const dataJson = JSON.stringify({ status: option });

            const req = await fetch(`http://localhost:3000/burgers/${id}`, {
                method: 'PATCH',
                headers: { "Content-Type": "application/json" },
                body: dataJson
            });

            const res = await req.json();

            this.msg = `Order N˚${res.id} was updated to status: ${res.status}.`;
            setTimeout(() => this.msg = '', 6000);
        }
    },
    mounted() {
        this.getRequests();
        this.getStatus();
    },
}
</script>

<style scoped>
    #burger-table {
        max-width: 1200px;
        margin: 0 auto;
    }

    #burger-table-heading, 
    #burger-table-rows,
    .burger-table-row {
        display: flex;
        flex-wrap: wrap;
    }

    #burger-table-heading {
        font-weight: bold;
        padding: 12px;
        border-bottom: 3px solid #333;
    }

    #burger-table-heading div,
    .burger-table-row div {
        width: 19%;
    }

    .burger-table-row {
        width: 100%;
        padding: 12px;
        border-bottom: 1px solid #CCC;
    }

    #burger-table-heading .order-id,
    .burger-table-row .order-number {
        width: 5%
    }

    select {
        padding: 12px 6px;
        margin-right: 12px;
    }

    .delete-btn {
        background-color: #222;
        color: #FCBA03;
        font-size: 16px;
        font-weight: bold;
        border: 2px solid #222;
        padding: 10px;
        margin: 0 auto;
        cursor: pointer;
        transition: .5s
    }
    .delete-btn:hover {
        background-color: transparent;
        color: #222;
    }
</style>