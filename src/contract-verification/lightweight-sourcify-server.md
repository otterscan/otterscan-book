# Lightweight Sourcify repository hosting

Sourcify publishes a compressed RepositoryV2 weekly, with contracts from all chains. You can follow [its instructions](https://docs.sourcify.dev/docs/repository/file-repositories/#download) to download the entire repository yourself and self-host it:

```sh
curl -L -O https://repo-backup.sourcify.dev/manifest.json
jq -r '.files[].path' manifest.json | xargs -I {} curl -L -O https://repo-backup.sourcify.dev/{}
cat sourcify-repository-*.part.gz.* | tar -xz
```

### Hosting the repository locally

You can host the extracted repository using Caddy, a lightweight, portable HTTP(S) server:

1. Install Caddy. You can find installation instructions on the [official Caddy website](https://caddyserver.com/docs/install).
2. Run a Caddy file server on `localhost:7877`:

   ```sh
   caddy file-server --root ./repositoryV2/ --listen 127.0.0.1:7877
   ```

   <details>
     <summary>Alternatively, use a Caddyfile instead</summary>
     
     1. Create a Caddyfile. In the same directory used to run the download commands, create a file named `Caddyfile` with the following content:
         
         ```caddyfile
         # Caddyfile for hosting Sourcify RepositoryV2
         localhost:7877 {
             # Set the root directory to the extracted repository folder
             root * ./repositoryV2
         
             # Serve files from the repository
             file_server
         }
         ```
     
     2. In the directory containing the `Caddyfile`, run
         ```sh
         caddy run
         ```
     
     </details>

3. Update the Otterscan config. Your Sourcify repository is now hosted at `http://localhost:7877`. Add an entry in your Otterscan config to point to your new repository URL:

   ```json
   "sourcify": {
     "sources": {
       "Local Sourcify Repo": {
         "url": "http://localhost:7877",
         "backendFormat": "RepositoryV2"
       },
       "Sourcify Servers": {
         "url": "https://repo.sourcify.dev",
         "backendFormat": "RepositoryV1"
       }
     }
   }
   ```

   Replace `http://localhost:7877` with your server's domain or IP address if you're accessing it remotely. If you want to host your repo over HTTPS, follow Caddy's instructions at <https://caddyserver.com/docs/automatic-https>.
