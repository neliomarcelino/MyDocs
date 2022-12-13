# Chai API

> [Chai API Documentation](https://www.chaijs.com/api/)

# Chai API most commonly usages

## Check if has property

```js
pm.expect(body).to.have.property(key);
```

## Expect the value to be equal

```js
pm.expect(pm.response.code).to.be.eq(200);
```

## Expect the value to be on of ...

```js
pm.expect(pm.response.code).to.be.oneOf([200, 206]);
```

## Expect to be below ...

```js
pm.expect(array.length).to.be.below(MAXSIZE);
```

#postman