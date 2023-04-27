> <span style="color:red">Currently unimplemented.</span>
# Spreadsheet
The spreadsheet is essentially just a web application that hooks directly into the [economy](/src/mech/economy.md) and works with the [Market](/src/mech/economy/market.md) system.

## Overview

Our goal is to build a dynamic website that displays the prices of items on a per-region and global basis. We want to hook the website directly into the server, so we can query the prices set for a specific item per region, as well as querying the global price of a specific item. The website should display all this information in a visually appealing format and update the prices in real-time, or at least once a day at a certain time.

To achieve this, we need to:

1. Connect the website to the server
2. Create an API within the Market plugin to retrieve price data
3. Organize the data into a spreadsheet format
4. Display the data on the website
5. Implement real-time updates, or update the prices once a day at a certain time

Let's break these steps down further:

### 1. Connect the website to the server
We need to connect the website to the Minecraft server running AirshipCraft. Since Spigot's built-in API doesn't provide us with a way to directly communicate with a web server, we'll need to create our own API within the Market plugin. 

#### ***Create an HTTP endpoint***

We can create an HTTP endpoint in the Market plugin that handles incoming requests from the website. Here's some pseudocode that demonstrates how we might achieve this:

```java
import org.bukkit.plugin.java.JavaPlugin;
import org.bukkit.Material;
import spark.*;

public class MarketPlugin extends JavaPlugin {

    private static final int PORT = 8000;
    private static final String API_PATH = "/api/market";

    @Override
    public void onEnable() {
        Spark.port(PORT);
        Spark.path(API_PATH, () -> {
            Spark.get("/price/:item/:region", (req, res) -> {
                String itemName = req.params("item");
                String regionName = req.params("region");
                double price = getPrice(itemName, regionName);
                return Double.toString(price);
            });
        });
    }

    private double getPrice(Material itemName, String regionName) {
        // Retrieve the price for the given item in the given region
        // ...
        return price;
    }

    private double getPrice(String itemName, String regionName) {
        // Retrieve the price for the given item in the given region
        // ...
        return price;
    }

}
```

This code creates an HTTP endpoint at `localhost:8000/api/market/price/:item/:region`. When a request is made to this endpoint, the `getPrice()` method is called, which retrieves the price for the given item in the given region using Spigot's API. The price is then returned as a string.

#### ***Set up a web server***

We'll need to set up a web server that can handle HTTP requests from the website and communicate with the Market plugin's API.
   
Here's some pseudocode that demonstrates how we might achieve this using the Spark Java web framework:

```java
import spark.*;
import com.google.gson.Gson;

public class SpreadsheetServer {

    private static final String MARKET_API_URL = "http://localhost:8000/api/market";
    private static final Gson gson = new Gson();

    public static void main(String[] args) {
        Spark.port(8080);
        Spark.staticFileLocation("/public");

        Spark.get("/price/:item/:region", (req, res) -> {
            String itemName = req.params("item");
            String regionName = req.params("region");
            String url = MARKET_API_URL + "/price/" + itemName + "/" + regionName;
            String priceString = HttpUtils.get(url);
            double price = Double.parseDouble(priceString);
            return gson.toJson(price);
        });
    }

}
```

This code sets up a web server on port 8080 that serves static files from the `/public` directory. When a request is made to `/price/:item/:region`, the server makes a request to the Market plugin's API to retrieve the price for the given item in the given region. The price is then returned as a JSON object.

#### ***Front-end integration***

Certainly! After we have established a connection between the website and the server, we can move on to integrating the front end. The front end is responsible for displaying the data retrieved from the server in a user-friendly and visually appealing format.

To achieve this, we can use a front-end framework like React or Angular, which will provide us with the necessary tools to create a dynamic and responsive user interface. We can also use libraries like Chart.js or D3.js to create interactive data visualizations.

Here is some pseudocode for the front-end integration:

```java
// Define the front-end application
public class SpreadsheetApplication extends Application {

    // Initialize the application
    public void init() {
        // Connect to the server
        connectToServer();
    }

    // Connect to the server
    private void connectToServer() {
        // Use AJAX to send an HTTP request to the server
        XMLHttpRequest request = new XMLHttpRequest();
        request.open("GET", "http://localhost:8080/prices");
        request.onload = () -> {
            if (request.status == 200) {
                // Retrieve the price data from the response
                String responseText = request.responseText;
                JSONArray priceData = new JSONArray(responseText);
                
                // Render the data on the page
                renderSpreadsheet(priceData);
            }
        };
        request.send();
    }

    // Render the spreadsheet on the page
    private void renderSpreadsheet(JSONArray priceData) {
        // Use React or Angular to render the data
        // Use Chart.js or D3.js to create data visualizations
        // Use CSS to style the page
        // Use JavaScript to add interactivity
    }

}

// Start the application
public static void main(String[] args) {
    launch(args);
}
```

In this example, we use AJAX to send an HTTP request to the server and retrieve the price data in JSON format. We then use React or Angular to render the data on the page and use Chart.js or D3.js to create data visualizations. Finally, we use CSS to style the page and JavaScript to add interactivity.

This is just a high-level overview of the front-end integration process, but it should give you an idea of how we can create a dynamic and visually appealing user interface for our spreadsheet application.

Certainly! Here are some code examples for using React and Chart.js to render and visualize data on the front-end:

#### ***Rendering data with React***

We can use React to create a dynamic user interface that updates in real-time as new data is received from the server. Here's an example of how we might use React to render a table of item prices:

