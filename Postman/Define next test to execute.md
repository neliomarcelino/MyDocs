# Define next test to execute

## Example

> The request has a pagination, needs to update the field `next=` to the next page

> Define a variable with the value of the `next=` field and set it on the path like `https://www.api.xxx.com/next={{nextPage}}`

```js
if (pm.response.code != 200) {
    pm.response.to.have.status(200);
} else {
    const responseJson = pm.response.json();

    if(responseJson.hasOwnProperty("nextPage"){
        pm.collectionVariables.set(
            "nextPage",
            responseJson["nextPage"]
        );
    }
}
```

> Define the next request with the command below with the exact name of the postman request

```js
postman.setNextRequest("show next items from list");
```

> If on request has a flag like `finished: true` or by knowing the response code `206 Partial Content` it's not yet finished, you can do a `if` statement like:

```js
if (pm.response.code == 200 && responseJson["finished"] == true) {
  postman.setNextRequest("show next items from list");
}
```

#postman