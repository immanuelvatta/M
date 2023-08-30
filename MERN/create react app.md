#
# ReadMe to make a Fullstack React Application

### {Authors: **Immanuel Vattakunnel, Braxton Walters, Robert Owens**}
```bash
mkdir ProjectName
```
### Create BACKEND
```bash
mkdir server
cd server
npm init -y
npm install express mongoose dotenv cors
mkdir config controllers models routes
touch server.js .gitignore .env
```

- In your .env file write this

```
PORT=8000
DB=my_db
# mongo atlas connection
ATLAS_USERNAME=yourUserName
ATLAS_PASSWORD=yourPassword
```
- In your .gitignore file write this

```
/node_modules
.env
```

- Then go to config folder

```bash
touch mongoose.config.js
```
- In mongoose.config.js file write this

```js
const mongoose = require('mongoose');
const dbName = process.env.DB;
const username = process.env.ATLAS_USERNAME;
const pw = process.env.ATLAS_PASSWORD;
const uri = `mongodb+srv://${username}:${pw}@cluster0.11uag4t.mongodb.net/${dbName}?retryWrites=true&w=majority`;
mongoose.connect(uri, {
    useNewUrlParser: true,
    useUnifiedTopology: true,
})
    .then(() => console.log("Established a connection to the database"))
    .catch(err => console.log("Something went wrong when connecting to the database", err));

```

- go to server.js

```js
const express = require('express');
const cors = require('cors');
const app = express();
require('dotenv').config();
const port = process.env.PORT;
require('./config/mongoose.config');
app.use(cors());
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

// !!!create your link to routes like example below!!!
// Example below
// require('./routes/product.routes')(app);

app.listen(port, () => console.log(`Listening on port: ${port}`) );

```
-- go to model folder and create a file
```bash
touch nameHere.model.js
<!-- example: product.model.js -->
```
inside the model file write this

```js
// example model file
const mongoose = require('mongoose');
 
// Create a Schema for Product
const ProductSchema = new mongoose.Schema(
    {
    title: {
        type: String,
        required: [true, "Title is required!"],
        minlength: [6, "Title must be at least 6 characters long...."]
    },
    price: {
        type: Number,
        required: [true, "Price is required...."],
        min: [500, "The price must be above 500!"],
        max: [10000000, "The price must be below 10,000,000!!"]

    },
    description: {
        type: String,
        required: [true, "Description is required...."],
        minlength: [6, "Description must be at least 6 characters long...."]
    }
   }, 
   { timestamps: true })
   const Product = mongoose.model('product', ProductSchema);
module.exports = Product;

```

- in your controllers folder create a file

```bash
touch nameHere.controller.js
<!-- example: product.controller.js -->

```
inside the controller file write this

```js
// example code for controller
// importing the model file
const Product = require('../models/product.model');

// get all async and try catch
module.exports.findAllProducts = async(req,res) => {
    try{
        const productList = await Product.find();
        res.json(productList)
        // console.log(productList)
    }catch(err){
        res.json(err);
    }
};

// get all "then catch"
module.exports.findAllProducts = (req, res) => {
    Product.find()
        .then((allProducts) => {
            res.json(allProducts)
        })
        .catch((err) => {
            res.json(err)
        })
}

// get one by id "async and try catch"
module.exports.findOneProduct = async (req, res) => {
    try {
        const oneProduct = await Product.findOne({ _id: req.params.id });
        res.json(oneProduct);
    } catch (err) {
        res.json(err);
    }
};

// get one by id "then catch"
module.exports.findOneProduct = (req, res) => {
    Product.findOne({_id: req.params.id})
        .then(oneProduct => {
            res.json(oneProduct)
        })
        .catch((err) => {
            res.json(err)
        })
}

// create "async and try catch"
module.exports.createProduct = async (req, res) => {
    try {
        const newProduct = await Product.create(req.body);
        res.json(newProduct);
    } catch (err) {
        res.status(400).json(err)
    }
};

