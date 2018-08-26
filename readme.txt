Webpack tutorial. MJS 8.24.18 
---------------------------------------------------
- See the readme.md file if you wish to use this tutorial to learn very basic webpack.
- The readme.md file in this directory should be an exact copy of the minimal-setup.md file 
submitted to webpack.js.org on 8.26.18.  
- This file, readme.txt, contains text on the same topics as the readme.md file.  However 
it is not as complete or precise. It also includes some more npm instructions
- Instructions on using webpack.config.js, even for entry and output files, is no longer 
part of this file.  See the wpconfig folder for webpack.config.js issues including 
entry, output, loaders (css, babel, vue SFCs) and plug-ins.

------------------------------------------------------------------------------

This tutorial uses NPM, webpack and JavaScript to learn basic webpack. 
Since it is very hard to find minimal instructions on running a first 
webpack command we first concentrate on this.  Then we introduce progressively 
more complex webpack features.

Part I. Minimal Webpack Command
-----------------------------------------------------
A. Installation
-----------------------------------------------------
Webpack must be installed before running it. This is almost always done using NPM. Hence NPM must be installed. This is normally done by installing Node.js. While full NPM details are NOT covered in this tutorial, a basic setup consists of the following. 

a. Download and install node.js at https://nodejs.org/  Node.js includes NPM by default. Reboot and verify that npm has been installed using "npm -v" which prints the version number (currently 5.6.0). 
b. Create a directory for your project such as webpackBasic.  cd into it. 
c. Install both webpack and the webpack-cli using 'npm install webpack webpack-cli --global'. While we will only use webpack, the system will prompt you to install webpack-cli when you first run webpack, so it is easiest just to install it now. You may verify webpack has install correctly by typing 'webpack -v' which prints the version number (currently 4.17.1). 
 
B. Simpliest Webpack Command - webpack <entryFilename.js>
----------------------------------------------------------
1. The simpliest webpack command in the current version of webpack is "webpack <entryFilename.js>".
Before issuing this command the <entryFilename.js> file must be created. Create a file named "entryMain.js" with the content: console.log('Console.log message inside entryMain.js')

2. Type "webpack entryMain.js". You should see output similar to the following: 

PS C:\Users\Mike\desktop\js\temp> webpack entryMain.js
Hash: ae8ebe3170f4c6373946
Version: webpack 4.17.1
Time: 1660ms
Built at: 2018-08-25 11:31:33
  Asset       Size  Chunks             Chunk Names
main.js  984 bytes       0  [emitted]  main
Entrypoint main = main.js
[0] ./entryMain.js 56 bytes {0} [built]

WARNING in configuration
The 'mode' option has not been set, webpack will fallback to 'production' for this value. Set 'mode' option to 'development' or 'production' to enable defaults for each environment.
You can also set it to 'none' to disable any default behavior. Learn more: https://webpack.js.org/concepts/mode/

Congratulations, you have webpacked your first file!!! 

3. Confirm that webpack created a new directory and "bundled" file. 
Ignore the warning about the mode option for now. Confirm that a dist folder has been created inside your project folder. Change into it and confirm that a main.js file has been created. The dist folder is the default location for the bundled file and webpack created it automatically since it did not exist.

Similarly webpack bundled the entryMain.js file into a main.js file.  This can be seen in the output line: Entrypoint main = main.js.  Open the main.js file and observe that it consists of javascript code, most likely without line breaks. You should be able to find the content of your entryMain.js file inside the webpacked main.js file, likely near the end of the file. 

C. Using a webpacked file.
-----------------------------
1. By far the most common way to use a webpacked file is to create an html file named index.html, main.html or similar. This file is often (but not always) empty except for <script src="bundledfilename.js"></script>. In this case we start with a file named indexMain.html whose content is: 

<!DOCTYPE html>
<html>
	<body> 
		Webpack indexMain.html Body Text (With only: script src='./dist/main.js').
		<br> <script src='./dist/main.js'></script>
	</body>
</html>

2. Notice that the above file attempts to use the "dist/main.js" file created by webpack. Such a file is referred to as a bundled file since it consists of one or more javascript files bundled together by webpack. 

Why do we use this "bundled" file when the original entryMain.js file could have been used instead?  Great Question!  In a real world application you will likely have hundreds of javascript files with complex dependencies.  Webpack will help keep track of these dependencies, ensure they are used in the correct order and bundle all the javascript files together into a single file. In this minimal example we only bundled one file named entryMain.js, but in a production application webpack will efficiently handle hundred of files.   

3. You should now be able to load your indexMain.html file. In our case we entered 
file:///C:/Users/Mike/Desktop/js/wpbasic/indexMain.html in a chrome browser. You will, of course, need to substitute your pathname for the above.  The expected output of: "Webpack indexMain.html Body Text (With only: script src='./dist/main.js').", should be displayed.  This is the text found in indexMain.html. Use chrome developer tools (f12) or another tool to bring up a console window.  You should see the message from the entryMain.js file: "Console.log message inside entryMain.js". 

