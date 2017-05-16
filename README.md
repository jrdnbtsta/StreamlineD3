## <b>Project is currently in development</b>

# StreamlineD3
Providing developers with a simple way to create live updating visualizations for their streaming data.

## Features
  * Customize colors, fonts, heights, widths, and more.
  * Render only the data that changes for blazing fast performance with our diffing algorithm.
  * Load balancing through Node clusters lets your app scale to multiple streams and visualizations.
  * Pre-built ready to use visualizations including: Bar, Line, Scatter, Pie, Bubble, Word-Cloud, and World Map.
  
## Getting Started

1. ```npm install streamlined3``` to install our library. <br/>
2. create an index.js file
3. create an index.html file
  
### Index.js

NOTE: because our library uses load balancing through Node clusters on the back end to support scaling and optimal performance, it is important for you to NOT make a traditional server using Node or Express.  The functionality is already set up for you and no worries, you can still customize it. Follow the instructions below for more info. 
     
1. In your index.js file require our library:
```
const streamline = require('streamlined3');
```
2. Create a new instance, passing a callback and a port#:<br/>
```
let myStream = new streamline(sendFiles, port#);
```

note: please choose any port other than 6379 (as our Redis server will be running on this port)

3. Put all the code/routes/etc. you would normally want in your server file inside this function:
```
function sendFiles(app) {
  app.use(express.static(path.join(__dirname, 'client')));

  app.get('/', (req, res) => {
    res.sendFile(path.join(__dirname, 'client/home-page.html'));
  });
}
```

4. Create an array ```let myData = []``` to hold your data.  Call your API to get streaming data and push the results into your myData array.  FYI, it is a good idea to buffer this data to not overload your system.  

5. Look at your API data key/value pairs and find the data values that you want to visualize in your graph.  Put them inside a config object. (see Specific Configuration Settings for... below for what type of key/value pairs you'll need for each visualization):
```
let config = {
  width:      500,
  height:     500,
  xdomain:    10,
  ydomain:    10,
  xticks:     10,
  yticks:     10,
  xScale:     post
  yScale:     number-of-likes
  xLabel:     'the name of the post',
  yLabel:     'how many likes someone got'
};
```
6. Invoke the StreamlineD3 ```connect``` method on the new instance you created in step 2.  For names of methods you can call, see Specific Configuration Settings for... below for each type of visualization. <br/>
```
myStream.connect((socket) => {
  myStream.line(socket, myData, config);
});
```

### Install Redis and Start Your Servers

Because sockets and Node clusters don't work well togther without additional measures taken, you must install Redis and start a Redis server. The easiest way is through HomeBrew.  Download Homebrew https://brew.sh/ and then in the terminal ```$ brew install redis```.  Once Redis is installed ```$redis-server``` to start a Redis server.  Lastly, type ```$ node index.js``` or whatever you named your js file to start the initial Node server we set up for you in our library.  Note: all these steps assume you are using a mac.  If you're using windows, these Bash commands will not work.  

### HTML
      
1. Add our library as a script by either using the CDN or choose the graph files you want and add them individually: 
```
<script src="line.js"></script>
```

OR <br />

```
  <script src="http://cdn.jsdelivr.net/gh/StreamlineD3/SD3-Demo@1.2/client/graphs/bundle.min.js"></script>
```
2. Add the dependencies: d3 (please ensure you are using version 4) and socket.io
```
  <script src="https://cdnjs.cloudflare.com/ajax/libs/socket.io/1.7.3/socket.io.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/d3/4.8.0/d3.min.js"></script>
```

3. Add a ```<div>``` node with an id of ```(see Specific Configuration Settings for... below for what each visualization is called)``` where you want your visualization to appear:
```<div id="Name-Of-Visualization"></div>```
      
And voilà! You now have a working, live-updating visualization.

## Specific Configuration Settings for the Bar Graph
  method name
  config settings
  html id name of div

## Specific Configuration Settings for the Line Graph

## Specific Configuration Settings for the Bubble Graph

## Specific Configuration Settings for the Word-Cloud

## Specific Configuration Settings for the Pie Chart

## Specific Configuration Settings for the Scatter
