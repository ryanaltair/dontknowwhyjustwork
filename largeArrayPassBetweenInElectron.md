in electron, you get 2 process, at least

sometimes you need to pass some data from one to another

but sad song goes when you pass large data, no matter from ipcMain to ipcRenderer,
or the way reversed.

now my problem is try to send a big array contains the verts of a mesh, maybe 15 Mb more

as you know, ipcSenders(what i mean ipcRenderer.send or something alike) will try to these:

'Arguments will be serialized in JSON internally and hence no functions or prototype chain will be included.' from docs here

https://electronjs.org/docs/api/ipc-renderer

it will freezes the window, and will hang or crash for some.

so make it way out:

that is slice the array and everything good.

-------------------------

old way

-----------------------
===================
Main side
```
ipcMain.on('channelA',(event,largeArray)=>{
    let arrayGet = largeArray
   // arrayGet.doSomething
})
```

=====================


Renderer side

```
let bigArray= new Array(1000000)
bigArray.fill("heavy data")
ipcRenderer.send("channelA",bigArray)
```

=====================


------------------
new way
===================
Main side
```
let arrayLength
let arrayGet
ipcMain.on('channelANew', (event, largeArrayLength) => {
  arrayLength = largeArrayLength
  arrayGet = []
})

ipcMain.on('channelA', (event, largeArraySlice) => {
  largeArraySlice.forEach((item) => {
    arrayGet.push(item)
  })
  if (arrayGet.length === arrayLength) {
    //   arrayGet.doSomething
  }
})
```

Renderer side
```

let bigArray = new Array(1000000)
bigArray.fill('heavy data')

let bigArrayLength = bigArray.length

ipcRenderer.send('channelANew', bigArrayLength)
for (let i = 0; i < bigArrayLength; i += 5000) {
  if (i + 5000 < bigArrayLength) {
    ipcRenderer.send('channelA', bigArraySilce(i, i + 5000))
  } else {
    ipcRenderer.send('channelA', bigArraySilce(i))
  }
}


```


