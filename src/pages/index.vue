<template>
    <div class="container">
        <h2 title>Home Page</h2>
        <div class="home_text">
            <p>Welcome to the To Do List app</p>
        </div>
        
    </div>
</template>
<script>
    export default {
    }
</script>
<style>
    .home_text{
        box-shadow: inset 2px 2px 4px rgba(0, 0, 0, 0.2), 2px 2px 4px rgba(0, 0, 0, 0.2);
        height: 100px;
        display: flex;
        justify-content: center;
        align-items: center;
    }
    p{  
        
        font-size: 16px;
        color: #000;
        font-weight: 500;
    }
</style>
