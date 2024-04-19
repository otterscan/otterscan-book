# Enabling the CL REST API

Instructions for enabling the REST API are dependent on which CL implementation you are using.

It is out-of-scope of this document to explain how it is done since there are many CL implementations. Please check their docs.

We'll provide instructions for Lighthouse and Caplin as an example since they are the implementations we use in our tests.

But the key points you need to observe are:

- Default port number: that is not standardized, Lighthouse uses `5052`, Prysm uses `3500`, etc.
- Bind the REST webserver to the correct network interface: it is usually set to `localhost` for security reasons, be sure to explicitly bind it to your network interface in order to your browser to reach it. You'll most likely want to bind it to `0.0.0.0`.
- Enable CORS.

Example instructions:

- [Lighthouse](./lighthouse.md)
- [Caplin](./caplin.md)
