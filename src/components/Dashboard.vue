<template>
    <div class="table">
        <Message :msg="msg" v-show="msg"/>

        <div class="table-row" v-for="resultado in pedidos" :key="resultado.id">
            <p class="id-order">Pedido: {{resultado.id}}</p>
            <p>Nome: {{resultado.nome}}</p>
            <p>Endereço: {{resultado.endereco}}</p>
            <p>Quantidade: {{resultado.quantidade}}</p>

            <p v-for="(tipo, index) in resultado.metodos" :key="index">Pagemento: {{tipo}}</p>

            <p v-for="(tipo, index) in resultado.tipos" :key="index">Pacote: {{tipo}}</p>

            <div>
                <select name="status" class="status" @change="updatedOrder($event, resultado.id)">
                    <option value="">Selecione</option>
                    <option v-for="s in status" :key="s.id" :value="s.tipo" :selected="resultado.status == s.tipo">
                        {{s.tipo}}
                    </option>
                </select>
                <button class="delete-btn" @click="deleteOrder(resultado.id)">Cancelar</button>
            </div>
        </div>
    </div>
</template>

<script>
import Message from "./Message.vue"

export default {
    name: "Dashboard",
    data() {
        return {
            pedidos: null,
            status: [],
            msg: null
        }
    },
    components: {
        Message
    },
    methods: {

        async getPedidos() {
            const req = await fetch("http://localhost:3000/pedidos");

            const data = await req.json();

            this.pedidos = data;

            this.getStatus();
        },
        async getStatus() {
            const req = await fetch(" http://localhost:3000/status");

            const data = await req.json()

            this.status = data;
        },
        async deleteOrder(id) {
            const req = await fetch(`http://localhost:3000/pedidos/${id}`, {
                method: "DELETE"
            });

            const res = await req.json();

            this.msg = `O Pedido foi removido com sucesso.`;

            setTimeout(() => this.msg = "", 5000);


            this.getPedidos();
        },
        async updatedOrder(event, id) {
            const option = event.target.value;
            const dataJson = JSON.stringify({ status: option })
            const req = await fetch(`http://localhost:3000/pedidos/${id}`, {
                method: "PATCH",
                headers: { "Content-Type": "application/json" },
                body: dataJson
            })

            const res = await req.json()  
            
            this.msg = `O pedido Nº ${res.id} foi atualizado para ${res.status}.`;

            setTimeout(() => this.msg = "", 5000);

        }   
    },
    mounted() {
        this.getPedidos();
    }
}
</script>

<style scoped>

    .table {
        width: 350px;
        display: flex;
        flex-direction: column;
        background-color: white;
        align-items: center;
        justify-content: center;
    }

    @media (min-width: 1200px) {
        .table {
            width: 90%;
            flex-direction: row;
        }
    }

    .table-row + .table-row {
        margin: 10px 0px 0px;
    }

    @media (min-width: 1200px) {
        .table-row + .table-row {
            margin: 0px 5px 0px;
        }
    }

    .table-row {
        width: 100%;
        height: 300px;
        border-radius: 10px;
        background-color: white;
        box-shadow: 0 0 1em black;
        display: flex;
        align-items: center;
        flex-direction: column;
    }

    @media (min-width: 1200px) {
        .table-row {
            width: 25%;
        }
    }

    select {
        padding: 12px 6px;
        margin-right: 12px;
    }

    .delete-btn {
        background-color: rgb(179, 22, 22);
        color: white;
        font-weight: bold;
        padding: 15px;
        border: none;
        border-radius: 5px;
        font-size: 16px;
        margin: 0 auto;
        cursor: pointer;
        transition: 1s;
    }

    p + p {
        margin: 10px 0px 0px;
        font-weight: 500;
    }

    .delete-btn:hover {
        opacity: 0.7;
        color: black;
    }

    .status {
        padding: 15px;
        margin: 10px 15px 10px;
    }

    .id-order {
        font-size: 1.5em;
        margin-top: 20px;
        font-weight: 600;
    }

</style>