----------------------------------------------
II. Improving on the Simpliest Webpack Command
----------------------------------------------

A. Eliminating the Mode Nag Warning 
------------------------------------
When we webpacked our file a warning message about the mode being undefined was generated.

WARNING in configuration
The 'mode' option has not been set, webpack will fallback to 'production' for this value. Set 'mode' option to 'development' or 'production' to enable defaults for each environment.
You can also set it to 'none' to disable any default behavior. Learn more: https://webpack.js.org/concepts/mode/
 
We wish to get rid of this "nag" warning.  As noted in the warning there are 3 basic modes, production, development and none.  Development mode best suits our needs so add --mode development to the webpack command.  Type "webpack entryMain.js --mode development".  You should see the same output as before but without the warning message. 

Now open the dist/main.js file.  You should see that the file is the same as before except that the text has line breaks.  This is one of the advantages of using development mode.  Development mode is also optimized for speed during webpacking while production mode is optimized for speed while running the resulting entryMain.js file. 

B. Changing the Bundled Filename and Location
----------------------------------------------
So far our output has been automatically generated in dist/main.js. This can be changed using the --output <outputFileName> or -o <outputFileName> options. Note the double dash in --output, but the single dash in -o.  This is a common convention.  Full names for options tend to be preceded by a double dash while abbreviated names tend to be preceded by a single dash. As a second example "npm install <filename> --global" (which we saw before) is equivalent to "npm install <filename> -g".  

From now on we will name our webpacked bundle file "bundle.js". This is a common convention and helps to distingush this file from the default main.js file. Type "webpack entryMain.js --output bundle.js --mode development". You should see output confirming the bundle.js file was created. Examine it and confirm that it is correct. But note that it is in current directory. This is not where we wish to keep it. As with the default main.js file, and as discussed further below the bundled webpack files are almost always kept in the dist folder. Delete the bundle.js file and type "webpack entryMain.js -o dist/bundle.js --mode development". Note that for learning purposes we used -o here instead of --output.  They are equivalent.  The bundle.js file should show up in the dist folder. 

Finally note that "webpack <entryfilename> <outputfilename>" (without --output or -o) does NOT work, (despite what some older tutorials may say, this likely worked with older version of webpack). 

C. src Folder - Bookkeeping Multiple Examples 
----------------------------------------------
We now have two bundled files, main.js and bundle.js. It's time for a little bookkeeping. While we could include the bundle.js file in the current indexMain.html file we wish to keep the previously generated "main" files distinct from the newer files. Typically the index.html type file is kept in the root directory of the project while the various javascript files such as entry.js are kept in a source directory. This comes in very handy as we create more and more javascript files that depend upon eachother. 

Copy indexMain.html to indexBundle.html. Create a src folder and copy entryMain.js to src/entryBundle.js. (We'll leave entryMain.js where it is for reference purposes.) From now on we will work with the indexBundle.html, src/entryBundle.js files. 

After copying the files we will need to edit indexBundle.html to point to the new bundled file, bundle.js. Edit indexBundle.html. Also update it's text to reflect it is indexBundle.html instead of indexMain.html. Similarly modify the console.log message in entryBundle.js. Now "webpack src/entryBundle.js -o dist/bundle.js --mode distribution".  Change your URL to .../indexBundle.html and you should now be able to see the updated text inside indexBundle.html as well as the updated console message.

Having an index.html file in the root folder, all javascript input files in a src sub-folder, and the webpacked bundle.js file in a dist sub-folder is the accepted practice for most webpack projects. 

----------------------------------------------
II. Webpacking Multiple Files
----------------------------------------------
Webpack automatically "sucks in" and bundles multiple *.js files as needed.  The simpliest was to do this is to use a require statement in your "entry" file. The required file(s) will automatically be bundled along with the entry file. 

Before proceeding with a more complex example that includes multiple auto-bundled files we first do a little more book keeping. 

A. Typical File Structure and NPM
-----------------------------------
1. Create a package.json file by typing 'npm install -y'.  You should examine this file and note its structure.

3B. The entry file may only include ES5 commands using this option.  Most noteably "require" commands may be used. 

Almost all webpack projects use a standardized directory structure. In the "root" project folder are the index.html file and npm's package.json file. All user defined *.js files go in a "src" folder, while the bundled result of webpack goes in a "dist" folder. You should confirm this structure for this project. Also, note that the webpack commands could be 
"webpack ./src/entry.js -o ./dist/bundle.js".

As a simple example, in the entry.js file include: require('./two.js') and require('./three.js').  In two.js and three.js console.log messages.  You may, if you wish, also include html in your index.html file such as <p class=p1>P1 index msg</p>.  This p tag may then be modified inside your *.js files using document.getElementById("p1).innerText = "text from .js file".

D. Examing the resulting "bundle.js" file. 
-------------------------------------------
Take a look at the resulting bundle.js file. It may be opened with notepad or another text editor. You should be able to search through it and find the JavaScript commands from your original entry.js, two.js and three.js files. 