// create "then catch"
module.exports.createProduct = (req, res) => {
    Product.create(req.body)
        .then(newProduct => {
            res.json(newProduct)
        })
        .catch((err) => {
            res.status(400).json(err)
        })
}

// update by id "async and try catch"
module.exports.updateProduct = async(req,res) => {
    try{
        const updatedProduct = await Product.findOneAndUpdate(
            { _id: req.params.id },
            req.body,
            { new: true, runValidators: true }
        );
        res.json(updatedProduct)
    }catch (err){
        res.status(400).json(err);
    }
};

// update by id "then catch"
module.exports.updateProduct =(req,res) => {
    Product.findOneAndUpdate(
        { _id: req.params.id },
        req.body,
        { new:  true, runValidators: true }
    )
        .then(updatedProduct => {
            res.json({product: updatedProduct})
        })
        .catch(err => {
            res.status(400).json(err)
        });
}

// delete by id "async and try catch"
module.exports.deleteProduct = async (req,res) => {
    try{
        const result = await Product.deleteOne({ _id: req.params.id });
        res.json(result);
    }catch (err) {
        res.json(err);
    }
}

// delete by id "then catch"
module.exports.deleteProduct = (req, res) => {
    Product.deleteOne({ _id: req.params.id })
        .then(result => {
            res.json(result)
        })
        .catch(err =>{
            res.json(err)
        });
};
```
- in your routes folder create a file

```bash
touch nameHere.routes.js
<!-- example: product.routes.js -->

```
inside the routes file write this

```js
//example routes file
// importing controller file 
const ProductController = require("../controllers/product.controller");

module.exports = app => {
    app.get("/api/product", ProductController.findAllProduct)
    app.get("/api/product/:id", ProductController.getOneProduct)
    app.post("/api/product/new", ProductController.createNewProduct)
    app.put("/api/product/edit/:id", ProductController.updateProduct)
    app.delete("/api/product/delete/:id", ProductController.deleteProduct)
}
```
- to start the server
```bash
nodemon server.js
```

### CREATE FRONTEND

- !!!MAKE SURE YOU ARE IN THE PROJECT FOLDER!!!

```bash
npx create-react-app client
cd client
npm install axios react-router-dom
```
-to start the client
```bash
npm run start
```
- go to public/index.html file (**Optional for BOOTSTRAP**)

```html
// add this line
<link
  rel="stylesheet"
  href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css"
  integrity="sha384-9ndCyUaIbzAi2FUVXJi0CjmCapSmO7SnpJef0486qhLnuZ2cdeRhO02iuK6FUUVM"
  crossorigin="anonymous"
/>
```
- modify your index.js to this 

```js
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import reportWebVitals from './reportWebVitals';
import './index.css';

import { BrowserRouter } from "react-router-dom"

const root = ReactDOM.createRoot(document.getElementById('root'));

root.render(
  <React.StrictMode> 
    {/* wrapping App in BrowserRouter */}
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </React.StrictMode>
);

reportWebVitals();
```

- from your client folder go to src and create 2 folders called components and views

```bash
mkdir views components
```

- in your views create the (parent) aka Main.jsx
```bash
cd views
touch Main.jsx
```

* in your Main.jsx file add this
    - shortcut snippet (rafce)

```jsx
import React, { useState, useEffect } from "react";
import axios from "axios";

const Main = () => {
  return (
    <div>Main</div>
  )
}

export default Main
```

- go to src/app.js
```js
import React from 'react';
import './App.css';
import {Routes, Route} from  'react-router-dom'
//import main
import Main from './views/Main';

function App() {
  return (
    <div className="App">
      <Routes>
        <Route path='/' element={<Main/>}/>
        {/* more routes to be added later as we do our crud */}
      </Routes>

    </div>
  );
}
export default App;
```

- go to our src/components and add (create functionality) 
```bash
# change FileName
touch FileNameForm.jsx
```

- in your components/FileNameForm.jsx add this

```jsx
// example code
import React, { useState } from "react";
import axios from 'axios'

