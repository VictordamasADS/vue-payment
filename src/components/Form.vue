<template>
    <div class="container">
        <Message :msg="msg" v-show="msg"/>
        <div>
            <form id="form" @submit="createOrder">

                <div class="header">
                    <h4 class="title">Formulário para compra de Pacote de adesivos</h4>
                    <img class="image" src="../../public/img/foto.png" alt="alt" id="logo">
                </div>

                <div id="ingredientes-container" class="input-container">
                    <label id="ingredientes-title" for="tipos">Tipos de Adesivos:</label>
                    <div class="checkbox-container" v-for="tipo in frameworksTipos" :key="tipo.id">
                        <input type="checkbox" name="tipos" v-model="tipos" :value="tipo.tipo">
                        <span>{{tipo.tipo}}</span>
                    </div>    
                </div>

                <div class="input-container">
                    <label for="nome">Nome do cliente</label>
                    <input class="input" type="text" id="nome" name="name" v-model="nome" placeholder="Digite o seu nome">

                    <label for="endereco">Endereço de entrega</label>
                    <input class="input" type="text" id="endereco" name="endereco" v-model="endereco" placeholder="Digite o seu endereço">
                </div>

                <div class="input-container">
                    <label for="quantidade">Escolha a quantidade:</label>
                    <select class="input" name="quantidade" id="quantidade" v-model="quantidade">
                        <option value="">Escolha uma opção</option>
                        <option v-for="quantidade in quantidades" :key="quantidade.id" :value="quantidade.tipo">
                            {{quantidade.tipo}}
                        </option>
                    </select>
                </div>    

                <div id="ingredientes-container" class="input-container">
                    <label id="ingredientes-title" for="pagamentos">Métodos de pagamento:</label>

                    <div class="checkbox-container" v-for="pagamento in pagamentos" :key="pagamento.id">
                        <input type="checkbox" name="metodos" v-model="metodos" :value="pagamento.tipo">
                        <span>{{pagamento.tipo}}</span>
                    </div>    
                </div>

                <div class="input-container">
                    <input type="submit" class="submit-btn" value="Finalizar pagamento">
                </div>
            </form>
        </div>
    </div>    
</template>

<script>
import Message from "./Message.vue"

export default {
    name: "Form",
    props: ["foto", "alt"],
    data() {
        return {
            quantidades: null,
            frameworksTipos: null,
            pagamentos: null,
            nome: null,
            endereco: null,
            quantidade: null,
            tipos: [],
            metodos: [],
            msg: null
        }
    },
    methods: {
        async getIngredientes() {
            const req = await fetch("http://localhost:3000/ingredientes");
            const data = await req.json();

            this.quantidades = data.quantidades;
            this.pagamentos = data.pagamentos;
            this.frameworksTipos = data.tipos;
        },
        async createOrder(e) {
            e.preventDefault();

            const data = {
                nome: this.nome,
                endereco: this.endereco,
                quantidade: this.quantidade,
                pagamento: this.pagamento,
                tipos: Array.from(this.tipos),
                metodos: Array.from(this.metodos),
                status: "Solicitado"
            }

            const dataJson = JSON.stringify(data);

            const req = await fetch("http://localhost:3000/pedidos", {
                method: "POST",
                headers: {"Content-Type": "application/json"},
                body: dataJson
            })

            const res = await req.json();
            
            this.msg = `Pedido Nº ${res.id} realizado com sucesso!`;

            setTimeout(() => this.msg = "", 5000);

            this.nome = "";
            this.endereco = "";
            this.quantidade = "";
            this.pagamentos = "";
            this.tipos = "";
        }

    },
    mounted() {
        this.getIngredientes()
    },
    components: {
        Message
    }
}
</script>

<style scoped>
    * {
        margin: 0;
        padding: 0;
    }

    .container {
        display: flex;
        align-items: center;
        justify-content: center;
    }

    #form {
        width: 300px;
        margin-top: 40px;
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        border-radius: 10px;
        background-color: white;
        box-shadow: 0 0 1em black;
        height: 800px;
        margin-bottom: 50px;
    }

    @media (min-width: 1200px) {
        #form {
            width: 500px;
        }
    }

    .header {
        background-color: #312792;
        border-bottom-right-radius: 100%;
        flex-direction: row;
        display: flex;
        justify-content: space-around;
        border-bottom-left-radius: 70%;
        width: 100%;
        height: 300px;
    }

    .input-container {
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        margin-bottom: 10px;
    }

    .input {
        width: 250px;
    }

    label {
        font-weight: bold;
        margin-bottom: 15px;
        color: #222;
    }

    input, select {
        padding: 5px 10px;
        margin-bottom: 10px;
        width: 300px;
    }

    #ingredientes-container {
        margin: 10;
        margin-top: 10px;
    }

    #ingredientes-title {
        width: 100%;
    }

    .checkbox-container {
        flex-direction: row;
        margin: 0;
        width: 100%;
        margin-bottom: 10px;
    }

    .image {
        margin-top: 20px;
        width: 130px;
        height: 120px;
    }

    .checkbox-container span,
    .checkbox-container input {
        width: auto;
    }

    .checkbox-container span {
        margin-left: 5px;
        font-weight: bold;
    }

    .submit-btn {
        background-color: #312792;
        color: white;
        width: 200px;
        font-weight: bold;
        border: none;
        border-radius: 5px;
        padding: 10px;
        font-size: 16px;
        cursor: pointer;
        transition: 1s;
    }

    .title {
        margin-top: 20px;
        color: white;
        width: 30%;
    }

    @media (min-width: 1200px) {
        .title {
            margin-top: 50px;
        }
    }
    
    .payment {
        display: flex;
        align-items: center;
        justify-content: center;
        flex-direction: column;
    }

    .submit-btn:hover {
        opacity: 0.9;
        color: black;
    }

</style>