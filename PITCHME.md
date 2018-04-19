---
### What is Flask?
- a free/open source "micro-framework" for python
- handles HTTP requests with python functions
- exposes server endpoints (i.e. urls) which trigger
  the function calls
- customizable, and supports many extensions



---
#### A Simple Flask app
```python
from flask import Flask
app = Flask(__name__)

@app.route('/hello')
def hello():
    return "Hello World!"
```

`curl -X GET http://your_server/hello`

```html
Hello World!
```



---
#### The request object
```python
from flask import request
@app.route('/login', methods=['POST'])
def login():
    phone = request.form.get('phone')
    pin = request.form.get('pin')
    token = validate(phone, pin)
    if token is None
        return {'status': 'failed'}, 400
    return {'status': 'authenticated', 'token': token}
```
- **`request.args`** contains the url paramters
- **`request.form`** contains form info (POST request)
- **`request.data`** contains the raw request body



---
#### Authentication
- Using Google Firebase as an external auth service
- Users authenticate directly with Firebase, and receive a JSON Web Token (JWT)
- We verify the token's validity
- A token is just a hex string with 3 sections: header, claims, and signature



---
#### More about JWTs
- Tokens are **not encrypted!**
- used to verify the integrity of claims, not to hide the contents of claims
- claims can include username, phone number, etc
- claims do not include sensitive information like passwords, credit card info, bank details, etc
- The signature section is used to verify that the token was created by a trusted source
- more info here: https://jwt.io/introduction/



---
#### Example Token
<div style="word-wrap: break-word; background: #1E1E1E; margin: auto; font-size: 70%; width: 90%"><span style="color: blue">eyJhbGciOiJIUzI1NiJ9</span>.<span style="color: yellow">eyJzdWIiOiJ0ZXN0IHVzZXIiLCJpYXQiOjE1MjM0NTY3ODksImV4cCI6MTUyMzQ1OTk5OX0</span>.<span style="color: red">ZDNW7R7JLDGemU23nekutCt0Zpl4QtJ1bamkjvxRoNQ</span></div>

<span style="color: blue">header</span>:
```json
{
  "alg": "HS256"
}
```
<span style="color: yellow">claims</span>:
```json
{
  "sub": "test user",
  "iat": 1523456789,
  "exp": 1523459999
}
```



---
#### Auth demo
- run local firebase quickstart app and visit: http://localhost:5001/phone-invisible.html
- Save token locally, and hit the `/users/me` endpoint with it:
```bash
token='xxx.yyy.zzz'  #accessToken from sign-in results
curl -H "Authorization: Bearer $token" localhost:5000/users/me
```