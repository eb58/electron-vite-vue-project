<script>
const ref = require("ref-napi");
const ffi = require("ffi-napi");

const voidPtr = ref.refType(ref.types.void);
const stringPtr = ref.refType(ref.types.CString);

// const winax = require("./winax/winax-for-electron-16.2.8/winax");
// const winax = require("./winax/winax-for-electron-15.5.7/winax");
// const winax = require("./winax/winax-for-electron-17.4.11/winax");
// const winax = require("./winax/winax-for-electron-18.3.15/winax");
const winax = require("./winax/winax-for-electron-20.3.8/winax");
// geht nicht !!! const winax = require("./winax/winax-for-electron-22.3.6/winax"); 
// geht nicht !!! const winax = require("./winax/winax-for-electron-24.1.2/winax");
//const winax = require("winax");

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
            const app = new winax.Object("Word.Application"); 
            app.visible = true;

            const x = findWindow("Word");
            console.log( "FindWindow returns:", x );
        },
    },
};
</script>

<template>
    <h1>{{ msg }}</h1>

    <div class="card">
        <button type="button" @click="testWinax()">TEST WINAX</button>
    </div>
</template>

<style scoped>
.read-the-docs {
    color: #888;
}
</style>
