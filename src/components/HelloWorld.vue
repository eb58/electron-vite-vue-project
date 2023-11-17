<script lang="ts">

const { exec } = require("child_process");

/*
const ref = require("ref-napi");
const ffi = require("ffi-napi");

const voidPtr = ref.refType(ref.types.void);
const stringPtr = ref.refType(ref.types.CString);
*/

// const winax = require("./winax/winax-for-electron-16.2.8/winax");  // ok
// const winax = require("./winax/winax-for-electron-17.4.11/winax"); // ok
// const winax = require("./winax/winax-for-electron-18.3.15/winax"); // ok 
// const winax = require("./winax/winax-for-electron-19.1.9/winax");  // ok
// const winax = require("./winax/winax-for-electron-20.3.8/winax");  // ok
// const winax = require("./winax/winax-for-electron-22.3.6/winax");  // ok
// const winax = require("./winax/winax-for-electron-24.1.2/winax"); //ok
// const winax = require("./winax/winax-for-electron-26.6.0/winax"); // ok
const winax = require("./winax/winax-for-electron-27.1.0/winax"); // ok
// const winax = require("winax");

/*const user32 = ffi.Library("user32.dll", {
    EnumWindows: ["bool", [voidPtr, "int32"]],
    EnumDesktopWindows: ["bool", ["long", voidPtr, "int32"]],
    GetWindowTextA: ["long", ["long", stringPtr, "long"]],
    SetForegroundWindow: ["bool", ["long"]],
    SetFocus: ["long", ["long"]],
    ShowWindow: ["bool", ["long", "int32"]],
    FindWindowA: ['long', ['string', 'string']],
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
*/
// console.log("WINAX", winax, ffi, ref, user32);

export default {
    methods: {
        testWinax() {
            exec("taskkill /im WINWORD.exe", () => {
                console.log("testWinax1");
                const app = new winax.Object("Word.Application");
                console.log("testWinax2");
                app.visible = false;
                console.log("testWinax3");
                app.visible = true;
                console.log("testWinax4");/*
                const x = user32.FindWindowA(null, "Word");
                console.log("FindWindow returns:", x);
                */
            })
        },
    },
};
</script>

<template>
    <div class="card">
        <button type="button" @click="testWinax()">TEST WINAX</button>
    </div>
</template>

<style scoped>
.read-the-docs {
    color: #888;
}
</style>
