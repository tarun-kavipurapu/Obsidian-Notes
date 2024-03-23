## 1.

Node.js is a JavaScript runtime environment that uses the V8 engine outside of the web browser. Hence, it opens up the possibilities of backend server development.

![[Images-attachments/Untitled 33.png|Untitled 33.png]]

## Reading and Writing Files

```JavaScript
const fs = require("fs"); //!Importing the filesystem

const input = fs.readFileSync("./txt/input.txt", "utf-8");//!if utf-8 not mentioned it retturns buffer value
//!Nou write something in a file.
fs.writeFileSync('./txt/output.txt',`This is a just a test  case i am writing now at ${Date.now()}`);
console.log(input);
```

  

## Blocking and Non-Blocking: Asynchronous Nature of Node.js

![[Images-attachments/Untitled 1 10.png|Untitled 1 10.png]]

![[Images-attachments/Untitled 2 9.png|Untitled 2 9.png]]

_**Think deeply of Why asynchronous code is preferred and used?**_

![[Images-attachments/Untitled 3 8.png|Untitled 3 8.png]]

![[Images-attachments/Untitled 4 6.png|Untitled 4 6.png]]

## Implementing asynchronous code

```JavaScript
fs.readFile('./1-node-farm/starter/txt/start.txt', 'utf-8', (err, data1) => {
  // console.log(data);
if (err) return console.log(err);
  fs.readFile(`././1-node-farm/starter/txt/${data1}`,'utf-8',(err,data2)=>{
      fs.readFile(`././1-node-farm/starter/txt/append.txt`,'utf-8',(err,data3)=>{
        fs.writeFile(`./1-node-farm/starter/txt/final.txt`,
        `${data2}\n${data3}`,
        'utf-8',err=>{
          console.log('Your file hass been written');
        })
      })
  })
});
//basically maanipulating using input and output files.
```

## Creating a simple web Server

  

## Routing to Different Locations in webserver

```JavaScript
const fs = require("fs");
const http = require("http");
const url = require("url");


//note that create server gets executed over and over hence advised to use async function there.  
const server = http.createServer((req, res) => {//reapeating code because it is called everytime there is sconnecctionn to a server by a client
  const pathName = req.url;

  if (pathName === "/" || pathName === "/overview") {
    res.end("This is the overview");//routing to overview
  } else if (pathName === "/product") {
    res.end("This is the product");//routing to product
  } else {
    res.writeHead(404, {//sending error code 404
      "Content-type": "text/html",
      "my-own-header": "hello-world",//sending headers
    });
    res.end("Page Not found");//sending error message
  }
});

server.listen(8000, "127.0.0.1", () =>
  console.log("Listening to requests on port 8000")
);
```

## Creating a small api

```JavaScript
const fs = require("fs");
const http = require("http");
const url = require("url");

const data = fs.readFileSync(`${__dirname}/dev-data/data.json`,'utf-8');//we are using sync because it does not repeat it is optimal usage beacuase we are reading json only one time and using it repeating code of server.
const dataObj = JSON.parse(data);
const server = http.createServer((req, res) => {//reapeating code because it is called everytime there is sconnecctionn to a server by a client
  const pathName = req.url;


  if (pathName === "/" || pathName === "/overview") {
    res.end("This is the overview");
  } else if (pathName === "/product") {
    res.end("This is the product");
  } else if (pathName === "/api") {
      // console.log(productData);
      res.writeHead(200,{'content-type': 'application/json'});
      res.end(data)//calling data variable
  } 
  else {
    res.writeHead(404, {
      "Content-type": "text/html",
      "my-own-header": "hello-world",
    });
    res.end("Page Not found");
  }
});

server.listen(8000, "127.0.0.1", () =>
  console.log("Listening to requests on port 8000")
);
```

_**Make sure to Replace HTML doccument with necessary placeholders before coming to the server ;**_

## Replacing Placeholders in HTML by api json data object watch lecture for more

```JavaScript
const fs = require("fs");
const http = require("http");
const url = require("url");

const replaceTemplate = require('./modules/replaceTemplate');
const tempOverview = fs.readFileSync(
  `${__dirname}/templates/template-overview.html`,
  "utf-8"
);
const tempCard = fs.readFileSync(
  `${__dirname}/templates/template-card.html`,
  "utf-8"
);
const tempProducts = fs.readFileSync(
  `${__dirname}/templates/template-product.html`,
  "utf-8"
);


const data = fs.readFileSync(`${__dirname}/dev-data/data.json`, "utf-8");
const dataObj = JSON.parse(data);//json oobject in the starter files 


const server = http.createServer((req, res) => {
  // const pathName = req.url;
const {query,pathname}=url.parse(req.url , true)
  //Overview
  if (pathname === "/" || pathname === "/overview") {
    res.writeHead(200, { "Content-type": "text/html" });
    const cardsHtml = dataObj.map((ele) => replaceTemplate(tempCard, ele));
    // console.log(cardsHtml);
    let output = tempOverview.replace(/{%PRODUCT_CARDS%}/g,cardsHtml);
    res.end(output);
  }
  //product section
  else if (pathname === "/product") {
    const product = dataObj[query.id];
    const output = replaceTemplate(tempProducts,product);
    res.writeHead(200, { "Content-type": "text/html" });
    res.end(output);
  }
  //api
  else if (pathname === "/api") {
    // console.log(productData);
    res.writeHead(200, { "content-type": "application/json" });
    res.end(data);
  } else {
    res.writeHead(404, {
      "Content-type": "text/html",
      "my-own-header": "hello-world",
    });
    res.end("Page Not found");
  }
});

server.listen(8000, "127.0.0.1", () =>
  console.log("Listening to requests on port 8000")
);
```

```JavaScript
module.exports= (temp, product) => {
  let output = temp.replace(/{%IMAGE%}/g, product.image);
  output = output.replace(/{%PRODUCTNAME%}/g, product.productName);
  output = output.replace(/{%PRICE%}/g, product.price);
  output = output.replace(/{%FROM%}/g, product.from);
  output = output.replace(/{%NUTRIENTS%}/g, product.nutrients);
  output = output.replace(/{%QUANTITY%}/g, product.quantity);
  output = output.replace(/{%DESCRIPTION%}/g, product.description);
  output = output.replace(/{%ID%}/g, product.id);
  if (product.organic === false) {
    output = output.replace(/{%NOT_ORGANIC%}/g, "not-organic");
  }

  return output;
};
```

Read about slugify module in npm and also learnt handling npm from lectures

Node_modules folder should not be uploaded on github since it has data on all modules and a waste of space only configuration of `package.json` is enough and `npm install` will install all packages innto node_modules

Always share `package.json` and `package-lock.json`

_**How A web works? —→ checkout inn javascript section**_

![[Images-attachments/Untitled 5 6.png|Untitled 5 6.png]]