// bringing in prop called 'onCreate' from Main
// this is a function
const ProductForm = ({onCreate}) => {
    // use state for each form input 
    // make sure to set default value as "" for strings and 0 for int
    const [title, setTitle] = useState("")
    const [price, setPrice] = useState(0)
    const [description, setDescription] = useState("")
    const [errors, setErrors] = useState([]); 

    // define a function to handle form submission
    const handleSubmit = (e) =>{
        e.preventDefault();
        
        // make sure your front end and backend variable names match if they do you can do like this comment below
        // example const newProduct = {title, price, description}
        const newProduct = {
            title: title, 
            price: price, 
            description: description,
        }
        // database call to create a new product
        // notice the post on the axios  
        // POST request to backend API to create a new product
        axios.post('http://localhost:8000/api/products/new',newProduct)
            .then(res =>{
                // call the 'onCreate' callback function to update the list
                onCreate();
                // this is resetting the states back to their default values after successful submission
                setTitle("");
                setPrice(0);
                setDescription("");
            })
            // does straight up nothing
            .catch(err =>{
                const errorResponse = err.response.data.errors;
                const errorArr = [];
                for (const key of Object.keys(errorResponse)) {
                    errorArr.push(errorResponse[key].message)
                }
                setErrors(errorArr);
            })
        
    }
    // this is the return lol
    return (
        <div className="container">
            {/*call the function to handle form submission */}
            <form onSubmit={handleSubmit}>
                {errors.map((err, index) => <p key={index}>{err}</p>)}
                <div className="form-group row mt-2">
                    <label htmlFor="title" className="col-sm-2 col-form-label">Title:</label>
                    <div className="col-sm-10">
                        {/* Make sure the value is set to the state var. Onchange to monitor the input. Set it to a callback function that sets the state */}
                        <input className="form-control" type="text" name="title" value={title} onChange={(e) => setTitle (e.target.value)} />
                    </div>
                </div>
                <div className="form-group row mt-2">
                    <label htmlFor="price" className="col-sm-2 col-form-label">Price:</label>
                    <div className="col-sm-10">
                        {/* same as above */}
                        <input className="form-control" type="number" name="price" value={price} onChange={(e) => setPrice (e.target.value)}/>
                    </div>
                </div>
                <div className="form-group row mt-2">
                    <label htmlFor="description" className="col-sm-2 col-form-label">Description:</label>
                    <div className="col-sm-10">
                        {/* same as above */}
                        <textarea className="form-control" name="description" id="description" cols="10" rows="3" value={description} onChange={(e) => setDescription (e.target.value)}/>
                    </div>
                </div>
                <button className="btn btn-success mt-3" type="submit">Create</button>
        </form>
        </div>
    );
};

export default ProductForm;

```

go back to Main.jsx and add this

```jsx
import React, { useState, useEffect } from "react";
import axios from "axios";
import ProductForm from '../components/ProductForm' // this is new

const Main = () => {
    // creating the state to hold the array of products
    const [productList, setProductList] = useState([]);
    
    // create a function that executes a callback ONLY if the dependencies have changed between rendering
    // must have 2 arguments
    useEffect(() => {
        getProductList();
    }, []);

    // creating a function to handle setting the state
    const getProductList = () => {
        // calling the DB to get all the products
        axios
            .get("http://localhost:8000/api/products")
            .then((res) => {
                console.log(res.data);
                // setting state with all products
                setProductList(res.data);
            })
            .catch((err) => {
                console.log(err);
            });
    };
    // callback function to update list after creating a new product
    const handleNewProductCreate = () => {
        getProductList();
    };
  return (
    <div>
    {/* render ProductForm component and pass the callback handleProductCreate */}
        <ProductForm onCreate={handleProductCreate}/>
    </div>
  )
}

export default Main
```
- go to our src/components and add list view (read functionality)
```bash
# change FileName
touch FileNameList.jsx
```
```jsx
import React from "react";
import { Link } from "react-router-dom";
import axios from "axios";

