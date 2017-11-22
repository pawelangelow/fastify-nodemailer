# fastify-nodemailer

[![js-standard-style](https://img.shields.io/badge/code%20style-standard-brightgreen.svg?style=flat)](http://standardjs.com/)
[![Build Status](https://travis-ci.org/lependu/fastify-nodemailer.svg?branch=0.1.0)](https://travis-ci.org/lependu/fastify-nodemailer)

Fastify nodemailer plugin, with this you can share the same nodemailer transport in every part of your server.

Under the hood the it wraps [nodemailer](https://github.com/nodemailer/nodemailer) transporter and the options that you pass to `register` will be passed to the transporter. For configuration/usage details please check the [nodemailer documentation](https://nodemailer.com/usage/).

## Install
```
npm i fastify-nodemailer --save
```

## Usage
Add it to you project with `register` and you are done!
You can access the transporter via `fastify.nodemailer` and *sendMail()* via `fastify.nodemailer.sendMail()`.
```js
const fastify = require('fastify')()

fastify.register(require('fastify-nodemailer'), {
  pool: true,
  host: 'smtp.example.com',
  port: 465,
  secure: true, // use TLS
  auth: {
      user: 'username',
      pass: 'password'
  }
})

fastify.get('/sendmail/:email', (req, reply, next) => {
  let { nodemailer } = fastify
  let recipient = req.params.email

  fastify.nodemailer.sendMail({
    from: 'sender@example.com',
    to: recipient,
    subject: 'foo',
    text: 'bar'
  }, (err, info) => {
    if (err) next(err)
    reply.send({
      messageId: info.messageId
    })
  })
})

fastify.listen(3000, err => {
  if (err) throw err
  console.log(`server listening on ${fastify.server.address().port}`)
})
```

## License

Licensed under [MIT](./LICENSE).
