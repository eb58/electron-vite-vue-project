<script lang="ts">
const { execSync } = require("child_process");
const path = require('path');
const koffi = require('koffi')

// const winax = require("winax");
// const winax = require("./winax/winax-for-electron-16.2.8/winax");  // ok
// const winax = require("./winax/winax-for-electron-17.4.11/winax"); // ok
// const winax = require("./winax/winax-for-electron-18.3.15/winax"); // ok 
// const winax = require("./winax/winax-for-electron-19.1.9/winax");  // ok
// const winax = require("./winax/winax-for-electron-20.3.8/winax");  // ok
// const winax = require("./winax/winax-for-electron-22.3.6/winax");  // ok
// const winax = require("./winax/winax-for-electron-24.1.2/winax");  // ok
// const winax = require("./winax/winax-for-electron-26.6.0/winax");  // ok
// const winax = require("./winax/winax-for-electron-27.1.0/winax");  // ok
const winax = require("./winax/winax-for-electron-31.3.1/winax");     // ok

const lib = koffi.load('user32.dll');
const user32 = {
    FindWindowA: lib.stdcall('FindWindowA', 'long', ['str', 'str'])
};

const range = (n: number) => [...Array(n).keys()]
const toArray = (objlist: any): any[] => range(objlist.Count).map((i) => objlist.Item(i + 1))
const ccAction = (_: Word.ContentControl): void => { }

const handleLock = (cc: Word.ContentControl, action: typeof ccAction): void => {
    const keepLock = cc.LockContents;
    cc.LockContents = false;
    try { action(cc) } finally { cc.LockContents = keepLock; }
}

let app: Word.Application;

const path1 = path.resolve("test-data", "test-doc1.docx");
const path2 = path.resolve("test-data", "test-doc2.docx");

try { execSync("taskkill /f /im WINWORD.exe") } catch (e) { }



export default {
    methods: {
        testWinax() {

            app = new winax.Object("Word.Application");
            app.Visible = true;
            const x = user32.FindWindowA(null, "Microsoft Word");
            console.log("FindWindow returns:", x);

        },
        initDoc() {
            const doc = app.Documents.Open(path1, true, false, false);
            range(5).forEach(n => {
                app.Selection.EndKey(Word.WdUnits.wdStory)
                app.Selection.Paragraphs.Add()
                const cc = doc.ContentControls.Add(Word.WdContentControlType.wdContentControlText)
                cc.Range.Text = " ";
                cc.Tag = cc.Title = `test-cc-${n + 1}`;
                cc.LockContents = cc.LockContentControl = true
            })
            doc.Save()
            doc.Close()
        },
        fillCCsInDoc() {
            const doc = app.Documents.Open(path2, true, false, false);
            console.log("start")
            const ccs = toArray(doc.ContentControls);
            ccs.forEach((cc) => handleLock(cc, (c: any) => c.Range.Text = "BBBBBBBBBBBBBB"))
            console.log("end")
            doc.Save()
            doc.Close()
        },
        fillCCsInDoc2() {
            const doc = app.Documents.Open(path2, true, false, false);
            console.log("start")
            const ccs = toArray(doc.ContentControls)
            ccs.forEach((cc: Word.ContentControl) => {
                if (cc.Range.Text !== "test") handleLock(cc, (c) => {
                    // c.Range.Font.Bold = false
                    c.Range.Text = "test";
                })
            })
            console.log("end")
            doc.Save()
            doc.Close()
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