# Webpack Final Touches
The final touches to complete setting up Webpack is to add functionallity to the pre-written JavaScript code, tweak Production and implement Service Workers in order for the web app to work offline. 

## Functionality
The JavaScript event code is not running and the Express server is not doing anything. The main reason why the event code is not working is due to scope. Scope can be global or local (also known as function scope). Webpack likes to keep the global scope really clean (which is a good thing) and encapsulates everything inside of IIFEs (Immediately Invoked Function Expressions), that is, everything is put into function scopes and can no longer be accessed by browser events or anything in global scope for that matter. 

## Production

## Service Workers

## Review