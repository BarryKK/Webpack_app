<div>
 <img src="https://user-images.githubusercontent.com/19600132/72697210-1144ec80-3b7a-11ea-9e00-42f993fa36ec.png" > 
<div>


此项目用于学习Webpack

:pencil:此项目用到的loaders:
* Css-loader
* Sass-loader
* Style-loader
* Html-loader
* File-loader

:electric_plug:此项目用到的Plugins:
* html-webpack-plugin: generate a HTML file to serve webpack bundles.
* mini-css-extract-plugin: split css file.
* optimize-css-assets-webpack-plugin: minify css files.
* clean-webpack-plugin: clean build folder.

:paperclip:其他
* webpack-dev-server: watch changes
* webpack-merge: merge webpack files

## Webpack Configuration
:pushpin:webpack common
```javascript
module.exports = {
  entry: {
    main: "./src/index.js",
    vendor: "./src/vendor.js"
  },
  module: {
    rules: [
      {
        test: /\.html$/,
        use: ["html-loader"]
      },
      {
        test: /\.(svg|png|jpg|gif)$/,
        use: {
          loader: "file-loader",
          options: {
            name: "[name].[hash].[ext]",
            esModule: false,
            outputPath: "imgs"
          }
        }
      }
    ]
  }
};

```

:pushpin:webpack development
```javascript
const path = require("path");
const common = require("./webpack.common");
const merge = require("webpack-merge");
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = merge(common, {
  mode: "development",
  output: {
    filename: "[name].bundle.js",
    path: path.resolve(__dirname, "dist")
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: "./src/template.html"
    })
  ],
  module: {
    rules: [
      {
        test: /\.scss$/,
        use: [
          "style-loader", //3.Inject styles into DOM
          "css-loader", //2.Turns css into commonjs
          "sass-loader" //1.Turns sass into css
        ]
      }
    ]
  }
});
```

:pushpin:webpack production
```javascript
const path = require("path");
const common = require("./webpack.common");
const merge = require("webpack-merge");
const { CleanWebpackPlugin } = require("clean-webpack-plugin");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const OptimizeCssAssetsPlugin = require("optimize-css-assets-webpack-plugin");
const TerserPlugin = require("terser-webpack-plugin");
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = merge(common, {
  mode: "production",
  output: {
    filename: "[name].[contentHash].bundle.js",
    path: path.resolve(__dirname, "dist")
  },
  optimization: {
    minimizer: [
      new OptimizeCssAssetsPlugin(),
      new TerserPlugin(),
      new HtmlWebpackPlugin({
        template: "./src/template.html",
        minify: {
          removeAttributeQuotes: true,
          collapseWhitespace: true,
          removeComments: true
        }
      })
    ]
  },
  plugins: [
    new MiniCssExtractPlugin({ filename: "[name].[contenthash].css" }),
    new CleanWebpackPlugin()
  ],
  module: {
    rules: [
      {
        test: /\.scss$/,
        use: [
          MiniCssExtractPlugin.loader, //3.Extract css into files
          "css-loader", //2.Turns css into commonjs
          "sass-loader" //1.Turns sass into css
        ]
      }
    ]
  }
});
```

