Document Link : https://suryanshsahu1801.notion.site/Pie-Pay-Assignment-Document-2359e56e264a805599c2ef67b95592ce

To complete the setup and run the application, follow these steps carefully:

1. **Clone the Repository:** Download or clone the project : https://github.com/suryansh1801/pie-pay code onto your local machine by using the following command.
    
    ```jsx
    git clone https://github.com/suryansh1801/pie-pay.git
    ```
    
2. **Change directory to pie-pay :** Run the following command 
    
    ```jsx
    cd pie-pay
    ```
    
3. **Install Dependencies:** Run the following command in the project root to install all required Node.js packages:
    
    ```jsx
    npm install
    ```
    
4. **Set Up Environment Variables** file in the project root if needed (for database URLs, secrets, etc.). **`Create a .env in the project root with the following variables.`**
    
    ```jsx
    MONGO_URI=mongodb://localhost:27017/pie-pay
    PORT=3000
    ```
    
5. **Start MongoDB:** with the correct remote connection string. Make sure you have MongoDB installed and running locally or update your .env with the correct remote connection string.
6. **Run the Server:**Start the Express server with:
    
    ```jsx
    npm run dev
    ```
    
7. **Test the Endpoints:** and  endpoints.
    
    Use Postman or similar tools to test the following curls : 
    
    1. POST  :  /offer
    
    ```jsx
    curl --location 'http://localhost:3000/api/v1/offer' \
    --header 'Content-Type: application/json' \
    --data '[
        {
            "adjustment_type": "CASHBACK_ON_CARD",
            "adjustment_id": "FPO250619134128USHPF",
            "summary": "5% cashback on Flipkart Axis Bank Credit Card upto ₹4,000 per statement quarter",
            "contributors": {
                "payment_instrument": [
                    "CREDIT"
                ],
                "banks": [
                    "FLIPKARTAXISBANK"
                ],
                "emi_months": [
                    "0"
                ],
                "card_networks": []
            },
            "display_tags": [
                "PAYMENT_OPTIONS"
            ],
            "image": "https://img1a.flixcart.com/www/linchpin/fk-cp-pay/axis-78501b36.svg"
        }
    ]'
    ```
    
    2. GET : /highest-discount

    ```jsx
    curl --location 'http://localhost:3000/api/v1/highest-discount?amountToPay=50000&paymentInstrument=CREDIT&bankName=FLIPKARTAXISBANK'
    ```

## Design Decisions

1. Express.js framework 

I decided to go with Express because Express.js is a great choice because it's incredibly simple and flexible. It's a **minimal** framework, giving you only the essential tools to build a web application and then getting out of your way. Because it's **unopinionated**, it doesn't force you to organize your code in any specific way, giving you complete freedom. This makes it very easy to learn and perfect for building small applications, simple APIs, or quick prototypes where you want to get started immediately without a lot of setup.

2. MongoDB 

I chose MongoDB because it works perfectly with the JSON data from the Flipkart Offers API. As a NoSQL document database, MongoDB stores data in a format that's nearly identical to the JSON payload i receive. This direct mapping simplifies our code, as i don't need to translate the data into the rigid tables required by a traditional SQL database. This flexibility is a major advantage; if Flipkart ever adds new fields to their offers, we can easily adapt and store them without performing complex database migrations.

The schema design focuses on performance and data integrity. I **embedded** the `contributors` information (like banks and payment types) directly within each offer document because this data always belongs to a specific offer. This design is highly efficient, as it allows me to fetch all the information for an offer in a single, fast database query. To make our `GET /highest-discount` endpoint fast, i added **indexes** to the `banks` and `payment_instrument` fields. Finally, making the `adjustment_id` **unique** was a simple and powerful way to let the database automatically handle the requirement to reject duplicate offers.

3. Typescript 

I chose TypeScript over plain JavaScript primarily for **type safety**, which helps us catch common bugs before the code ever runs. By defining the expected structure of the data with interfaces like `IOffer`, TypeScript acts as a safety net. For example, it guarantees that the `amountToPay` parameter is always a number and that the `contributors` object contains the correct fields, like an array of `banks`. This process catches potential errors and typos during development, not when a user is interacting with the API, leading to a much more reliable and robust application.

This use of types also makes the code **self-documenting** and easier to work with. Any developer can look at the `IOffer`interface and immediately understand the exact shape of an offer object, which improves collaboration and long-term maintainability. 

## Short Note on handling 1000 GET read request per second

1. To handle 1,000 requests per second, the first step is **horizontal scaling**. Since the endpoint is stateless, we can run multiple instances of our Node.js application across several CPU cores and servers using a process manager like PM2 or containers like Docker. A **load balancer** is then placed in front of these instances to intelligently distribute the incoming traffic. This approach bypasses the single-threaded limitation of Node.js and ensures no single server is overwhelmed, allowing us to serve a high volume of concurrent users.
2. The most critical optimization is implementing a **caching layer** with an in-memory datastore like **Redis**. Since offer data doesn't change every second, querying the database for every request is highly inefficient. With a cache, i first check if the requested offer data for a specific bank and payment instrument exists in Redis. If i don’t found the request offer data in Redis, then i query the database and fetch the result, i store that result in Redis for future use case. 
3. Finally, the database itself must be robust. The **MongoDB replica set** needs to be properly scaled to handle concurrent connections from all application instances, especially during cache misses and write operations from other endpoints. Continuous performance monitoring across the entire system is essential. I would track response times, CPU usage, and database query performance to proactively identify and resolve bottlenecks, ensuring the entire application remains fast and reliable under heavy load.

## Short Note on improvement of the assignment, if time persists

A crucial improvement would be building a **comprehensive testing suite** to guarantee code quality. This involves writing **unit tests** to verify isolated logic, like ensuring the `discountCalculator` function handles all edge cases correctly. I would also add **integration tests** to confirm that the API endpoints properly communicate with the MongoDB database. Finally, **end-to-end tests** would simulate a full user journey, ensuring the entire system works together seamlessly and that future code changes don't introduce regressions.

I would establish a professional development workflow with a **CI/CD (Continuous Integration/Continuous Deployment) pipeline**. Using tools like Jenkins, this would automatically run your tests and deploy the application when code is pushed and job is triggered. Alongside this, i would implement **structured logging** with a library. Instead of a custom logger file, this creates rich JSON logs that are far easier to search and analyze in a production environment, making debugging significantly faster and more efficient.


