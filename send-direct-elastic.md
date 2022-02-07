# send direct elastic

## install package
````
npm install winston
npm install @elastic/ecs-winston-format
npm install got
````

## code `webserver.js`
````
const http = require('http')
const winston = require('winston')
const ecsFormat = require('@elastic/ecs-winston-format')

const logger = winston.createLogger({
  level: 'debug',
  format: ecsFormat({ convertReqRes: true }),
  transports: [
    //new winston.transports.Console(),
    new winston.transports.File({
      //path to log file
      filename: 'logs/log.json',
      level: 'debug'
    })
  ]
})

const server = http.createServer(handler)
server.listen(3000, () => {
  logger.info('listening at http://localhost:3000')
})

function handler (req, res) {
 res.setHeader('Foo', 'Bar')
  res.end('ok')
  logger.info('handled request', { req, res })
}
````

## run
````
node webserver.js
````

## tip for filebeat run
````
./filebeat -e -c filebeat.yml
````


https://www.elastic.co/guide/en/cloud/current/ec-getting-started-search-use-cases-node-logs.html
