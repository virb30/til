# Set cookies on Supertest request

## Problem

How to get the browser cookies setted by an application and send it on subsequent requests while testing on jest with supertest?

## Scenario

An authentication backend that stores token in cookies on successful authentication

## Apply to

Supertest on jest.

## Solution

According to supertest documentation we can set cookies on request by using the `.set('Cookie')` method of request.
To do so, we need, first, grab the cookies sent by the login by using the `set-cookies` property of response header, as shown below:

```typescript
import request from "supertest";

// login route
const credentials = {
  /* ... */
};
const auth = await request(app).post("/login").send(credentials);

const cookies = auth.header["set-cookie"];

// subsequent routes (after login)
const response = await request(app)
  .get("/my_route")
  .set("Cookie", cookies)
  .send();
```

`cookies` variable will be a string array containing cookies data defined by auth response (the same info that are sent to browser)
