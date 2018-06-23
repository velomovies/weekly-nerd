[Vorige](/Artikelen/De_eeuwige_discussie_artikel.md)

# The NPM Hell

Npm is the ‘world’s largest’ package manager for JavaScript used by a lot of front-enders. We wanted for our ‘meesterproef’ a good boilerplate and decided to have a great own made package.json. By making our own boilerplate it was particularly problem solving.

We chose npm, because it has really extensive documentation. That makes it easier to get a good working boilerplate. We started with an empty package.json and started to get the most necessary npm packages.  We used a few things few dependencies for building our app. 

`Eslint && PostCSS && rollup` are used in our boilerplate. With npm-scripts we called them and they start to build each time we start the script. With `nodemon` we checked all files so it restarts the server when there is a change. By chaining all scripts in “start” we let run everything. `"start": "npm run rollup & npm run css & npm run lint & npm run nodemon"`. The problem is that it doesn’t rebuild when there is a change.

```
{
 "name": "Boegle",
 "version": "1.0.0",
 "description": "Amsterdam OBA | To search a book",
 "main": "index.js",
 "scripts": {
   "test": "echo \"Error: no test specified\" && exit 1",
   "lint": "eslint app.js",
   "rollup": "rollup src/js/main.js --o dist/js/bundle.js --f iife",
   "css": "postcss src/styles/style.css -o dist/styles/style.css",
   "nodemon": "nodemon --inspect app.js",
   "start": "npm run rollup & npm run css & npm run lint & npm run nodemon"
 },
 "repository": {
   "type": "git",
   "url": "git+https://github.com/Boegle/Boegle.git"
 },
 "keywords": [],
 "author": "",
 "license": "ISC",
 "bugs": {
   "url": "https://github.com/Boegle/Boegle/issues"
 },
 "homepage": "https://github.com/Boegle/Boegle#readme",
 "devDependencies": {
   "dotenv": "^6.0.0",
   "ejs": "^2.6.1",
   "eslint": "^4.19.1",
   "express": "^4.16.3",
   "nodemon": "^1.17.5",
   "postcss-cli": "^5.0.0",
   "rollup": "^0.59.4"
 }
}
```

This was our first package.json. It worked alright, but there where a few problems. It did build all things we needed. But because it didn’t watch the js and css you had to restart the script each time. 

To get them working we updated the package.json:

```
{
 "name": "Boegle",
 "version": "1.0.0",
 "description": "Amsterdam OBA | To search a book",
 "main": "index.js",
 "scripts": {
   "test": "echo \"Error: no test specified\" && exit 1",
   "lint": "eslint app.js",
   "rollup": "rollup src/js/main.js --o dist/js/bundle.js --f iife",
   "css": "postcss src/styles/style.css -o dist/styles/style.css",
   "nodemon": "nodemon --inspect app.js",
   "start": "npm run rollup & npm run css & npm run lint & npm run watch  & npm run nodemon",
   "watch": "nodemon --watch src/** -e css,js -x \"npm run rollup & npm run css & npm run nodemon\""
 },
 "repository": {
   "type": "git",
   "url": "git+https://github.com/Boegle/Boegle.git"
 },
 "keywords": [],
 "author": "",
 "license": "ISC",
 "bugs": {
   "url": "https://github.com/Boegle/Boegle/issues"
 },
 "homepage": "https://github.com/Boegle/Boegle#readme",
 "devDependencies": {
   "dotenv": "^6.0.0",
   "ejs": "^2.6.1",
   "eslint": "^4.19.1",
   "express": "^4.16.3",
   "node-fetch": "^2.1.2",
   "nodemon": "^1.17.5",
   "postcss-cli": "^5.0.0",
   "rollup": "^0.59.4",
   "xml2js": "^0.4.19"
 },
 "dependencies": {
   "socket.io": "^2.1.1"
 }
}
```

As you can see we added a few dependencies that where needed to make the application work. We also changed the “start”. We did a little research to see what is the best solution to the problems we had. This makes it possible to lets it automatically rebuild when there is a change to the js and css. Unfortunately it still got a problem. 

Nodemon is using a certain way to watch. In the code we made nodemon is started two times. This immediately started a problem. It gave a few errors. We changed the “start” again and ended up with this: 

```
"scripts": {
   "test": "echo \"Error: no test specified\" && exit 1",
   "lint": "eslint app.js",
   "rollup": "rollup src/js/main.js --o dist/js/bundle.js --f iife",
   "css": "postcss src/styles/style.css -o dist/styles/style.css",
   "nodemon": "nodemon --inspect app.js --ignore dist/**",
   "start": "npm run lint & npm run watch",
   "watch": "nodemon --watch src/** -e css,js -x \"npm run rollup & npm run css & npm run nodemon\""
 }
```

This was a good working code for our windows users. Except the macbooks did get an annoying error. It took us half a day to get a solution. The problem was a `/`. Adding that let it work for macbooks. 

With our NPM all done we could finally focus on our cool project for the ‘meesterproef.’

## Conclusion

By making our own packages.json and NPM scripts for the ‘meesterproef’, we made sure we learned a lot about how building tools are working. You can choose yourself a best way, but for us was using NPM scripts the best way. It is a vanilla way to learn how everything is working. As you read we did a lot of problem solving. That is a important thing in making code. You always get errors and to know where to find the solution is important. Next to that you better learn what does everything, how works it and how can we make it work the best for us.

## Sources

* https://www.npmjs.com/
* https://github.com/Boegle/Boegle

