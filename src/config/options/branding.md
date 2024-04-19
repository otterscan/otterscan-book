# Branding

Integrators can customize some aspects of the Otterscan UI in order to match their brand.

## Customization

Some components in the user interface can be customized in the config under the `branding` key:

```json
{
  "branding": {
    "siteName": "Otterscan",
    "networkTitle": "Holesky Testnet",
    "hideAnnouncements": false
  }
}
```

* `siteName`: Sets the name displayed on the home page, header, and page titles.
* `networkTitle`: If set, adds an additional name to page titles.
* `hideAnnouncements`: If set to true, hides new feature announcements from the home page.

## Logo

To replace the default Otterscan logo with your own, simply replace `src/otter.png` with a different PNG image. Rebuild Otterscan for the change to take effect.
