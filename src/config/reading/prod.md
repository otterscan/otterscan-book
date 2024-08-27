# Production

In a production environment, the `VITE_CONFIG_JSON` environment variable __needs__ to be defined at build time, otherwise it has no effect; in that case, the node/chain config will be static.

That way the config won't need to be fetched from server—one less network call on page load. That's the recommended setting for controlled hosted environments where you control the node your users will connect to.

You can either:

- Make your CI `export` a `VITE_CONFIG_JSON` env variable with the desired settings.
- During build time, make sure there is a `.env.production` file with a `VITE_CONFIG_JSON` variable defined in it.
