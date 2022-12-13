# Check query paramters

## Code

```js
const check_query = function (url = "", data = {}) {
  if (url == "") return;

  const params = String(url).split("?");
  params.shift();
  const query = params[0].split("&");

  // remove 'fields'
  for (i = 0; i < query.length; i++) {
    if (query[i].includes("fields")) {
      query.splice(i, 1);
      break;
    }
  }

  query.forEach((q) => {
    const split = String(q).split("=");
    const key = split[0];
    const value = split[1];

    if (Array.isArray(data)) {
      data.forEach((element) => {
        pm.expect(element[key]).to.be.eq(value.toString());
      });
    } else {
      pm.expect(data[key]).to.be.eq(value.toString());
    }
  });
};
```

#postman