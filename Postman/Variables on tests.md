# Variables on tests

## Function as a variable

```js
const equals = (a, b) => a.length === b.length && a.every((v, i) => v === b[i]);
```

## Get a variable name from the collection

```js
pm.collectionVariables.get("VARIABLE_NAME");
```

## Define a variable value in the collection

```js
pm.collectionVariables.set("VARIABLE_NAME", responseJson["VARIABLE_NAME"]);
```

## Get URL request

```js
pm.request.url.toString();
```

#postman