# ember-local-storage

[![Build Status](https://travis-ci.org/funkensturm/ember-local-storage.svg)](https://travis-ci.org/funkensturm/ember-local-storage)

An addon for ember-cli that provides sessionStorage and localStorage object and array in your ember-cli app.

The idee was taken from Tom Dale's gist [Ember Array that writes every change to localStorage](https://gist.github.com/tomdale/11360257) and extended to objects.

It supports:
* sessionStorage
* localStorage
* Object
* Array


## Installation

* `npm install --save-dev ember-local-storage`

## Changelog

### 0.0.2
* [ENHANCEMENT] sessionStorage added
* [BREAKING] localStorage array on object location changed
* [BREAKING] proxyObjects and proxyArrays have to be created

## Usage

### Object

If you need to persist in sessionStorage change the import:

`import StorageObject from 'ember-local-storage/session/object';`

```javascript
// app/models/settings.js
import StorageObject from 'ember-local-storage/local/object';

export default StorageObject.create({
  storageKey: 'your-app-settings',
  initialContent: {
    welcomeMessageSeen: false
  }
});
```

```javascript
// app/controllers/application.js
import Ember from 'ember';
import Settings from 'your-app/models/settings';

export default Ember.Controller.extend({
  settings: Settings.create(),

  actions: {
	hideWelcomeMessage: function() {
		this.set('settings.welcomeMessageSeen', true);
	}
  }
});
```

```handlebars
{{! app/templates/application.hbs}}
{{#unless settings.welcomeMessageSeen}}
  Welcome message.
  <button {{action "hideWelcomeMessage"}}>X</button>
{{/unless}}
```

### Array

If you need to persist in sessionStorage change the import:

`import StorageObject from 'ember-local-storage/session/array';`

```javascript
// app/models/anonymous-likes.js
import StorageArray from 'ember-local-storage/local/array';

export default StorageArray.create({
  storageKey: 'your-app-anonymous-likes'
});
```

```javascript
// app/controllers/item.js
import Ember from 'ember';
import AnonymousLikes from 'your-app/models/anonymous-likes';

export default Ember.ObjectController.extend({
  anonymousLikes: AnonymousLikes.create(),

  isLiked: computed('id', function() {
	return this.get('anonymousLikes').contains(this.get('id'));
  }),

  actions: {
	like: function() {
		this.get('anonymousLikes').addObject(this.get('id'));
	}
  }
});
```

```handlebars
{{! app/templates/item.hbs}}
{{#unless isLiked}}
  <button {{action "like"}}>Like it</button>
{{else}}
  You like it!
{{/unless}}
```

## Running

* `ember server`
* Visit your app at http://localhost:4200.

## Running Tests

* `ember test`
* `ember test --server`

## Building

* `ember build`

For more information on using ember-cli, visit [http://www.ember-cli.com/](http://www.ember-cli.com/).
