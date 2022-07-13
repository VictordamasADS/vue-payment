<template>
    <div class="message-container">
       <p> {{msg}} </p>
    </div>    
</template>

<script>
export default {
    name: "Message",
    props: {
        msg: String
    }
}
</script>

<style>
    .message-container {
        color: white;
        position: absolute;
        top: 0;
        margin: 10px 0px 0px;
        position: fixed;
        background: rgb(8, 167, 179);
        border-radius: 5px;
        width: 350px;
        height: 40px;
        display: flex;
        align-items: center;
        justify-content: center;
    }
</style>