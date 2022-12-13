# Check if the response body contains the same value as the request body

> Notes:
>
> - Usually used on `PATCH` or `POST` to check if the API updated to the correct values

## Code

```js
const compare_json = function (obj1 = {}, obj2 = {}, ignore = []) {
  const obj1_keys = Object.keys(obj1);
  const obj2_keys = Object.keys(obj2);

  const less_keys = obj1_keys.length < obj2_keys.length ? obj1_keys : obj2_keys;
  const lower = obj1_keys.length < obj2_keys.length ? obj1 : obj2;
  const higher = obj1_keys.length < obj2_keys.length ? obj2 : obj1;

  less_keys.forEach((key) => {
    if (!ignore.includes(key)) {
      if (typeof lower[key] === "object" && lower[key] != null) {
        if (Array.isArray(lower[key])) {
          lower[key].forEach((element, index) => {
            compare_json(element, higher[key][index]);
          });
        } else {
          compare_json(element[key], higher[key]);
        }
      } else {
        if (typeof lower[key] === "string") {
          lower[key] = lower[key].toLowerCase();
          higher[key] = higher[key].toLowerCase();
        }
        if (!isNaN(new Date(lower[key])) && typeof lower[key] != "number") {
          console.log(lower[key]);
          lower_date = new Date(lower[key]);
          lower[key] = new Date(
            lower_date.getFullYear(),
            lower_date.getMonth(),
            lower_date.getDate()
          ).toJSON();

          higher_date = new Date(higher[key]);
          higher[key] = new Date(
            higher_date.getFullYear(),
            higher_date.getMonth(),
            higher_date.getDate()
          ).toJSON();
        }
        pm.expect(lower[key]).to.be.eq(higher[key]);
      }
    }
  });
};
```

## Usage example:

```js
pm.test(
  "[*API NAME* API] Check if body response has same values as the request",
  function () {
    if (pm.response.code != 200) {
      pm.response.to.have.status(200);
    } else {
      const responseJson = pm.response.json();
      const requestJson = JSON.parse(pm.request.toJSON()["body"]["raw"]);

      compare_json(responseJson, requestJson, ["name"]);
    }
  }
);
```

#postman