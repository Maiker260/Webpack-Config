# Webpack-Config
Instructions to setup webpack in a project

1. Initiate the Project:

		Run:
			npm init -y
			npm install webpack webpack-cli --save-dev

2. Create the “src” folder and move the files.

3. Make sure the script file is named “index.js”

		*In “package.json” remove the line:
			"main": "script.js",

4. Modify the “index.html” file, remove the script and add:

		<script src="../dist/main.js" defer></script>

5. Create the config file “webpack.config.js” and add the lines:

		const path = require('path');

		module.exports = {
  			entry: './src/index.js',
  			output: {
    				filename: 'main.js',
    				path: path.resolve(__dirname, 'dist'),
  			},
		};

6. Add the script in the “package.json”:

		"build": "webpack"

7. Now the “npm run build” command can be used in place of the npx command we used earlier. 

8. Load CSS to from within a JS file:

		Run:
			npm install --save-dev style-loader css-loader
			
9. Modify the “webpack.config.js” file and add inside “”Module.exports:

		module: {
			rules: [
				{
					test: /\.css$/i,
					use: ['style-loader', 'css-loader'],
				},
				{
					test: /\.(png|svg|jpg|jpeg|gif)$/i,
					type: 'asset/resource',
				},
			],
		},

10. Import the “style.css” file in the “index.js” file:

		import './style.css';

11. Minimize the CSS for Production Mode only:

		https://webpack.js.org/plugins/mini-css-extract-plugin/#minimizing-for-production

12. Modify “webpack.config.js” and add the lines in “module.exports”:

		entry: {
			index: './src/index.js',
		},

13. Modify “webpack.config.js” and add the lines in “output”:

		filename: '[name].bundle.js',

14. Install the HTML Webpack Plugin:

		npm install --save-dev html-webpack-plugin

15. Modify the “webpack.config.js” and add the lines:

		const HtmlWebpackPlugin = require('html-webpack-plugin');

16. Modify the “webpack.config.js” and add the lines into “module.export” after the “entry” section and change the “Title Section”:

		plugins: [
			new HtmlWebpackPlugin({
				title: ‘myProject’,
				template: './src/index.html',
			}),
		],

17. Cleaning up the “dist” folder, in the “webpack.config.js” add the following line in the “output” section:

		clean: true,

18. Setup the Dev/Prod Mode:

	Run:
		npm install --save-dev webpack-merge

19. Create the config files:

	webpack.common.js
	webpack.dev.js
	webpack.prod.js

20. Modify the file “webpack.common.js” and add the lines:

		const path = require('path');
		const HtmlWebpackPlugin = require('html-webpack-plugin');

		module.exports = {
			entry: {
				app: './src/index.js',
			},
			plugins: [
				new HtmlWebpackPlugin({
					title: 'Production',
					template: './src/index.html',
				}),
			],
			output: {
				filename: '[name].bundle.js',
				path: path.resolve(__dirname, 'dist'),
				clean: true,
			},
			module: {
        			rules: [
            				{
						test: /\.css$/i,
						use: ['style-loader', 'css-loader'],
					},
					{
						test: /\.(png|svg|jpg|jpeg|gif)$/i,
						type: 'asset/resource',
					},
				],
			},
		};

21. Modify the file “webpack.dev.js” and add the lines:

		const { merge } = require('webpack-merge');
		const common = require('./webpack.common.js');

		module.exports = merge(common, {
			mode: 'development',
			devtool: 'inline-source-map',
			devServer: {
				static: './dist',
			},
		});

22. Modify the file “webpack.prod.js” and add the lines:

		const { merge } = require('webpack-merge');
		const common = require('./webpack.common.js');

		module.exports = merge(common, {
			mode: 'production',
			devtool: 'source-map',
		});

23. Modify the file “package.json” and the script lines:

		"scripts": {
			"start": "webpack serve --open --config webpack.dev.js",
			"build": "webpack --config webpack.prod.js"
   		},

24. Modify the file “webpack.prod.js” and add the lines:

		const MiniCssExtractPlugin = require("mini-css-extract-plugin");
		const CssMinimizerPlugin = require("css-minimizer-webpack-plugin");

		module.exports = {
  			plugins: [
				new MiniCssExtractPlugin({
					filename: "[name].css",
					chunkFilename: "[id].css",
				}),
  			],
			module: {
				rules: [
					{
						test: /\.css$/,
						use: [MiniCssExtractPlugin.loader, "css-loader"],
					},
				],
			},
			optimization: {
				minimizer: [
					new CssMinimizerPlugin(),
				],
			},
		};


25. More:

	https://webpack.js.org/guides/production/
