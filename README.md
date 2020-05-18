# I18n

The purpose is to show the faulty behaviour reported in https://github.com/angular/angular-cli/issues/17737
To recreate run `yarn && yarn build`, this would build two locales specified in the `angular.json` file: en & fr.
```
$ ng build --localize --baseHref='/assets/' --deployUrl='http://localhost:4200/assets/'
```
The issue is that the locale gets appended to the `baseHref` but not to `deployUrl` which results in the following `fr/index.html`
```
$ cat dist/i18n/fr/index.html 
<!doctype html>
<html lang="fr">
<head>
  <meta charset="utf-8">
  <title>I18n</title>
  <base href="/assets/fr/">  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="icon" type="image/x-icon" href="favicon.ico">
</head>
<body>
  <app-root></app-root>
<script src="http://localhost:4200/assets/runtime-es2015.js" type="module"></script><script src="http://localhost:4200/assets/runtime-es5.js" nomodule defer></script><script src="http://localhost:4200/assets/polyfills-es5.js" nomodule defer></script><script src="http://localhost:4200/assets/polyfills-es2015.js" type="module"></script><script src="http://localhost:4200/assets/styles-es2015.js" type="module"></script><script src="http://localhost:4200/assets/styles-es5.js" nomodule defer></script><script src="http://localhost:4200/assets/vendor-es2015.js" type="module"></script><script src="http://localhost:4200/assets/vendor-es5.js" nomodule defer></script><script src="http://localhost:4200/assets/main-es2015.js" type="module"></script><script src="http://localhost:4200/assets/main-es5.js" nomodule defer></script></body>
</html>
```
As you can see above, the `<base href=` gets the locale appended, but all the other assets (like `runtime.js`) are not prepeneded with the locale, which would cause them to return 404.
