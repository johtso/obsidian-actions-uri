# Documentation

Obsidian natively supports a custom URI protocol `obsidian://` which can be used to trigger various actions within the app.  This is commonly used on macOS and mobile apps for automation and cross-app workflows.

**This plugin adds new `x-callback-url` endpoints** to Obsidian so that external sources can better interact with an Obsidian instance by making `GET` requests to a `obsidian://actions-uri/*` URL.  All new routes support `x-success` and `x-error` parameters as a way of communicating back to the sender.  See below for detailed documentation.

It is a clean, somewhat super-charged addition to Obsidian's [own URI scheme](https://help.obsidian.md/Advanced+topics/Using+obsidian+URI#Using+Obsidian+URIs).


## Routes added by Actions URI
- [`/daily-note`](routes/daily-note.md): Reading, writing, updating daily notes.
- [`/info`](routes/info.md): Plugin & Obsidian environment info.
- [`/note`](routes/note.md): Reading, writing, updating any notes.
- [`/open`](routes/open.md): Opening notes, daily notes and searches in Obsidian.
- [`/search`](routs/-search.md): Running searches in Obsidian.
- [`/`](route-s/oot.md): The root note. Not much is happening here.


## Anatomy of an Actions URI… URL
An Action URI-provided URL doesn't look much different from a standard Obsidian URI.  It adds a new namespace that tells Obsidian which plugin is taking care of the incoming call:

> obsidian://`actions-uri`/daily-note/get-current?parameter=value

… and specifies routes in that namespace:

> obsidian://actions-uri/`daily-note/get-current`?parameter=value

Both data and configuration is passed as URL search parameters:

> obsidian://actions-uri/daily-note/get-current?`parameter=value`

**Please note:** all parameter data must be properly encoded (see [Wikipedia](https://en.wikipedia.org/wiki/Percent-encoding) for a short intro), as Actions URI will make no attempts to correct malformed input.


## Parameters required in/ accepted by all calls

| Parameter    | Value type | Optional? | Description                                                                                                                                                                |
| ------------ | ---------- |:---------:| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `vault`      | string     |           | The name of the target vault.                                                                                                                                              |
| `x-success`  | string     |  mostly   | Base URL for on-success callbacks, see [Getting data back from Actions URI](callbacks.md).                                                                                 |
| `x-error`    | string     |  mostly   | Base URL for on-error callbacks, see [Getting data back from Actions URI](callbacks.md).                                                                                   |
| `call-id`    | string     |    ✅     | Unique ID for pairing request & callback, see [Getting data back from Actions URI](callbacks.md).                                                                          |
| `debug-mode` | boolean    |    ✅     | When enabled, Actions URI will include not just the `call-id` in the return calls but all parameters of the original request, prefixed with `input-`. Defaults to `false`. |

### Notes about parameters

<dl>
  <dt>"mostly"</dt>
  <dd>optional unless specified otherwise in the detailed route description</dd>
  <dt>"boolean"</dt>
  <dd>Actions URI uses what I call "benevolent booleans": the absence of the parameter, an empty string or the string "false" are considered <code>false</code>, everything else is <code>true</code></dd>
</dl>


## Getting data back from Actions URI
See [Getting data back from Actions URI](callbacks.md).