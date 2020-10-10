---
authors:
- admin
categories: []
date: "2020-10-10T00:00:00Z"
draft: false
featured: false
image:
  caption: ""
  focal_point: ""
lastMod: "2020-10-10T00:00:00Z"
projects: []
subtitle: Creating an API endpoint for your model
summary: Connecting models to an API
tags: []
title: Machine learning as a service
---

## Creating API endpoints in Python with Flask

In this post, we'll create a minimal API endpoint that allows users to make request to calculate the area of a rectangle. The following code sets up an API endpoint locally. We'll import `Flask`, a lightweight web application framework and `CORS` (cross-origin resource sharing) which allows for various HTTP requests. 

We have two endpoints, one basic "hello world" and the other calculate the area (i.e., width x height).

This is saved in `App.py`. Then command to run this file is `python3 App.py`. The last line ensures the API is running locally on `localhost:5000`. 

```
from flask import Flask, request
from flask_cors import CORS, cross_origin
import joblib
import numpy as np 

app = Flask(__name__)
CORS(app)

@app.route('/')
def helloworld():
    return 'Helloworld'

# Example request: http://localhost:5000/area?w=50&h=3
@app.route('/area', methods=['GET'])
@cross_origin()
def area():
    w = float(request.values['w'])
    h = float(request.values['h'])
    return str(w * h)

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000, debug=True)
```

You can just run `localhost:5000` and get `Helloworld` or make a request to get the **area**, `http://localhost:5000/area?w=20&h=33`.


## Training a Logistic Regression Classification model

After setting up some demo API endpoints, it's time to create a basic Machine Learning Model. We'll create a Logistic Regression model to classify flowers from the **Iris** dataset. This will be created in a `jupyter notebook`. 




## Testing the API endpoint on Postman



