# Check if body response has all the keys

> Notes:
>
> - Can be used in any method
> - On arrays, the mapping only needs the first position to compare with all others positions of the body response

## Code

```js
const check_keys = function (object = {}, mapping = {}, ignore = []) {
  const keys = Object.keys(mapping);

  keys.forEach((key) => {
    if (!ignore.includes(key)) {
      if (typeof object[key] == "object" && object[key] != null) {
        if (Array.isArray(object[key])) {
          object[key].forEach((element) => {
            if (typeof element == "object" && element != null) {
              check_keys(element, mapping[key][0]);
            }
          });
        } else {
          check_keys(object[key], mapping[key]);
        }
      } else {
        pm.expect(object).to.have.property(key);
      }
    }
  });
};
```

## Usage example:

```js
pm.test(
  "[*API NAME* API] Check if body response has all properties",
  function () {
    if (pm.response.code != 200) {
      pm.response.to.have.status(200);
    } else {
      const responseJson = pm.response.json();
      const mapping = {
        id: null,
        type: null,
        name: null,
        description: null,
        category: null,
        siteType: null,
        buildHeight: null,
        minAntennaHeight: null,
        maxAntennaHeight: null,
        siteRelationship: [
          {
            "@type": null,
            id: null,
            name: null,
            role: null,
            href: null,
          },
        ],
        place: [
          {
            "@type": null,
            streetAddress1: null,
            streetAddress2: null,
            streetNr: null,
            fieldRegister: null,
            postcode: null,
            city: null,
            county: null,
            country: null,
            geographicLocationRefOrValue: [
              {
                "@type": null,
                latitude: null,
                longitude: null,
              },
            ],
          },
        ],
        href: null,
        "@type": null,
      };

      check_keys(responseJson, mapping);
    }
  }
);
```

#postman