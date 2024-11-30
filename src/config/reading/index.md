# Specifying the configuration

There are multiple ways to configure Otterscan base settings, depending on how you want to run it.

## Static hardcoded node/chain ID

Define a `VITE_CONFIG_JSON` environment variable containing a JSON string with the entire config.

- [During development time](./devel.md)
- [For production](./prod.md)

## Fetch config from server

If you don't specify a `VITE_CONFIG_JSON` variable, the dapp will fetch a `<your-domain>/config.json` file on page load.

That file can be overwritten server-side and changes will be reflected when users refresh the page.

You are free to define the best way to do that depending on how you package your Otterscan distribution.

For reference, our official Docker image accepts initialization parameters that overwrite that file when the container is initialized.
