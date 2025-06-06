
```
function myCreate(proto, propertiesObject = undefined) {
  if (typeof proto !== 'object' && typeof proto !== 'function' || proto === null) {
    throw new TypeError('Object prototype may only be an Object or null');
  }

  const obj = {};
  Object.setPrototypeOf(obj, proto);

  if (propertiesObject !== undefined) {
    Object.defineProperties(obj, propertiesObject);
  }

  return obj;
}
```

```
const proto = { greet() { console.log('hi') } };

const obj = myCreate(proto, {
  name: {
    value: 'heihei',
    writable: true,
    configurable: true,
    enumerable: true
  }
});

console.log(obj.name); // heihei
obj.greet();           // hi
```