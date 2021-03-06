---
version: v0.33.0
category: API
title: Dialog
source_url: 'https://github.com/electron/electron/blob/master/docs/api/dialog.md'
---

# dialog

The `dialog` module provides APIs to show native system dialogs, such as opening
files or alerting, so web applications can deliver the same user experience as
native applications.

An example of showing a dialog to select multiple files and directories:

```javascript
var win = ...;  // BrowserWindow in which to show the dialog
var dialog = require('dialog');
console.log(dialog.showOpenDialog({ properties: [ 'openFile', 'openDirectory', 'multiSelections' ]}));
```

**Note for OS X**: If you want to present dialogs as sheets, the only thing you
have to do is provide a `BrowserWindow` reference in the `browserWindow`
parameter.

## Methods

The `dialog` module has the following methods:

### `dialog.showOpenDialog([browserWindow][, options][, callback])`

* `browserWindow` BrowserWindow (optional)
* `options` Object (optional)
  * `title` String
  * `defaultPath` String
  * `filters` Array
  * `properties` Array - Contains which features the dialog should use, can
    contain `openFile`, `openDirectory`, `multiSelections` and
    `createDirectory`
* `callback` Function (optional)

On success this method returns an array of file paths chosen by the user,
otherwise it returns `undefined`.

The `filters` specifies an array of file types that can be displayed or
selected when you want to limit the user to a specific type. For example:

```javascript
{
  filters: [
    { name: 'Images', extensions: ['jpg', 'png', 'gif'] },
    { name: 'Movies', extensions: ['mkv', 'avi', 'mp4'] },
    { name: 'Custom File Type', extensions: ['as'] },
    { name: 'All Files', extensions: ['*'] }
  ]
}
```

The `extensions` array should contain extensions without wildcards or dots (e.g.
`'png'` is good but `'.png'` and `'*.png'` are bad). To show all files, use the
`'*'` wildcard (no other wildcard is supported).

If a `callback` is passed, the API call will be asynchronous and the result
will be passed via `callback(filenames)`

**Note:** On Windows and Linux an open dialog can not be both a file selector
and a directory selector, so if you set `properties` to
`['openFile', 'openDirectory']` on these platforms, a directory selector will be
shown.

### `dialog.showSaveDialog([browserWindow][, options][, callback])`

* `browserWindow` BrowserWindow (optional)
* `options` Object (optional)
  * `title` String
  * `defaultPath` String
  * `filters` Array
* `callback` Function (optional)

On success this method returns the path of the file chosen by the user,
otherwise it returns `undefined`.

The `filters` specifies an array of file types that can be displayed, see
`dialog.showOpenDialog` for an example.

If a `callback` is passed, the API call will be asynchronous and the result
will be passed via `callback(filename)`

### `dialog.showMessageBox([browserWindow][, options][, callback])`

* `browserWindow` BrowserWindow (optional)
* `options` Object (optional)
  * `type` String - Can be `"none"`, `"info"`, `"error"`, `"question"` or
  `"warning"`. On Windows, "question" displays the same icon as "info", unless
  you set an icon using the "icon" option.
  * `buttons` Array - Array of texts for buttons.
  * `title` String - Title of the message box, some platforms will not show it.
  * `message` String - Content of the message box.
  * `detail` String - Extra information of the message.
  * `icon` [NativeImage](http://electron.atom.io/docs/v0.33.0/api/native-image)
  * `cancelId` Integer - The value will be returned when user cancels the dialog
    instead of clicking the buttons of the dialog. By default it is the index
    of the buttons that have "cancel" or "no" as label, or 0 if there is no such
    buttons. On OS X and Windows the index of "Cancel" button will always be
    used as `cancelId`, not matter whether it is already specified.
  * `noLink` Boolean - On Windows Electron will try to figure out which one of
    the `buttons` are common buttons (like "Cancel" or "Yes"), and show the
    others as command links in the dialog. This can make the dialog appear in
    the style of modern Windows apps. If you don't like this behavior, you can
    set `noLink` to `true`.
* `callback` Function

Shows a message box, it will block the process until the message box is closed.
It returns the index of the clicked button.

If a `callback` is passed, the API call will be asynchronous and the result
will be passed via `callback(response)`.

### `dialog.showErrorBox(title, content)`

Displays a modal dialog that shows an error message.

This API can be called safely before the `ready` event the `app` module emits,
it is usually used to report errors in early stage of startup.
