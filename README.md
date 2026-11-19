<p align="center">
  <img src="banner.png" alt="ArcGIS Velocity Migration Tool" width="100%">
</p>

<p align="center">
  Migrate your ArcGIS GeoEvent Server configurations to ArcGIS Velocity.
</p>

<p align="center">
  <a href="https://github.com/EsriPS/velocity-migration/issues/new/choose">Submit an Issue</a>
  &nbsp;·&nbsp;
  <a href="https://github.com/EsriPS/velocity-migration/discussions">Join the Discussion</a>
  &nbsp;·&nbsp;
  <a href="https://velocitymigration.arcgis.com/guide/">Read the User Guide</a>
  &nbsp;·&nbsp;
  <a href="https://velocitymigration.arcgis.com">Open the App</a>
  &nbsp;·&nbsp;
  <a href="https://github.com/EsriPS/velocity-migration/releases">Offline Releases</a>
</p>

---

## Choose how to run the tool

- **Hosted:** [Open the app](https://velocitymigration.arcgis.com) and sign in with ArcGIS. No installation is needed.
- **Offline:** Serve the packaged files from GeoEvent Server (recommended) or another static web server. The browser processes the XML locally; the offline tool does not require ArcGIS sign-in or Node.js. Links to online product help still require internet access.

### Deploy offline using GeoEvent Server (recommended)

1. On a connected machine, visit [Releases](https://github.com/EsriPS/velocity-migration/releases). Once a release is published, download `velocity-migration-tool-geoevent-assets-<release>.zip` under **Assets**, not GitHub's **Source code** ZIP. Transfer the package to your environment.
2. Extract the ZIP **outside** the published assets directory. From the extracted release folder, verify the files against `CHECKSUMS.txt` (for example, `shasum -a 256 -c CHECKSUMS.txt`). Keep the manifest, license and notices with your deployment records.
3. Copy the complete `velocitymigration/` folder into your GeoEvent Server site's published assets directory. The result should be `assets/velocitymigration/index.html` alongside `assets/velocitymigration/assets/`. Do not flatten the folders or put XML exports in the served directory. No separate web server is needed.
4. Open `<your-published-assets-HTTPS-URL>/velocitymigration/index.html` in a browser. Use the same HTTPS prefix your users use for other published GeoEvent assets, not a local filesystem path. Opening the file directly (`file://`) prevents the migration worker from starting.
5. Check access from a signed-out browser as well as an authorized one: the hosting site, not the offline tool, controls who can open the page. With internet access blocked, select a non-sensitive test XML, run a migration, download the Velocity JSON and open the HTML migration guide.

On multi-node sites, verify that each serving node has the files. Check the URL again after a GeoEvent upgrade or repair. To remove the tool or restore a previous version, change only the `velocitymigration/` folder, not the parent assets directory.

### Alternative: use your own web server

Follow your server's instructions for hosting a static JavaScript application over HTTPS. Copy the same `velocitymigration/` folder into the site's document root and open `/velocitymigration/index.html` on that host. See the [self-hosting appendix](#appendix-self-hosting-on-windows-or-linux) for examples.

## Feedback & Support

Use this repository for bug reports, feature requests and questions about **both hosted and offline versions**. Application source is maintained separately.

> **How this tool is supported:** The ArcGIS Velocity Migration Tool is maintained by the **ArcGIS Velocity Product Team** and is **not** supported by Esri Technical Support. Please direct all questions, bug reports, and feature requests about the tool to this repository and its [Discussions](https://github.com/EsriPS/velocity-migration/discussions).
>
> This applies to the hosted migration tool itself. Esri Technical Support can still help with the underlying products and content — for example, exporting a GeoEvent Server configuration, importing the generated ArcGIS Velocity configuration, or troubleshooting items after they have been imported into Velocity.

### Found a bug?

[Open a bug report](https://github.com/EsriPS/velocity-migration/issues/new?template=bug_report.yml). Include how you ran the tool (hosted, GeoEvent assets or another web server), the release, browser and steps to reproduce.

### Have an idea?

[Open a feature request](https://github.com/EsriPS/velocity-migration/issues/new?template=feature_request.yml) to suggest an enhancement or new capability.

### Have a question?

Start a conversation in [Discussions](https://github.com/EsriPS/velocity-migration/discussions). It's the best place to ask questions, share tips, and work through migration challenges with others.

### Stay up to date

Release news is posted in the [Announcements](https://github.com/EsriPS/velocity-migration/discussions/categories/announcements) category. Each release mirrors the [User Guide Changelog](https://velocitymigration.arcgis.com/guide/#changelog), so you can follow along as the tool evolves.

### Writing a good report

- **Search first.** Someone may have already reported the same thing — a reaction or comment on an existing issue helps us prioritize.
- **Be specific.** Include what you expected to happen and what actually happened.
- **Redact sensitive information.** Please remove tokens, credentials, and any private data from screenshots and logs before posting.

## Protecting sensitive data

This is a public repository. **Please do not post GeoEvent Server configuration exports (XML), credentials, tokens, hostnames, or other sensitive data** in issues or discussions — anything shared here is publicly visible and effectively permanent.

If troubleshooting requires your configuration or other private details, **[start a discussion](https://github.com/EsriPS/velocity-migration/discussions)** and the maintainers will follow up privately to arrange a secure, private file transfer. Do not post the sensitive data in the public thread.

To report a security vulnerability, please see [SECURITY.md](SECURITY.md).

## Contributing

This repository distributes offline releases and welcomes feedback, but does not accept application code contributions. See
[CONTRIBUTING.md](CONTRIBUTING.md) for how to file bugs, request features, and
ask questions, and our [Code of Conduct](CODE_OF_CONDUCT.md) for community
expectations.

## Maintainers & contact

This repository is maintained by the ArcGIS Velocity Product Team.

- **Product help or private details:** [start a discussion](https://github.com/EsriPS/velocity-migration/discussions); for anything involving sensitive data, the maintainers will follow up privately to arrange a secure transfer.
- **Repository, access, or organization questions:** ps_github_admins@esri.com

## License

Licensed under the [Apache License 2.0](LICENSE).

## Appendix: Self-hosting on Windows or Linux

Use an existing HTTPS site with an approved certificate and the access controls your organization requires. The release ZIP contains built files: do not run a build, install a JavaScript runtime or configure a backend. Preserve the folder structure and serve files as files, not through a rewrite that returns `index.html` for missing assets.

| Web server | Where to copy `velocitymigration/` | Site configuration |
| --- | --- | --- |
| IIS (Windows) | The site's physical root, for example `C:\inetpub\wwwroot` | Enable Static Content and use the site's HTTPS binding. Map `.json` to `application/json` and `.mjs` to `text/javascript` if those mappings are missing. |
| nginx (Windows or Linux) | The configured `root`, for example `C:\nginx\html` or `/usr/share/nginx/html` | Load the server's MIME types and serve the directory as static files. On SELinux systems, ensure the deployed files have a web-readable content label. Test changes with `nginx -t` before reloading. |
| Tomcat (Windows or Linux) | The host's application base, typically `webapps` | With standard directory deployment enabled, `velocitymigration` becomes the context name. Use the site's existing HTTPS connector or reverse proxy. |

Browse to `https://host.example/velocitymigration/index.html`, replacing the host and port with your site's values. If the page loads but migration does not start, use the browser's Network panel to check that `assets/wizard.js`, `assets/migration-worker.js` and `assets/velocity-icons.json` return HTTP 200 from that site with JavaScript or JSON content types, not an HTML error or sign-in page. Repeat the disconnected migration check above before sharing the URL.
