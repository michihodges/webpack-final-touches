# Webpack Final Touches
The final touches to complete setting up Webpack is to add functionallity to the pre-written JavaScript code, tweak Production and implement Service Workers in order for the web app to work offline. 

## Functionality
The JavaScript event code is not working when running `npm run build-dev` in the Command Line. The browser window should open using `localhost:8080` as the URL and the CSS styles should load after clicking OK on the alert 'I exist!'. This is great, but the result after entering Picard, Janeway, Kirk, Archer or Georgiou as input in the form field throws up the same alert 'I exist!' again instead of 'Welcome, Captain!' and there is no result added to the section Form Results underneath the input field.

### Scope
The main reason why the event code is not working is due to scope. Scope can be global or local (also known as function scope). Webpack likes to keep the global scope really clean (which is a good thing) and encapsulates everything inside of IIFEs (Immediately Invoked Function Expressions), that is, everything is put into function scopes and can no longer be accessed by browser events or anything in global scope for that matter.

#### webpack.dev.js
Add an `output` object between the `entry` and `module` objects:
```js
output: {
        libraryTarget: 'var',
        library: 'Client'
    },
```
#### webpack.prod.js
Add the same to `webpack.prod.js`:
```js
output: {
        libraryTarget: 'var',
        library: 'Client'
    },
```
#### src/client/index.js
Export `checkForName` and `handleSubmit`:
```js
export {
    checkForName,
    handleSubmit
}
```
#### src/client/views/index.html
Change every `handleSubmit(event)` to `Client.handleSubmit(event)`:
```html
<!--Old-->
<form class="" onsubmit="return handleSubmit(event)">
    <input id="name" type="text" name="input" value="" onblur="onBlur()" placeholder="Name">
    <input type="submit" name="" value="submit" onclick="return handleSubmit(event)" onsubmit="return handleSubmit(event)">
</form>
```
```html
<!--New-->
<form class="" onsubmit="return Client.handleSubmit(event)">
    <input id="name" type="text" name="input" value="" onblur="onBlur()" placeholder="Name">
    <input type="submit" name="" value="submit" onclick="return Client.handleSubmit(event)" onsubmit="return Client.handleSubmit(event)">
</form>
```
#### src/client/js/formHandler.js
Change `checkForName(formText)` to `Client.checkForName`:
```js
// Old
checkForName(formText)
```
```js
// New
Client.checkForName(formText)
```

### Server
The Express server is not doing anything, because `localhost:8080` is being used by the Webpack Dev Server as well as the Exxpress server. Two servers cannot be run on the same localhost. To affirm that the Express server is not doing anything, run `npm start` in the Command Line and it will be evident that there is no CSS styling.

#### formHandler

#### src/server/index.js

## Production

## Service Workers

## Review