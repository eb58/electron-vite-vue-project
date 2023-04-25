<script>
// const ffi = require("ffi-napi");
// const ref = require("ref-napi");

//const winax = require("./winax/winax-for-electron-17.4.11/winax");
//const winax = require("./winax/winax-for-electron-22.3.6/winax");
const winax = require("./winax/winax-for-electron-24.1.2/winax");
//const winax = require("winax");

// console.log("WINAX", winax);
// console.log("REF", ref);
// console.log("FFi", ffi);

export default {
    methods: {
        greet(event) {
            // `this` inside methods points to the current active instance
            alert(`Hello ${this.name}!`);
            // `event` is the native DOM event
            if (event) {
                alert(event.target.tagName);
            }
        },
        testWinax() {
            console.log("WINAX", winax);
            const app = new winax.Object("Word.Application");
            app.visible = true;
        },
    },
};
</script>

<template>
    <h1>{{ msg }}</h1>

    <div class="card">
        <button type="button" @click="testWinax()">TEST WINAX</button>
        <p>
            Edit
            <code>components/HelloWorld.vue</code> to test HMR
        </p>
    </div>

    <p>
        Check out
        <a href="https://vuejs.org/guide/quick-start.html#local" target="_blank"
        >create-vue</a
        >, the official Vue + Vite starter
    </p>
    <p>
        Install
        <a href="https://github.com/johnsoncodehk/volar" target="_blank">Volar</a>
        in your IDE for a better DX
    </p>
    <p class="read-the-docs">Click on the Vite and Vue logos to learn more</p>
</template>

<style scoped>
.read-the-docs {
    color: #888;
}
</style>
