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

1. MongoDB 

I chose MongoDB because it works perfectly with the JSON Horoscope data. As a NoSQL document database, MongoDB stores data in a format that's nearly identical to the JSON payload that I store for Horoscope data. This direct mapping simplifies our code, as i don't need to translate the data into the rigid tables required by a traditional SQL database. This flexibility is a major advantage, we can easily adapt and store them without performing complex database migrations.

1. Typescript 

TypeScript was chosen for this horoscope API project to build a more robust, maintainable, and error-free backend. Its primary benefit, **static typing**, is crucial for managing the specific data structures involved.

For instance, by defining an `IUser` interface, we guarantee that every user object consistently has properties like `name: string`, `birthdate: Date`, and `zodiacSign: string`. This prevents common runtime errors that could occur if a property were misspelled or had the wrong data type, especially within the authentication middleware where `req.user` is used.

This use of types also makes the code **self-documenting** and easier to work with. Any developer can look at the `IUser` and immediately understand the exact shape of an offer object, which improves collaboration and long-term maintainability. 

## Short Note on improvement of the assignment, if time persists

A crucial improvement would be building a **comprehensive testing suite** to guarantee code quality. This involves writing **unit tests** to verify isolated logic. I would also add **integration tests** to confirm that the API endpoints properly communicate with the MongoDB database. Finally, **end-to-end tests** would simulate a full user journey, ensuring the entire system works together seamlessly and that future code changes don't introduce regressions.

I would establish a professional development workflow with a **CI/CD (Continuous Integration/Continuous Deployment) pipeline**. Using tools like Jenkins, this would automatically run your tests and deploy the application when code is pushed and job is triggered. Alongside this, i would implement **structured logging** with a library. Instead of a custom logger file, this creates rich JSON logs that are far easier to search and analyze in a production environment, making debugging significantly faster and more efficient.




