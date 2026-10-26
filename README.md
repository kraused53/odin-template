# odin-template
A skeleton HTML/CSS/JavaScript template for use in The Odin Project projects

## To setup:

Run these to initialize NPM modules and install plugins
```bash
npm init -y --init-type=module
npm install --save-dev webpack webpack-cli
npm install --save-dev html-webpack-plugin
npm install --save-dev style-loader css-loader
npm install --save-dev html-loader
npm install --save-dev webpack-dev-server
```

Open package.json and update the scripts section:
```JSON
scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack --config webpack.prod.js",
    "dev": "webpack serve --open --config webpack.dev.js",
    "deploy": "git subtree push --prefix dist origin gh-pages"
},
```

Create these three config files:

webpack.common.js
```JavaScript
 import path from 'node:path';
 import HtmlWebpackPlugin from "html-webpack-plugin";
 import { fileURLToPath } from 'node:url';

 const __filename = fileURLToPath(import.meta.url);
 const __dirname = path.dirname(__filename);

 export default {
    entry: "./src/index.js",
    output: {
        filename: "main.js",
        path: path.resolve(import.meta.dirname, "dist"),
        clean: true,
    },
    plugins: [
        new HtmlWebpackPlugin({
            template: "./src/template.html",
        }),
    ],
    module: {
        rules: [
            {
                test: /\.css$/i,
                use: ["style-loader", "css-loader"],
            },
            {
                test: /\.html$/i,
                use: ["html-loader"],
            },
        ],
    },
};
```

webpack.dev.js
```JavaScript
import { merge } from 'webpack-merge';
import common from './webpack.common.js';

export default merge(common, {
    mode: 'development',
    devtool: "eval-source-map",
    devServer: {
        watchFiles: ["./src/template.html"],
    },
});
```

webpack.prod.js
```JavaScript
import { merge } from 'webpack-merge';
import common from './webpack.common.js';

export default merge(common, {
    mode: 'production',
});
```

Commands:


This will build the production version of the webpage and store it inside dist/
```bash
npm run build
```


This will build the development version of the webpage and store it inside dist/
Then, it will start and open a development server.
```bash
npm run dev
```

To deploy to gh-pages:
```bash
npm run build
git checkout gh-pages && git merge main --no-edit
git add dist -f && git commit -m "Deployment commit"
git subtree push --prefix dist origin gh-pages
git checkout main
```