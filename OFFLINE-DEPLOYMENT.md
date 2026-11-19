# Offline deployment

| [Overview](README.md) | **Offline deployment** |
| :---: | :---: |

Serve the packaged files from GeoEvent Server (recommended) or another static web server. The browser processes the XML locally; the offline tool does not require ArcGIS sign-in or Node.js. Links to online product help still require internet access.

## Deploy offline using GeoEvent Server (recommended)

1. On a connected machine, visit [Releases](https://github.com/EsriPS/velocity-migration/releases). Once a release is published, download `velocity-migration-tool-offline-<release>.zip` under **Assets**, not GitHub's **Source code** ZIP. Transfer the package to your environment.
2. Extract the ZIP **outside** the published assets directory. From the extracted release folder, verify the files against `CHECKSUMS.txt` (for example, `shasum -a 256 -c CHECKSUMS.txt`). Keep the manifest, license and notices with your deployment records.
3. Copy the complete `velocitymigration/` folder into your GeoEvent Server site's published assets directory. The result should be `assets/velocitymigration/index.html` alongside `assets/velocitymigration/assets/`. Do not flatten the folders or put XML exports in the served directory. No separate web server is needed.
4. Open `https://gisserver.domain.com:6143/geoevent/assets/velocitymigration/index.html` in a browser, substituting your site's GeoEvent Server hostname and HTTPS route as needed. Opening the file directly (`file://`) prevents the migration worker from starting.
5. Check access from a signed-out browser as well as an authorized one: the hosting site, not the offline tool, controls who can open the page. With internet access blocked, select a non-sensitive test XML, run a migration, download the Velocity JSON and open the HTML migration guide.

Check the URL again after a GeoEvent upgrade or repair. To remove the tool or restore a previous version, change only the `velocitymigration/` folder, not the parent assets directory.

## Alternative: use your own web server

Follow your server's instructions for hosting a static JavaScript application over HTTPS. Copy the same `velocitymigration/` folder into the site's document root and open `/velocitymigration/index.html` on that host. See the [self-hosting appendix](#appendix-self-hosting-on-windows-or-linux) for examples.

## Appendix: Self-hosting on Windows or Linux

Use an existing HTTPS site with an approved certificate and the access controls your organization requires. The release ZIP contains built files: do not run a build, install a JavaScript runtime or configure a backend. Preserve the folder structure and serve files as files, not through a rewrite that returns `index.html` for missing assets.

| Web server | Where to copy `velocitymigration/` | Site configuration |
| --- | --- | --- |
| IIS (Windows) | The site's physical root, for example `C:\inetpub\wwwroot` | Enable Static Content and use the site's HTTPS binding. Map `.json` to `application/json` and `.mjs` to `text/javascript` if those mappings are missing. |
| nginx (Windows or Linux) | The configured `root`, for example `C:\nginx\html` or `/usr/share/nginx/html` | Load the server's MIME types and serve the directory as static files. On SELinux systems, ensure the deployed files have a web-readable content label. Test changes with `nginx -t` before reloading. |
| Tomcat (Windows or Linux) | The host's application base, typically `webapps` | With standard directory deployment enabled, `velocitymigration` becomes the context name. Use the site's existing HTTPS connector or reverse proxy. |

Browse to `https://host.example/velocitymigration/index.html`, replacing the host and port with your site's values. If the page loads but migration does not start, use the browser's Network panel to check that `assets/wizard.js`, `assets/migration-worker.js` and `assets/velocity-icons.json` return HTTP 200 from that site with JavaScript or JSON content types, not an HTML error or sign-in page. Repeat the disconnected migration check above before sharing the URL.
