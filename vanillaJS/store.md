## store.js

## Basic Usage

All you need to know to get started:

### API

store.js exposes a simple API for cross-browser local storage:

```javascript
// Store current user
store.set('user', { name:'Marcus' })

// Get current user
store.get('user')

// Remove current user
store.remove('user')

// Clear all keys
store.clearAll()

// Loop over all stored values
store.each(function(value, key) {
	console.log(key, '==', value)
})
```

### Installation

Using npm:

```bash
npm i store
```

```javascript
// Example store.js usage with npm
var store = require('store')
store.set('user', { name:'Marcus' })
store.get('user').name == 'Marcus'
```

Using script tag (first download one of the [builds](https://github.com/marcuswestin/store.js/blob/master/dist)):

```javascript
<!-- Example store.js usage with script tag -->
<script src="path/to/my/store.legacy.min.js"></script>
<script>
store.set('user', { name:'Marcus' })
store.get('user').name == 'Marcus'
</script>
```