```jsx
import React, { useState, useEffect } from 'react';
import axios from 'axios';

function ItemPrices() {
  const [prices, setPrices] = useState([]);

  useEffect(() => {
    axios.get('/api/prices')
      .then(response => {
        setPrices(response.data);
      })
      .catch(error => {
        console.error(error);
      });
  }, []);

  return (
    <table>
      <thead>
        <tr>
          <th>Item</th>
          <th>Region</th>
          <th>Price</th>
        </tr>
      </thead>
      <tbody>
        {prices.map((price, index) => (
          <tr key={index}>
            <td>{price.item}</td>
            <td>{price.region}</td>
            <td>{price.price}</td>
          </tr>
        ))}
      </tbody>
    </table>
  );
}
```

In this example, we use the `useState` and `useEffect` hooks to manage the state of the `prices` array. We make a GET request to the `/api/prices` endpoint to retrieve the latest prices from the server, and update the state with the response data. We then use the `map` function to render each item in the `prices` array as a table row.

#### ***Visualizing data with Chart.js***

We can use Chart.js to create data visualizations that help users understand the trends and patterns in the data. Here's an example of how we might use Chart.js to create a line chart of item prices over time:

```jsx
import React, { useState, useEffect } from 'react';
import axios from 'axios';
import { Line } from 'react-chartjs-2';

function PriceChart() {
  const [data, setData] = useState({});

  useEffect(() => {
    axios.get('/api/prices')
      .then(response => {
        const labels = response.data.map(price => price.date);
        const prices = response.data.map(price => price.price);
        setData({
          labels: labels,
          datasets: [
            {
              label: 'Item Prices',
              data: prices,
              fill: false,
              borderColor: 'rgb(75, 192, 192)',
              tension: 0.1
            }
          ]
        });
      })
      .catch(error => {
        console.error(error);
      });
  }, []);

  return (
    <Line data={data} />
  );
}
```

In this example, we use the `useState` and `useEffect` hooks to manage the state of the `data` object. We make a GET request to the `/api/prices` endpoint to retrieve the latest prices from the server, and format the data as an object that can be consumed by Chart.js. We then pass the `data` object to the `Line` component, which renders a line chart with the item prices over time.

### 2. Create an API within the Market plugin to retrieve price data

We'll need to modify the Market plugin to expose an API that we can use to retrieve the prices of all the items in the game. We can do this by adding methods to the plugin that retrieve the prices for each item in each region. We'll need to store this data in a database or other data storage system so we can access it easily and organize it into a spreadsheet format.

### 3. Organize the data into a spreadsheet format

Once we have the price data, we'll need to organize it into a spreadsheet format. We can use a spreadsheet library or write our own code to organize the data into rows and columns. We'll also need to create a user interface for the website that displays this data in a visually appealing format.

### 4. Display the data on the website

We'll need to use HTML, CSS, and JavaScript to create the user interface for the website. We can use a front-end framework like React or Angular to help us create a dynamic and responsive user interface that updates in real-time.

### 5. Implement real-time updates, or update the prices once a day at a certain time

Finally, we'll need to implement real-time updates or schedule daily updates at a certain time. We can use WebSocket technology to implement real-time updates or set up a scheduled task that retrieves the latest price data once a day at a certain time.

## Benefits of Implementation
Below is a list of benefits from designing and implementing a web app that dynamically displays historical market data such as buy and sell orders for the in-game economy:
   
1. **Easy access to market information:** Players will have easy access to real-time information about the prices of items in different regions, allowing them to make more informed decisions when buying or selling items.

2. **Improved trading experience:** With access to real-time market data, players can make more strategic trading decisions, leading to a more satisfying trading experience.

3. **Increased player engagement:** A dynamic website that displays market data can increase player engagement by encouraging them to participate in the in-game economy.

4. **Promotion of player-to-player trading:** By providing information on buy and sell orders, the website can encourage players to trade directly with each other, rather than relying solely on NPC vendors.

5. **Market transparency:** A dynamically updated market website can increase transparency in the economy by showing the current state of supply and demand for different items.

6. **Facilitates market analysis:** With access to detailed historical data, players and economists can conduct more detailed analysis of the in-game economy, leading to more advanced strategies and a deeper understanding of market dynamics.

7. **Supports server administration:** With a website that displays market data, server administrators can monitor the state of the economy and adjust game mechanics as needed to ensure a healthy and balanced economy.
   
Overall, a website that displays dynamic buy and sell information for the in-game market can greatly enhance the trading experience, encourage player engagement, increase market transparency, and support more advanced market analysis.

## Other Notes
There are a few features that could be added to the web application to enhance the user experience and provide more value to the community:
   
1. Search Functionality: A search function that allows users to quickly find the items they're interested in can be a huge time saver. The search function could include filters to narrow down results based on the region, item name, or other criteria.

3. Historical Data: Providing historical data on price trends for each item in the game can help traders make more informed decisions. This data can be displayed using charts or graphs and can be made available for download in spreadsheet format.

4. Discord Integration: Integrating with Discord can help users stay up-to-date on the latest market trends. Combining this with a notification system that alerts users when prices for certain items drop below a certain threshold or reach a certain price can make the application a valuable tool for traders. 

5. Mobile Optimization: With more and more people accessing the web on their mobile devices, it's important to ensure that the web application is optimized for mobile use. This can be achieved by using responsive design and ensuring that the website is compatible with a wide range of mobile devices.

Overall, these additional features can enhance the functionality and user experience of the web application and make it a valuable tool for traders in the AirshipCraft community.