# Webpack con React

Esta es la configuración básica para trabajar con React usando webpack y sin necesidad de emplear create react app.

1. Crear las carpetas y archivos

```
-src
-webpack.config.js
```

1. Instalar

```
$ npm webpack webpack-cli
```

1. Dentro de la carpeta src crear los archivos index.js e index.html.
1. Configurar el archivo webpack.config.js

```
const path = require('path');
module.exports{
  entry:"./src/index.js",
  output:{
    path:path.resolve(__dirname,'dist'),
    filename:"bundle.js"
  }
};
```

Tenemos que decirle dónde se encuentra nuestro archivo de origen, que es index.js y dónde va a colocar el archivo bundle.js.  
Hasta este momento sólo capta código de JavaScript y aun no compila **React** , por lo que para probarlo hay que usar solo JS.  
Se ejecuta la prueba con el comando `npx webpack --mode development` .  
Se tiene que crear el archivo bundle.js en la carpeta dist.

1. Instalar html-webpack-plugin para integrar el archivo html en la carpeta dist. Configurar webpack

```
const HtmlWebpackPlugin = require('html-webpack-plugin');
...
module:{
  rules:[]
},
plugins:[
  new HtmlWebpackPlugin({
    template:'./src/index.html
  })
]
```

1. Instalar el entorno de desarrollo `webpack-dev-server`.
1. Configurar el dev-server en el archivo config antes de "module".

```
...
devServer:{
  port:4000,
  hot:true,
  watch:true,
  open:true
}
```

Ejecutar con el comando `npx webpack serve --mode development` apartir de webpack 5

```
npm i react react-dom @babel/core @babel/cli  @babel/preset-env
```

Configurar el archivo .bablerc. El archivo babel es un objeto JSON

```
{
  "presets":["@babel/preset-env"],
  "plugins":[]
}
```

1. Decirle a webpack que comience a usar babel, para ello en webpack se le añade una regla para testear los archivos js y jsx.

```
rules:[
  {
    test:/\.(js|jsx)$/,
    use:["babel-loader"],
    exclude:/node_modules/
    }
]
```

Adicionalmente añadir el objeto resolve con las extensiones de js y jsx

```
...
resolve:{extensions:['.js','.jsx']
module:...
}
```

1. Añadirle a los presets de babel e instalar el @babel/preset-react
1. Instalar un plugi para que acepte el uso de state en react @babel/plugin-proposal-class-properties en el archivo babel añadir un array de plugins con el nuevo plugin.
1. Añadir el css-loader y Minicssextractplugin.loader como loaders de estilos en css.
1. Instalar el loader para imágenes file-loader.
1. Añadir la configuración para producción.
1. Instalar el plugin para minificar los archivos css MiniCssExtractPlugin y realizar la configuración correspondiente y añadirlo a los loaders de css.
1. Activar el TerserPlugin de que viene con Webpack.

```
const devMode = process.env.NODE_ENV !== "production";
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const CssMinimizerPlugin = require("css-minimizer-webpack-plugin");
const TerserPlugin = require("terser-webpack-plugin");

module.exports = {
  entry: "./src/index.js",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "bundle.js",
  },
  devServer: {
    port: 4000,
    hot: true,
    open: true,
  },
  mode: "development",
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        use: ["babel-loader"],
        exclude: /node_modules/,
      },
      {
        test: /\.css$/,
        use: [MiniCssExtractPlugin.loader, "css-loader"],
      },
    ],
  },

  plugins: [
    new HtmlWebpackPlugin({
      template: "./src/index.html",
    }),
    new MiniCssExtractPlugin({
      filename: "[name].css",
    }),
  ],
  optimization: {
    minimize: true,
    minimizer: [
      // For webpack@5 you can use the `...` syntax to extend existing minimizers (i.e. `terser-webpack-plugin`), uncomment the next line
      // `...`
      new CssMinimizerPlugin({ test: /\.css(\?.*)?$/i }),
      new TerserPlugin({ test: /\.js(\?.*)?$/i }),
    ],
  },
};


```
