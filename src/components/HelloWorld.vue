<script lang="ts">
import { WD } from "../wordConsts"
// import koffi from 'koffi';
const koffi = require('koffi/indirect') 
const path = require('path');

const { execSync } = require("child_process");
const range = (n: number) => [...Array(n).keys()]
const toArray = (objlist: any) => {
    const res = [];
    for (let i = 1; i <= objlist.Count; i++) {
        res.push(objlist.Item(i));
    }
    return res;
}

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

const lib = koffi.load('user32.dll');
const user32= {
    FindWindowA: lib.stdcall('FindWindowA', 'long', ['str', 'str'])

};


console.log("WINAX", winax, user32);



const path1 = path.resolve("test-data", "test-doc1.docx");
const path2 = path.resolve("test-data", "test-doc2.docx");

try { execSync("taskkill /f /im WINWORD.exe") } catch (e) { }
const app = new winax.Object("Word.Application");
app.visible = true;


export default {
    methods: {
        testWinax() {
            const x = user32.FindWindowA(null, "Word");
            console.log("FindWindow returns:", x);
            // const app = new winax.Object("Word.Application");
            // app.visible = true;
            /*
            const x = user32.FindWindowA(null, "Word");
            console.log("FindWindow returns:", x);
            */
        },
        initDoc() {
            const doc = app.documents.open(path1, true, true, false);
            range(50).forEach(n => {
                app.selection.endKey(WD.wdStory)
                app.selection.paragraphs.add()
                const cc = doc.contentControls.add(WD.wdContentControlText)
                cc.range = " ";
                cc.tag = cc.title = `test-cc-${n + 1}`;
                cc.lockContents = cc.LockContentControl = true
            })
            doc.save(WD.WdSaveOptions.wdSaveChanges)
        },
        fillCCsInDoc() {
            const doc = app.documents.open(path2, true, true, false);
            console.log("start")
            toArray(doc.contentControls).forEach(cc => {
                cc.lockContents = false
                cc.range = "BBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBB rt j vkljfglkfjgklfd jglkfjk fjglk fjkjg ggfjkglfj gkfgf gkfjgk lfjgklfj gklfgklfj"
                cc.lockContents = true
            })
            console.log("end")
            doc.close(WD.WdSaveOptions.wdDoNotSaveChanges)
        },
        fillCCsInDoc2() {
            const doc = app.documents.open(path2, true, true, false);
            console.log("start")
            const ccs = range(50).map(n => doc.selectContentControlsByTag(`test-cc-${n + 1}`).item(1));
            console.log("...")
            ccs.forEach(cc => {
                cc.lockContents = false
                cc.range.font.bold = true
                cc.range.font.Color = 5
                cc.range = "BBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBB rt j vkljfglkfjgklfd jglkfjk fjglk fjkjg ggfjkglfj gkfgf gkfjgk lfjgklfj gklfgklfj"
                cc.lockContents = true
            })
            console.log("end")
            doc.close(WD.WdSaveOptions.wdDoNotSaveChanges)
        }

    },
};
</script>

<template>
    <div>
        <button type="button" @click="testWinax()">TEST WINAX</button>
        <button type="button" @click="initDoc()">Init Document</button>
        <button type="button" @click="fillCCsInDoc()">Fill CCs in Document</button>
        <button type="button" @click="fillCCsInDoc2()">Fill CCs in Document 2</button>
    </div>
</template>