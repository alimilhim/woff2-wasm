# woff2-wasm
WebAssembly build of [woff2](https://github.com/google/woff2) 

- this version works on the web , look at the example folder that uses webpack and node's modules .
- this only supports converting from woff2 to ttf for now .

#Example
--------
```js
import wofftwo from '../wofftwo.js';
import wofftwo2Module from '../wofftwo.wasm';

const module = wofftwo({
    locateFile(path) {
        if (path.endsWith('.wasm')) {
            return wofftwo2Module;
        }
        return path;
    }
});

const convertFromVecToUint8arr=(vec)=>{
    let ttfarr = []
    for (let i = 0; i < vec.size(); i++) {
        ttfarr.push(vec.get(i))

    }
    return new Uint8Array(ttfarr)
}
module.onRuntimeInitialized = () => {
    fetch('Arial.woff2').then((x) => {
        x.arrayBuffer().then(buffer => {
            let viewBuf = new Uint8Array(buffer)
            const ttfbuff = module.woff2_dec(viewBuf, viewBuf.byteLength);
            //converting from std::vector to uint8array
           const ttfarr= convertFromVecToUint8arr(ttfbuff)
           console.log("ttf font as uint8array!: ")
            console.log(ttfarr)
        })
        })
    }
```
