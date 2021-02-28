# Tool for working with indexedDB

<div style="display:grid;grid-gap:1rem;grid-auto-flow:column;width:100%;justify-content:space-between; align-items:center;">
<div>
  <a style="display:block;z-index:1;"  href="https://badge.fury.io/js/indexdb-store">
    <img style="background:transparent;" src="https://badge.fury.io/js/indexdb-store.svg" alt="npm version" height="18">
  </a>
</div>
<div>

<a  href="https://twitter.com/intent/follow?screen_name=wilfreddonaldlo"><img style="background:transparent;" align="right" src="https://img.shields.io/twitter/follow/wilfreddonaldlo?style=social&label=Follow%20@wilfreddonaldlo" alt="Follow on Twitter"></a>

  </div>

</div>
<!-- A spacer -->
<p>&nbsp;</p>

#### Install

###### NPM

```
npm install indexdb-store
```

###### Script Tag

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>indexDBStore test</title>
  </head>

  <body>
    <div>
      <h1>indexDBStore</h1>
    </div>
    <script src="https://unpkg.com/indexdb-store@latest/dist/index.umd.js"></script>

    <script>
      const dbStore = indexDBStore.indexDBStore.createStore(
        'test-utils',
        'react-utils'
      )
      dbStore('readwrite', db => {
        db.add({ hello: 'world' }, 'hello')
      })
      dbStore('readonly', db => {
        db.get('hello').onsuccess = function () {
          console.log({ res: this.result })
        }
      })
    </script>
  </body>
</html>
```

###### ES6

### IndexDB Store

```ts
import { indexDBStore } from 'indexdb-store'

const store = indexDBStore.createStore('WDB', 'myStore', {
  version: 1,
  onupgradeHandler: (store, request, event) => {
    //do transactions that can only happen on upgrade events.
    store.createIndex('IdIndex', 'id', { unique: true })
    console.log(request, event)
  },
})

//write
store.readwrite(db => {
  db.add({ id: '1', name: '1 name' }, '1')
  db.add({ id: '2', name: '2 name' }, '2')
})

//get all entries
store.entries().then(data => {
  console.log({ entries: data })
})

//get all values
store.values().then(values => {
  console.log('values: ', { values })
})

//Read
store.readonly.get('2').then(value => {
  console.log('value with key 2 is: ', value)
})

//COUNT: Retrieve the number of records matching the given key

//COUNT: Object API
const range = IDBKeyRange.bound('0', '2')
store.count(range).then(result => {
  console.log(`count is : `, result)
})
//COUNT: Function API
store('readonly', db => {
  const request = db.count('2')
  request.onsuccess = function () {
    console.log(`count is : `, this.result)
  }
})

//delete
store.del('1').then(() => {
  console.log('Delete complete for key 1')
})

store.readonly.get('1').then(value => {
  console.log('value with key 1 is now: ', value)
})
//Clear all the data in store.
store.clear().then(() => {
  console.log('store is cleared.')
})
```
