const HttpError = require('lambda-images-resizer').HttpError
const createProcessor = new require('lambda-images-resizer')
const processor = createProcessor({
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
  processor(event)
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