// bringing in the product list and removeFromDom. Also destructuring them
const ProductList = ({productList, removeFromDom}) =>{
    
    // function to delete a product by id
    const deleteProduct = (id)=>{
        // send the delete request to the backend API to remove the product
        axios.delete(`http://localhost:8000/api/products/delete/${id}`)
        .then(res =>{
            console.log(res)
            // 'removeFromDom' callback to update the product list in the parent component
            removeFromDom(id);
        })
        .catch(err=>{
            console.log(err)
        })
    }
    return(
        <div>
            <h1>Product list:</h1>
            <ul >
                {/* mapping each product in the productList to programmatically render all products */}
                {productList.map((product)=>
                <li className="list-unstyled" key={product._id}>
                {/* render link to product details*/}
                    <Link to={`/products/${product._id}`}>{product.title}</Link>
                    {/* render delete button and add an onClick event to delete product function*/}
                    <button className="m-1 btn btn-danger" onClick={()=>deleteProduct(product._id)}>Delete</button>
                </li>
                )}
            </ul>
        </div>
    )
}
export default ProductList
```

- update our Main.jsx to include the delete From Dom based on its id

```jsx

// THIS IS OUR FINAL MAIN.JSX (no more updating this! WOOHOOO)🎉
import React, { useState, useEffect } from "react";
import axios from "axios";
import ProductForm from "../components/ProductForm";
import ProductList from "../components/ProductList";


import React, { useState, useEffect } from "react";
import axios from "axios";
import ProductForm from '../components/ProductForm' // this is new

const Main = () => {
    // creating the state to hold the array of products
    const [productList, setProductList] = useState([]);
    
    // create a function that executes a callback ONLY if the dependencies have changed between rendering
    // must have 2 arguments
    useEffect(() => {
        getProductList();
    }, []);

    // creating a function to handle setting the state
    const getProductList = () => {
        // calling the DB to get all the products
        axios
            .get("http://localhost:8000/api/products")
            .then((res) => {
                console.log(res.data);
                // setting state with all products
                setProductList(res.data);
            })
            .catch((err) => {
                console.log(err);
            });
    };
    // callback function to update list after creating a new product
    const handleNewProductCreate = () => {
        getProductList();
    };
    const removeFromDom = (id) => {
        // setting the state to a copy of the state by filtering out the product with specified id
        setProductList(productList.filter(product => product._id !== id));
    }; 

    return (
        <div>
            <h1>Product Manager</h1>
            <ProductForm onCreate={handleNewProductCreate}/>
            <!-- This is new. Passing down the removeFromDom function to the ProductList -->
            <ProductList productList={productList} removeFromDom={removeFromDom} /> 
        </div>
    );
};
export default Main;
```
- go to our src/views and add details view one (read one functionality)
```bash
# change FileName
touch FileNameDetails.jsx
```

```jsx
import React, {useEffect, useState} from 'react'
import { Link, useParams, useNavigate } from 'react-router-dom';
import axios from 'axios';


const ProductDetails = (props) => {
    // creating state to hold a product and setting the default value to a object
    const [ product, setProduct ] = useState({});
    // grabbing the id from the route
    const { id } = useParams();
    // creating navigate object 
    const navigate = useNavigate();

    useEffect(() =>{
        axios
            // on render call the DB with the id
            .get(`http://localhost:8000/api/products/${id}`)
            // set the the response into state
            .then(res => setProduct(res.data))
            .catch(err => console.log(err))
    }, []);

    const deleteProduct = (id) => {
        axios.delete(`http://localhost:8000/api/products/delete/${id}`)
            .then(res =>{
                // after the delete DB call nav back to home page
                navigate('/')
            })
            .catch(err =>{
                console.log(err)
            })
    }
    
    console.log(product)
    return (
        <div className='container'>
            <h3>Product Details</h3>
            <hr />
            <div className='container border border-dark'>
                <h4 className='mt-3'>Product Name : {product.title} </h4>
                <h5 className='mt-3'>Product Price : ${product.price}</h5>
                <h5 className='mt-3'>Product Description : {product.description} </h5>
                <Link className='btn btn-primary  m-2' to={`/`}>Home</Link>
                <Link className='btn btn-warning  m-2' to={`/products/edit/${product._id}`}>Edit</Link>
                <button className='btn btn-danger m-2' onClick={()=> deleteProduct(id)}>Delete</button>
            </div>
        </div>
    )
}

