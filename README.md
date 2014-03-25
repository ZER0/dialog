# dialog

A small module to open a window dialog with [features](https://developer.mozilla.org/en-US/docs/Web/API/Window.open#Position_and_size_features) in Add-on SDK.
It's also possible attach [content scripts](https://developer.mozilla.org/en-US/Add-ons/SDK/Guides/Content_Scripts) to dialog with the document loaded.

## usage

```js
const dialog = require('./dialog');

dialog.open({
  // both `foo.html` and `index.js` are supposed to
  // be in add-on's `data` folder
  url: './foo.html',
  contentScriptFile: './index.js',
  features: {
    width: 320,
    height: 200
  }
}).then(worker => {
  // do something with worker, e.g.
  worker.port.on('title', console.log);
});
```

## access to the window
`open` function returns a promise resolved once the document is ready, passing
the `worker` attached, if any `contentScript*` was declared, or `null`.

It's possible modify the `window` object from the document's script itself, like
any regular web page, or if a `worker` is returned, is possible access to
[worker.tab](https://developer.mozilla.org/en-US/Add-ons/SDK/High-Level_APIs/tabs)
and [worker.tab.window](https://developer.mozilla.org/en-US/Add-ons/SDK/High-Level_APIs/windows).

For example, this code will close the `window` opened, after two seconds.

```js
const dialog = require('./dialog');
const { setTimeout } = require('sdk/timers');

dialog.open({
  url: './foo.html',
  contentScriptFile: './index.js',
  features: {
    width: 320,
    height: 200
  }
}).then(worker => {
  setTimeout(() => worker.tab.close(), 2000);
});
```
