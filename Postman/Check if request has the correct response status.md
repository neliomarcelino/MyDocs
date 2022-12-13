# Check if request has the correct response status

```js
pm.test("[*API NAME* API] Status code is 200, 201, 204 or 206", function () {
  pm.expect(pm.response.code).to.be.oneOf([200, 201, 204, 206]);
});
```

#postman