export default ProductDetails
```
- go to our src/views and add update view one (update one functionality)
```bash
# change FileName
touch FileNameUpdate.jsx
```
```jsx

import React, { useEffect, useState } from 'react'
import axios from 'axios'
import { useNavigate, useParams } from 'react-router-dom'

const ProductUpdate = (props) => {
    // grabbing the id from the route
    const { id } = useParams();
    // creating state for title, price, and description to manage form inputs
    const [title, setTitle] = useState("")
    const [price, setPrice] = useState(0)
    const [description, setDescription] = useState("")
    const [errors, setErrors] = useState([]); 
    // accessing the navigate function from react router
    const navigate = useNavigate()

    useEffect(() => {
        axios
            // calling the DB to get the product
            .get(`http://localhost:8000/api/products/${id}`)
            .then(res => {
                // updating the state with the response data retrieved 
                setTitle(res.data.title)
                setPrice(res.data.price)
                setDescription(res.data.description)
            })
    }, [id])
    // runs when onSubmit is triggered
    const updateProduct = (e) => {
        e.preventDefault();
        // Send a PUT request to backend API to update product by id 
        axios.put(`http://localhost:8000/api/products/edit/${id}`, {
            // passing in states to the put request
            title,
            price,
            description
        })
            .then(res => {
                // navigate back to the previous page
                navigate(-1)
            })
                
            .catch(err => {
                const errorResponse = err.response.data.errors;
                const errorArr = [];
                for (const key of Object.keys(errorResponse)) {
                    errorArr.push(errorResponse[key].message)
                }
                setErrors(errorArr);
            })
    }
    
    return (
        <div className="container">
            <form onSubmit={updateProduct}>
                {errors.map((err, index) => <p key={index}>{err}</p>)}
                <div className="form-group row mt-2">
                    <label htmlFor="title" className="col-sm-2 col-form-label">Title:</label>
                    <div className="col-sm-10">
                        {/*-- input to update product price */}
                        <input className="form-control" type="text" name="title" value={title} onChange={(e) => setTitle (e.target.value)} />
                    </div>
                </div>
                <div className="form-group row mt-2">
                    <label htmlFor="price" className="col-sm-2 col-form-label">Price:</label>
                    <div className="col-sm-10">
                        {/* same as above */}
                        <input className="form-control" type="number" name="price" value={price} onChange={(e) => setPrice (e.target.value)}/>
                    </div>
                </div>
                <div className="form-group row mt-2">
                    <label htmlFor="description" className="col-sm-2 col-form-label">Description:</label>
                    <div className="col-sm-10">
                        {/*-- same as above */}
                        <textarea className="form-control" name="description" id="description" cols="10" rows="3" value={description} onChange={(e) => setDescription (e.target.value)}/>
                    </div>
                </div>
                <button className="btn btn-success mt-3" type="submit">Update</button>
            </form>
        </div>
    )
}

export default ProductUpdate
```
- update our App.js file to include necessary imports and routes

```js
import React from 'react';
import './App.css';
import {Routes, Route} from  'react-router-dom'
import Main from './views/Main';
import ProductDetails from './views/ProductDetails';
import ProductUpdate from './views/ProductUpdate';


function App() {
  return (
    <div className="App">
      <Routes>
        <Route path='/' element={<Main/>}/>
        <Route path='/products/:id' element ={<ProductDetails/>}/>
        <Route path="/products/edit/:id" element={<ProductUpdate/>}/>
      </Routes>

    </div>
  );
}

export default App;
```