# Webpack Final Touches
The final touches to complete setting up Webpack is to add functionallity to the pre-written JavaScript code, tweak production and implement service workers in order for the web app to work offline.

## Functionality
The JavaScript event code is not working when running `npm run build-dev` in the Command Line. The browser window should open using `localhost:8080` as the URL and the CSS styles should load after clicking OK on the alert 'I exist!'. This is great, but the result after entering Picard, Janeway, Kirk, Archer or Georgiou as input in the form field throws up the same alert 'I exist!' again instead of 'Welcome, Captain!' and there is no result added to the section Form Results underneath the input field.

### Scope
The main reason why the event code is not working is due to scope. Scope can be global or local (also known as function scope). Webpack likes to keep the global scope really clean (which is a good thing) and encapsulates everything inside of IIFEs (Immediately Invoked Function Expressions), that is, everything is put into function scopes and can no longer be accessed by browser events or anything in global scope for that matter (which is a bad thing). To solve this problem, a library needs to be set up and configured in order to gain access to the encapsuled event listeners. Below are the steps required to get the alert 'Welcome, Captain!' working:

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
<form class="" onsubmit="return Client.handleSubmit(event)">
    <input id="name" type="text" name="input" value="" onblur="onBlur()" placeholder="Name">
    <input type="submit" name="" value="submit" onclick="return Client.handleSubmit(event)" onsubmit="return Client.handleSubmit(event)">
</form>
```
#### src/client/js/formHandler.js
Change `checkForName(formText)` to `Client.checkForName(formText)`:
```js
Client.checkForName(formText)
```

### Test
#### Development Mode
Build Webpack in development mode and rev up the Webpack Dev Server:
```
npm run build-dev
```
Press OK on the 'I exist!' alert. Enter Picard, Janeway, Kirk, Archer or Georgiou in the input field and click the submit button. Note that if spelt incorrectly or if anything else is entered no alert will pop up. Press OK on the 'Welcome, Captain!' alert and and stop the Webpack Dev Server with `control + C` in the Command Line to finish testing:
```
control + C
```

#### Production Mode
Build Webpack in production mode:
```
npm run build-prod
```
Run the Express server:
```
npm start
```
Test the same way as before and stop the Express server with `command + C` to finish testing:
```
control + C
```
In both tests, the alert 'Welcome, Captain!' should pop up when one of the correct names has been enetered and submitted. However, it is also evident that the Form Results section is still empty in both development and production modes.

### Server
So, the input field is now working and the Form Results section is not. With the current setup, Form Results will never work in development mode, because no `dist` folder is built and never will be built. Production mode does build a `dist` folder. However, the Form Results section still does not work once the Express server is started, because both the Webpack Dev Server and the Express server use `localhost:8080`. The answer is to use a different localhost for either server. Below is a set of instructions on how to tweak the setup for the Express server:

#### formHandler
Change the localhost in the fetch request from `localhost:8080` to `localhost:8081`:
```js
fetch('http://localhost:8081/test')
```
#### src/server/index.js
 Change the localhost in the server from `localhost:8080` to `localhost:8081`:
 ```js
app.listen(8081, function () {
    console.log('Example app listening on port 8081!')
})
 ```

### Test
Run production mode and rev up the server:
```
npm run build-prod
npm start
```
A `dist` folder should have been built and the console should state 'Example app listening in port 8081!'. Switch to the browser, enter the required input in the input field, press the submit button and click OK on the 'Welcome, Captain' alert. This time Form Results should show the message 'this is a message'.

Stop the Express server:
```
control + C
```
Delete `dist` folder:
```
rm -rf dist
```

## Production
Now that development mode is up and running it is time to focus on production mode. Production mode is all about keeping things as small and light as possible. The biggest contributers to slow websites/web apps are, JavaScript, styles (CSS) and large/many images. Webpack already minifies (compresses) JavaScript into one `main.js` file with one illegible line of code. What Webpack currently does with styles (CSS) is that the plugin `style-loader` adds them inline to the HTML tags in `index.html`. A better way is for Webpack to build a minified `main.css` file and for that the following steps will need to be implemented:

### Installation
Install the following NPM packages:
```
npm i -D --legacy-peer-deps mini-css-extract-plugin@1.4.1
npm i -D --legacy-peer-deps terser-webpack-plugin@5.1.1
npm i -D --legacy-peer-deps optimize-css-assets-webpack-plugin@5.0.4
```

### Configuration

### Test

## Service Workers

## Review