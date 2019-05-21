# ES6 to ES5 transpilation with Babel.js and Node.js
> Transpiling _ECMAScript2015_ aka __ES6__ to __ES5__ with `Babel` from a JS entry point file, then automatically export it into a distribution folder _ready for production_ with NPM scripts

![img](https://raw.githubusercontent.com/Drozerah/MyGitHubStorage/master/img/babel-transpilation/readme-header.png)


The technique called _transpiling_ (transformation + compiling) is based on the usage of special tools to transform ES6 code into equivalent or close matches that works in ES5 environments.

This _ready to go_ configuration is aimed to give the JavaScript developer the capability to quickly set up a Node.js environment and deliver ES5 from ES6 without worrying time into the maze of `Babel` configuration options and its plugins... It gives you the minimum setting to start a new project or evaluate any new ideas and it includes one HTML code base to fulfil this objectif as well.

![img](https://raw.githubusercontent.com/Drozerah/MyGitHubStorage/master/img/babel-transpilation/readme-tree-structure.png)


#### Default Configuration

This configuration comes with :

- `package.json` _ready to go_ file (see "installation" section)
- `@babel/cli` to compile ES6 to ES5
- minification with `babel-preset-minify` - `Babel` preset used via command line
- [soure map](https://babeljs.io/docs/en/options#source-map-options) used as `@babel/cli` option
- `index.html` snippet file that imports `.dist/app.js` or `.dist/app.min.js`
- `.babelrc` file to host `Babel` configuration :
  - `@babel/preset-env` option to compile for each environment that supports ES5
  - `comments` as `boolean` option to preserve JS comments in the output code from `Babel`
- NPM scripts to :
  - __prebuild__         => create the projects structure `./dist`, `index.html`, `index.js`, `.babelrc` (all files/folder are empty, see further)
  - __clean:dist__       => clean/remove the `./dist` folder and its content before each `Babel` compilation
  - __build:dev__        => compile `index.js` to `.dist/app.js` (no minification, no source maps)
  - __build:dev:watch__  => watch and compile `index.js` to `.dist/app.js` (no minification, no source maps)
  - __build__            => compile `index.js` to `.dist/app.js` with inline source maps (no minification)
  - __build:minify__     => compile `index.js` to `.dist/app.min.js` with inline source maps + minification

#### Installation

Clone or download this repository locally then run:

```shell
$ npm install
```
It will install all the dependencies then you are ready to go...

#### Usage

Now you can write your ES6 JavaScript code into the provided `index.js` file and you have multiple NPM scripts to compile your code to ES5 depending of the evolution state of your project - development to production:

__NPM script exemple:__
```shell
$ npm run build:dev
```

- this script will compile your code as raw ES5 and then export it into the `./dist` folder as `./dist/app.js` without providing minification neither source maps (as discribed earlyer in the "Default Configuration" section of this page). All your JS comments are preserved (see further to remove JS comments for production).

__NPM scripts:__ 

- reading the `package.json` "scripts" section can help you to understand the purpose of each script:

`package.json`
```json
  //...
  "scripts": {
    "prebuild": "touch index.html && touch index.js && touch mkdir dist",
    "clean:dist": "rm -rf dist && mkdir dist",
    "build:dev": "npm run clean:dist && babel index.js --out-file dist/app.js",
    "build:dev:watch": "npm run clean:dist && babel index.js --out-file dist/app.js --watch",
    "build": "npm run clean:dist && babel index.js --out-file dist/app.js --source-maps inline",
    "build:minify": "npm run clean:dist && babel index.js --out-file dist/app.min.js --source-maps inline --presets minify"
  }
  //...
```

NPM scripts | babel output | watch task | source maps* | minification | JS comments** |
------------ | ------------- | ------------- | ------------- | ------------- | ------------- |
`build:dev` | `dist/app.js`| false | false | false | __true__ |
`build:dev:watch` | `dist/app.js`| __true__ | false | false | __true__ |
`build `| `dist/app.js`| false | __true__ | false | __true__ |
`build:minify` | __`dist/app.min.js`__| false | __true__ | __true__ | __true__ |


__Caution:__ 
  - running the `prebuild` script will overwrite the `index.html` file provided by default with the minimum required and necessary HTML tags to import your compiled JS file and run it in the broswer.
  - running `clean:dist` will remove the `./dist` folder from your root directory.

__Production import:__
  - the `build:minify` script brings minification for production, it compiles and export your JS file as `dist/app.min.js`. In order to use it, you'll need to import it in the `index.html` like so:

`index.html`
```diff
  //...
  <body>
-    <script src="./dist/app.js"></script>
+    <script src="./dist/app.min.js"></script>
  </body>
  //...
```
__* source maps:__
  - note that the generated Source Map is included in the compiled file. If you need a separated file pleaze refer to the [Babel Source Map Documentation](https://babeljs.io/docs/en/options#source-map-options)

__** JS comments for production:__
  - by default, any JS comments in the output code from `Babel` are preserved. If you want to remove them for production, you'll need to change the boolean value of the ``"comments"`` property into the `.babelrc` file like so:

`.babelrc`
```diff
  {
    //...
-   "comments": true,
+   "comments": false,
    "presets": ["@babel/preset-env"]
    //...
  }
```

#### Built With

[@babel/cli](https://babeljs.io/docs/en/babel-cli)
[babel-preset-minify](https://github.com/babel/minify/tree/master/packages/babel-preset-minify)
[Node.js](https://nodejs.org/en/)
[NPM](https://www.npmjs.com/)
[Visual Studio Code](https://code.visualstudio.com/)

#### Versioning

We use [SemVer](http://semver.org/) for versioning.

#### Author

* **Thomas G. aka Drozerah** - [Github](https://github.com/Drozerah)

#### Acknowledgments

- [Babel & preset-env](https://codeburst.io/babel-preset-env-cbc0bbf06b8f)

#### License

ISC
