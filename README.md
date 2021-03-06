# metalsmith-cache

![nodei.co](https://nodei.co/npm/metalsmith-cache.png?downloads=true&downloadRank=true&stars=true)

![npm](https://img.shields.io/npm/v/metalsmith-cache.svg)

![github-issues](https://img.shields.io/github/issues/leviwheatcroft/metalsmith-cache.svg)

![stars](https://img.shields.io/github/stars/leviwheatcroft/metalsmith-cache.svg)

![forks](https://img.shields.io/github/forks/leviwheatcroft/metalsmith-cache.svg)

[metalsmith][metalsmith]] helper to cache values and files between builds.

See the [annotated source][annotated source] or [github repo][github repo]

## install

`npm i --save metalsmith-cache`

## .gitignore cache

This package will persist data in a `cache` directory in your project root. You should probably `.gitignore` that directory.

## api

See [annotated source][annotated source]

### example

```javascript
import {
  FileCache,
  ValueCache
} from 'metalsmith-cache'

fileCache = new FileCache('google-drive')
valueCache = new ValueCache('google-drive-params')

Metalsmith('src')
// invalidate cache
.use((files) => {
  return fileCache.invalidate()
})
// store a file
.use((files) => {
  return fileCache.store('some/path', files[path])
})
// retrieve file & store in metalsmith files structure
.use((files) => {
  return fileCache.retrieve('some/path')
  .then((file) => files['some/path'] = file)
})
// merge all files from cache to metalsmith files structure
.use((files) => {
  return fileCache.all()
  .then((cache) => Object.assign(files, cache))
})
// fetch files according to multimatch mask
.use((files) => {
  return fileCache.mask('**/*.html')
  .then((cache) => Object.assign(files, cache))
})
// store a value
.use(() => {
  return valueCache.store('lastRun', new Date().toISOString())
})
// retrieve a value
.use(() => {
  valueCache.retrieve('lastRun')
  .then((lastRun) => console.log(lastRun))
})
.build( ... )
```

Full implementation examples:

 * [metalsmith-webpack][metalsmith-webpack]
 * [metalsmith-google-drive][metalsmith-google-drive]

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
