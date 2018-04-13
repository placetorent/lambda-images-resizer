Images resizing with AWS Lambda
 ---
 [!
 [!
 [!

 ## Install
 ```
 yarn add lambda-images-resizer
 ```
 
 ## Example of  index.js file of lambda function 
 ```js 
 const HttpError = require('lambda-images-resizer').HttpError  
-const Handler = new require('lambda-images-resizer').Handler 
-const handle = new Handler({ 
+const createProcessor = new require('lambda-images-resizer') 
+const processor = createProcessor({ 
   region: process.env.REGION, 
   bucket: process.env.BUCKET, 
   cachePrefix: process.env.CACHE_PREFIX, 
   signatureVerification: process.env.SIGNATURE_VERIFICATION === '1',
   signatureSecret: process.env.SIGNATURE_SECRET,
   cdnUrl: process.env.CDN_URL,
   maxWidth: parseInt(process.env.MAX_WIDTH, 10) || 1920,
   maxHeight: parseInt(process.env.MAX_HEIGHT, 10) || 1080,
   logging: process.env.LOGGING === '1'
 }) 
 const handler = function(event, context) { 
-  handle(event) 
+  processor(event) 
     .then((response) => { 
      context.succeed(response) 
     }) 
     .catch((err) => {
       if (err instanceof HttpError) {
         return context.succeed({
           statusCode: err.statusCode,
           body: err.message
         })
       }
       retu@@ -1,19 +1,19 @@
 Images resizing with AWS Lambda
 ---
 [!
 [!
 [!

 ## Install
 ```
 yarn add lambda-images-resizer
 ```
 ## Example of  index.js file of lambda function  
 ```js 
 const HttpError = require('lambda-images-resizer').HttpError 
-const Handler = new require('lambda-images-resizer').Handler 
-const handle = new Handler({ 
+const createProcessor = new require('lambda-images-resizer') 
+const processor = createProcessor({ 
   region: process.env.REGION,  
   bucket: process.env.BUCKET,  
   cachePrefix: process.env.CACHE_PREFIX, 
   signatureVerification: process.env.SIGNATURE_VERIFICATION === '1',
   signatureSecret: process.env.SIGNATURE_SECRET,
   cdnUrl: process.env.CDN_URL,
   maxWidth: parseInt(process.env.MAX_WIDTH, 10) || 1920,
   maxHeight: parseInt(process.env.MAX_HEIGHT, 10) || 1080,
   logging: process.env.LOGGING === '1'
 }) 
 const handler = function(event, context) { 
-  handle(event) 
+  processor(event) 
     .then((response) => { 
       context.succeed(response) 
     }) 
     .catch((err) => {
       if (err instanceof HttpError) {
         return context.succeed({
           statusCode: err.statusCode,
           body: err.message
         })
       }
       return context.fail(err)
     })
 }

 exports.handler = handler
 ```

 ## To-do list:
 - [x] Resizing constraints
 - [x] Signatures checkingrn context.fail(err)
     })
 }
 exports.handler = handler
 ```
 
 ## To-do list:
 - [x] Resizing constraints
 - [x] Signatures checking
