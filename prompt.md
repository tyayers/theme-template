Create a modern AI tools and models portal web app in vanilla HTML/CSS/JS supporting both light and dark modes. The style should be slick and modern.

Here are the details about how the app should be structured and which features should be offered:

## Overall web app structure
* The app's app.js should have a configuration properties "storefrontId", "demoMode" (default true) and "apiHost" (default empty) that are used for fetching configuration and product data.
* Upon loading, it should load the storefront config and product data. If demoMode is "true", then the mock data in "mock-storefront.jsom" and "mock-products.json" should be used. If demoMode is "false", then the data should be loaded from the "/api/storefronts/{storefrontId}" and "/api/storefronts/{storefrontId}/products" endpoints, using the root "apiHost" host.
* The storefront config properties authApiKey, authDomain, authType, and name should also be able to be overridden with environment variables, so AUTH_API_KEY, AUTH_DOMAIN, etc..
* If the storefront config has the "authType" property set to "demo", then Sign-In should just take an email address (default user@example.com) with no password. If "authType" is set to "identity-platform", then the app should use the Firebase auth client libraries to initialize the app with the "authApiKey" and "authDomain" properties, and offer Google, Email/Password and Enterprise SAML sign-on options. 
* The landing page should be visible to all, and have a hero section, benefits & testimonials. The hero section should also include a prominent sign-on option.

## Signing in
* If "demoMode" is set to "false", then after the sign-in a POST call to the endpoint "{apiHost}/api/storefronts/{storefrontId}/users/{email}/login" should be sent.
* After sign-on, the user should be forwarded to the catalog view.

## Catalog view
* The web app should have a frame with header with the app name and logo on the left side, and the main Categories from the product data listed as links on the right side, with the user's account picture and a drop-down menu to sign-out and view account information. Each view after signing in should be loaded as an HTML snippet from the ./views directory, and have a hash path. The user's apps should be loaded and stored in localstorage. If demoMode is "true", then the apps should be loaded from the file "mock-apps.json" file, or if demoMode is "false" then from the "/api/users/{email}/apps" endpoint using the "apiHost". If the user refreshes the user's signed-in state should be checked, either in demoMode or with a Firebase Auth event, and either allowed to the URL path, or redirected to the landing page.
* Each Category Link should load a catalog view into the main application panel below the header. The views should be stored in a ./views directory, and loaded as HTML snippets that are inserted into the main panel. Each view should have a hash route to make it easy to route to and share.
* The Catalog view that is clicked for the Categories at the top should display all of the products from the clicked Category, with the possibility to search & filter by keyword or Tag from the products, and to show the products either as cards or in a list. The list should be sortable on the column properties.

## Product detail view
* If the user clicks on a product, then a detail view is opened. If the product style is REST, and there is a spec, the contents are decoded and displayed in a Scalar OpenAPI view window, with colors adjusted to either light or dark mode. If the product style is MCP or any other style, then just documentation, endpoints, code snippets and other relevant documentation information is shown.
* The user can add the product to an existing app, or create a new app for the product. The user should have an overview of apps that this product has been added to, and be able to click on the app to go to the app detail view.

## User apps view
* If the user clicks on their profile photo in the header, a drop-down menu should let them manage their Account and Apps. After clicking on Apps it should show a list of their apps, as well as the possibility to create a new app. An app has a collection of credentials with a clientId and clientSecret, and a list of products that can be turned on or off. This mirrors how each product is shown in the product detail view - with the apps that the product belongs to, and the possibility to add to an app, or create a new app subscribing to the product. The products should also be clickable into the product detail view. If an app is created, updated or deleted, then either the app data are updated in memory if "demoMode" is "true", or sent to the "/api/storefronts/{storefrontId}/users/{email}/apps" endpoint if "demoMode" is "false".

## User account view
* If the user clicks on their profile photo and then on the Account link, this should open up the account view, which shows basic info about the user's account, and also a "Danger Zone" where the user can delete their account.

## User analytics view
* If the user clicks on their profile photo and then on the Analytics link, this should open up the analytics view that displays several graphs and tables of the user's consumption by product. If demoMode is "true", then the data should be loaded from the file mock-analytics.json, or if "false" then loaded from the "/api/users/{email}/analytics" endpoint at apiHost. The data should only be displayed from the "product" key in the response, and offer an graph and table, with the user being able to select to show the data for all products, or a selection of products.
