# Magic bytes

[![Build Status](https://travis-ci.org/LarsKoelpin/magic-bytes.svg?branch=master)](https://travis-ci.org/LarsKoelpin/magic-bytes)


Magic Bytes is a javascript library analyzing the first bytes of a file to tell you its type. The procedure
is based on https://en.wikipedia.org/wiki/List_of_file_signatures.

# Installation
Run `npm install magic-bytes.js`

# Usage
On server:
```javascript
import filetype from 'magic-bytes.js'

filetype(fs.readFileSync("myimage.png")) // ["png"]
```

Using HTML:
```html
<input type="file" id="file" />

 <script src="../../dist/browser.js" type="application/javascript"></script>
<script>
    document.getElementById("file").addEventListener('change', (event, x) => {
      const fileReader = new FileReader();
      fileReader.onloadend = (f) => {
        const bytes = new Uint8Array(f.target.result);
        console.log("Possible filetypes: " + filetypeinfo(bytes))
      }
      fileReader.readAsArrayBuffer(event.target.files[0])
    })
</script>
```

# API
The following functions are availble:
* `filetypeinfo(bytes: number[])` Contains typeinformation like name, extension and mime type: `[{typename: "zip"}, {typename: "jar"}]`
* `filetypenames(bytes: number[])` : Contains type names only: `["zip", "jar"]`

Both function return an empty array `[]` otherwise.

see examples for practical usage.

# Tests
Run  `npm test`

# Example
See examples/

# How does it work
The `create-snapshot.js` creates a new tree. The tree has a similar shape to the following 
```json
{  
   "0x47":{  
      "0x49":{  
         "0x46":{  
            "0x38":{  
               "0x37":{  
                  "0x61":{  
                     "key":[  
                        "gif"
                     ]
                  }
               },
            }
         }
      }
   }
}
```

It acts as a giant lookup map for the given byte signatures. To check all available entries, have a look at `pattern-tree.js` and its
generated `pattern-tree.snapshot`, which acts as a static resource.

# Supported types
Please refer to  `src/pattern-tree.js`

# Roadmap
* Specialize type detection(like zip) using offset-subtrees
* Add encoding detection
