# Option

The `option` service provides a simple way for storing and fetching values
to and from the database.

## Set a value

Use `set($key, $value, $autoload=null)` to set an option.

- `$key`: string to uniquely identify the option. In order not to have name
  collisions with other extensions, you should prefix the option with your
  extension's name, e.g. `hello:version`
- `$value`: value you want to store. This can be of the type `int`, `str` and
  `array`.
- `$autoload`: boolean to determine if the option should automatically be loaded
  from the database request on every request. Set to `false` for better
  performance. When no value or `null` is passed, the current autoload behavior
  for this option will be kept.


```
$this('option')->set('hello:message', 'Hello Universe!');
```


## Get a value

Use `get($name, $default = null)` to fetch the value of an option. If the
option has not been set, the value passed to `$default` will be returned.


```
$message = $this('option')->get('hello:message', 'Hello World!');

```
