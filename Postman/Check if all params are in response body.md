# Check if all params are in response body

## Code

```js
const check_params = function (url = "", data = {}) {
  if (url == "") return;

  const params = String(url).split("?");
  params.shift();
  const query_params = params[0].split("&");

  // get all fields
  for (i = 0; i < query_params.length; i++) {
    if (query_params[i].includes("fields")) {
      const fields = query_params[i].split("=")[1].split(",");

      if (fields.length > 0) {
        fields.forEach((field) => {
          console.log(field);
          if (Array.isArray(data)) {
            data.forEach((element) => {
              pm.expect(element).to.have.property(field);
            });
          } else {
            pm.expect(element).to.have.property(field);
          }
        });
      }
      break;
    }
  }
};
```

#postman