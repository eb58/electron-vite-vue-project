<script>
const ref = require("ref-napi");
const ffi = require("ffi-napi");

const voidPtr = ref.refType(ref.types.void);
const stringPtr = ref.refType(ref.types.CString);

const winax = require("./winax/winax-for-electron-16.2.8/winax");
// const winax = require("./winax/winax-for-electron-15.5.7/winax");
//const winax = require("./winax/winax-for-electron-17.4.11/winax");
//const winax = require("./winax/winax-for-electron-22.3.6/winax");
// const winax = require("./winax/winax-for-electron-24.1.2/winax");
// const winax = require("winax");

const user32 = ffi.Library("user32.dll", {
    EnumWindows: ["bool", [voidPtr, "int32"]],
    EnumDesktopWindows: ["bool", ["long", voidPtr, "int32"]],
    GetWindowTextA: ["long", ["long", stringPtr, "long"]],
    SetForegroundWindow: ["bool", ["long"]],
    SetFocus: ["long", ["long"]],
    ShowWindow: ["bool", ["long", "int32"]],
    BringWindowToTop: ["bool", ["long"]],
});

const findWindow = (s) => {
    let n = 0;
    let res = false;
    const windowProc = ffi.Callback("bool", ["long", "int32"], (hWnd) => {
        if (!res) {
          const buf = Buffer.alloc(255, " ");
          user32.GetWindowTextA(hWnd, buf, 255);
          const x = buf.toString().trim();
          if (x) {
            n++;
            // console.log(x, x.length, buf);
            res = res || x.includes(s);
          }
        }
    });
    user32.EnumWindows(windowProc, 0);
    console.log("end", res, n);
    return res;
};

console.log("WINAX", winax, ffi, ref, user32);

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
            // const app = new winax.Object("Word.Application"); app.visible = true;
            const x = findWindow("neu 1 - Notepad++");
            console.log( x );
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
