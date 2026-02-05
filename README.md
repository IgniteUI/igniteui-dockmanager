![ignite-ui-logo-flames](https://user-images.githubusercontent.com/52001020/173773052-e8fd2806-2631-47a8-838d-1eabdaa4afce.svg)
# Ignite UI Dock Manager Web Component - from Infragistics
[![npm version](https://badge.fury.io/js/igniteui-dockmanager.svg)](https://badge.fury.io/js/igniteui-dockmanager)
[![Discord](https://img.shields.io/discord/836634487483269200?logo=discord&logoColor=ffffff)](https://discord.gg/39MjrTRqds)

[Ignite UI Dock Manager Web Component](https://www.infragistics.com/) provides means to manage the layout of your application through panes, allowing your end-users to customize it further by pinning, resizing, moving and hiding panes.

![DockManagerDemo](https://user-images.githubusercontent.com/11231206/209106239-a435998a-6c01-45bf-adc3-030dbf39f2ae.gif)

> [!IMPORTANT]
> **Version 2.0+ Notice:** The Dock Manager has been migrated from Stencil to **Lit**. This is an internal architectural change that maintains full API compatibility with previous versions. For applications using version 1.x, please see the migration guide below.

## NPM Package

You can include the Ignite UI Dock Manager Web Component in your project as a dependency using the NPM package.

```
npm install igniteui-dockmanager --save
```

## Usage

### Lit Version (v2.0+)

In the new Lit-based version, you simply import the component and call the `defineComponents()` function:

```ts
import { defineComponents, IgcDockManagerComponent } from 'igniteui-dockmanager';

defineComponents(IgcDockManagerComponent);
```

### Legacy Stencil Version (v1.x)

If you are using version 1.x, it is necessary to import and call the `defineCustomElements()` function:

```ts
import { defineCustomElements } from 'igniteui-dockmanager/loader';

defineCustomElements();
```

### Adding to the Page

Once the Dock Manager is imported, you can add it on the page:

```html
<igc-dockmanager id="dockManager">
</igc-dockmanager>
```

### Localization

Starting with version 2.0, localization resources have been moved to a separate package. To localize the Dock Manager strings, install the peer dependency `igniteui-i18n-resources` and register a language bundle with `igniteui-i18n-core`:

```bash
npm install igniteui-i18n-resources --save
```

```ts
import { registerI18n } from 'igniteui-i18n-core';
import { DockManagerResourceStringsES } from 'igniteui-i18n-resources';

registerI18n(DockManagerResourceStringsES, 'es');
```

More information on how to use the Ignite UI Dock Manager Web Component can be found [here](https://infragistics.com/products/ignite-ui-web-components/web-components/components/dock-manager.html).

## Browser Support

![chrome_48x48](https://user-images.githubusercontent.com/2188411/168109445-fbd7b217-35f9-44d1-8002-1eb97e39cdc6.png) | ![firefox_48x48](https://user-images.githubusercontent.com/2188411/168109465-e46305ee-f69f-4fa5-8f4a-14876f7fd3ca.png) | ![edge_48x48](https://user-images.githubusercontent.com/2188411/168109472-a730f8c0-3822-4ae6-9f54-785a66695245.png) | ![opera_48x48](https://user-images.githubusercontent.com/2188411/168109520-b6865a6c-b69f-44a4-9948-748d8afd687c.png) | ![safari_48x48](https://user-images.githubusercontent.com/2188411/168109527-6c58f2cf-7386-4b97-98b1-cfe0ab4e8626.png)
--- | --- | --- | --- | --- |
Latest ✔ | Latest ✔ | Latest ✔ | Latest ✔ | Latest ✔ |

## Feedback
 - Request a [new feature](https://github.com/IgniteUI/igniteui-dockmanager/issues/new?assignees=igdmdimitrov&labels=feature-request%2C+status%3A+in-review&template=feature_request.md&title=)
 - Upvote [an existing feature request](https://github.com/IgniteUI/igniteui-dockmanager/issues?q=is%3Aissue+is%3Aopen+is%3Aopen+is%3Aissue+label%3Afeature-request)
 - [File a bug report](https://github.com/IgniteUI/igniteui-dockmanager/issues/new?assignees=igdmdimitrov&labels=bug%2C+status%3A+in-review&template=bug_report.md&title=) and be specific about the behavior you want to see changed.

## Support

Developer support is provided as part of the commercial, paid-for license via [Infragistics Forums](https://www.infragistics.com/community/forums/), or via Chat & Phone with a Priority Support license.  To acquire a license for paid support or Priority Support, please visit this [page](https://www.infragistics.com/how-to-buy/product-pricing#developers).

## License

This is a commercial product, requiring a valid paid-for license for commercial use.  
This product is free to use for non-commercial educational use for students in K through 12 grades or University programs, and for educators to use in a classroom setting as examples / tools in their curriculum.
In order for us to verify your eligibility for free usage, please [register for trial](https://www.infragistics.com/free-downloads) and open a support ticket with a request for free license.

To acquire a license for commercial usage, please [register for trial](https://www.infragistics.com/free-downloads) and refer to the purchasing options in the pricing section on the product page.  

© Copyright 2020 INFRAGISTICS. All Rights Reserved. 
The Infragistics Ultimate license & copyright applies to this distribution. 
For information on that license, please go to our website [https://www.infragistics.com/legal/license](https://www.infragistics.com/legal/license).
