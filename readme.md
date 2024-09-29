# identify-github-event

Map Github webhook events to their names

## Features

- Using the Github webhooks / services API with AWS Lambda can be annoying because by default, AWS Lambda strips out request headers and getting them back requires a bunch of fiddly configuration (and Github webhooks pass the event type in a header).
- `identify-github-event` identifies Github webhook events based on their payload and returns the event name
- since `v1.1.0`: can also return the `user`, `repo` and `branch` information from a Github event (except for MembershipEvent which does not contain that information).
- Tested on the example events [listed on the Github Events API page](https://web.archive.org/web/20150320055220/https://developer.github.com/v3/activity/events/types/).
- note that Github events have unique signatures as long as you look at the full set of key names; if this ever becomes not true then some events may be confused with each other since some events share key names (but not key name signatures; e.g. difference(event.keynames, union(other.keynames)) is an empty set for some events like PublicEvent and).
- the type key, which is not present on webhooks/services but is present on Events API calls is used if it is available (after validating it against the list of known events)
- the NPM version filesize is quite small - it [only includes the signatures and the wrapper code](https://github.com/mixu/identify-github-event/blob/master/.npmignore)

## Installing

```
npm install --save identify-github-event
```

## Usage

```
var identifyGithubEvent = require('identify-github-event');

console.log(identifyGithubEvent(event));
// returns the CamelCased name, e.g. PushEvent (or undefined if the event signature is unknown)

console.log(identifyGithubEvent.target(event));
// returns a hash { user: 'name', repo: 'somename', branch: 'somebranch' }
```

## Rebuilding

The `mapping.json` and `event-names.json` files are automatically generated via `npm prepublish` script. To run it manually, run `npm run-script prepublish`.
