## An Overview of Modern JavaScript Development

![[Images-attachments/Untitled 31.png|Untitled 31.png]]

## An overview of Modules

![[Images-attachments/Untitled 1 8.png|Untitled 1 8.png]]

![[Images-attachments/Untitled 2 7.png|Untitled 2 7.png]]

![[Images-attachments/Untitled 3 6.png|Untitled 3 6.png]]

## Examples

Sure! Here are some examples of importing and exporting modules in JavaScript using the ES6 module syntax:

1. Exporting a module:
    
    ```Plain
    // math.js
    export const add = (a, b) => a + b;
    export const subtract = (a, b) => a - b;
    ```
    
2. Importing a module:
    
    ```Plain
    // app.js
    import { add, subtract } from './math.js';
    
    console.log(add(5, 3)); // Output: 8
    console.log(subtract(10, 4)); // Output: 6
    ```
    

In the above example, we have a module named `math.js` that exports two functions `add` and `subtract`. In the `app.js` file, we import those functions using the `import` statement and specify the path to the module file. We can then use the imported functions as if they were defined locally in `app.js`.

Now, regarding your question about hoisting of modules:

In JavaScript, module imports and exports are statically analyzed during the compilation phase, which means they are resolved before the code is executed. This is known as "static module resolution" or "import/export hoisting." It ensures that all imports are processed before the code runs, and the dependencies are resolved correctly.

The hoisting behavior of modules ensures that imported module bindings (variables, functions, or classes) are available in the importing module's scope during runtime. This allows you to use the imported entities without worrying about the order of import statements in your code.

However, it's important to note that module imports themselves are not hoisted to the top of the file. The import statements are evaluated in the order they appear in the module, but their bindings are only available after the import statement has been executed.

In summary, import and export statements are hoisted during module initialization, ensuring that imported entities are available in the importing module's scope during runtime. This enables you to use those entities without any concerns about the order of imports.