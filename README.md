# metalsmith-cache

![nodei.co](https://nodei.co/npm/metalsmith-cache.png?downloads=true&downloadRank=true&stars=true)

![npm](https://img.shields.io/npm/v/metalsmith-cache.svg)

![github-issues](https://img.shields.io/github/issues/leviwheatcroft/metalsmith-cache.svg)

![stars](https://img.shields.io/github/stars/leviwheatcroft/metalsmith-cache.svg)

![forks](https://img.shields.io/github/forks/leviwheatcroft/metalsmith-cache.svg)

[metalsmith][metalsmith]] helper to cache values and files between builds.

Highlights:

 * wicked fast lokijs in-memory db
 * simple api

See the [annotated source][annotated source] or [github repo][github repo]

## install

`npm i --save metalsmith-cache`

Note that, this package will persist data in `cache.json` in your root folder. You should probably `.gitignore` that file. A postinstall warning is displayed regarding this concern.

## api

See [annotated source][annotated source]

### example

```javascript
import {
  FileCache,
  ValueCache,
  init as initCache,
  save as saveCache
} from 'metalsmith-cache'

initCache().then(() => {
  fileCache = new FileCache('google-drive')
  valueCache = new ValueCache('google-drive-params')
})

Metalsmith('src')
.use((files) => {
  return initCache().then(() => {
    const path = 'some/path'
    // store file
    fileCache.store(path, files[path])
    // retrieve file
    const file = fileCache.retrieve(path))
    // store value
    valueCache.store('lastRun', new Date().toISOString())
    // retrieve value
    const value = valueCache.retrieve('lastRun')
    return saveCache() // promise
  })
})
.build( ... )
```

Full implementation examples:

 * [metalsmith-webpack][metalsmith-webpack]
 * [metalsmith-google-drive][metalsmith-google-drive]

## lokijs

No need to worry about this if you're just consuming this package, just some dev notes.

There's some weirdness to be aware of when using lokijs

### don't mutate objects stored in docs

```
let doc = {foo: 'bar'}
collection.insert(doc)
doc.foo = 'baz'
db.save() // will save {foo: 'baz'}
```

### multiple instances of collections aren't synced

```
let loki = new Loki(...)
let foo1 = loki.getCollection('foo')
let foo2 = loki.getCollection('foo')
foo1.count() // 0
foo1.insert({...})
foo2.count() // 0
```

## Author

Levi Wheatcroft <levi@wht.cr>

## Contributing

Contributions welcome; Please submit all pull requests against the master
branch.

## License

 - **MIT** : http://opensource.org/licenses/MIT

[metalsmith]: https://metalsmith.io "metalsmith site"
[annotated source]: https://leviwheatcroft.github.io/metalsmith-cache "fancy annotated source"
[github repo]: https://github.com/leviwheatcroft/metalsmith-cache "github repo"
[metalsmith-webpack]: https://github.com/christophercliff/metalsmith-webpack "metalsmith-webpack on github"
[metalsmith-google-drive]: https://github.com/leviwheatcroft/metalsmith-google-drive "metalsmith-google-drive on github"