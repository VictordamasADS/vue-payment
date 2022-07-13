<template>
    <div id="nav">
        <router-link to="/" id="logo-url">
            <img src="../../public/img/logo.jpg" alt="alt" id="logo">
        </router-link>    
        <router-link to="/" id="text"><p>Home</p></router-link> 
        <router-link to="/pedidos" id="text">Pedidos</router-link>
    </div>
</template>

<script>
export default {
    name: "Navbar",
    props: ["logo", "alt"]
}
</script>

<style scoped>
    #nav {
        height: 70px;
        background-color: #1f1861;
        color: white;
        display: flex;
        position: fixed;
        top: 0;
        align-items: center;
        justify-content: center;
        width: 100%;
    }

    #logo-url {
        left: 0;
        padding: 1rem;
        position: fixed;
    }

    #logo {
        width: 50px;
        height: 50px;
    }

    #nav a {
        color: white;
        text-decoration: none;
        margin: 12px;
        transition: .5s;
    }

    #text {
        right: 0;
        flex-direction: row;
        position: fixed;
    }

    p {
        margin: 5rem;
    }

    #nav a:hover {
        color: white;
    }
</style>