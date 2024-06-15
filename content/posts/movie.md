+++
title = 'Movie Booking System with expressJS'
date = 2024-06-15T09:02:28+03:00

+++

### Building a Movie Reservation System with Node.js, Express.js, and MongoDB

**Introduction**

In this post, I will guide you through the process of building a simple movie reservation system using Node.js, Express.js, and MongoDB. This project will help you understand how to create a RESTful API with user authentication and perform CRUD operations on MongoDB.

**Project Overview**

The movie reservation system includes the following features:

* **Movie Listing Endpoint** : Retrieve a list of available movies.
* **Check Availability Endpoint** : Check the availability of a specific time slot for a movie.
* **Reserve Time Slot Endpoint** : Reserve a time slot for a movie.
* **User Registration and Login** : Register new users and log in existing users to obtain access tokens.
* **Token Authentication** : Secure the movie endpoints with JWT-based authentication.

**Tech Stack**

The project is built using:

* **Node.js** : JavaScript runtime for building the backend.
* **Express.js** : Web framework for Node.js to manage routes and middleware.
* **MongoDB** : NoSQL database to store movie and reservation data.

**Step-by-Step Guide**

1. **Setup the Project**

First, create a new directory for your project and navigate into it:

* mkdir Movie-Reservation-System
* cd Movie-Reservation-System

Initialize a new Node.js project:

* npm init -y

2. **Install Dependencies**

Install the necessary dependencies:

* npm install express mongoose dotenv bcryptjs jsonwebtoken

3. **Project Structure**

Create the following folder structure for your project:

* Movie-Reservation-System
  * models
    * user.model.js
    * movie.model.js
  * routes
    * auth.routes.js
    * movie.routes.js
  * controllers
    * auth.controller.js
    * movie.controller.js
  * middleware
    * auth.js
  * .env
  * server.js

4. **Environment Variables**

Create a `.env` file in the root directory and add your MongoDB connection string and secret key for JWT:

* MONGO_URI=your_mongodb_connection_string
* ACCESS_TOKEN_SECRET=your_secret_key

5. **Creating Models**

Create a `user.model.js` file in the `models` folder:

* Define user schema with fields for username and password.
* Hash the user's password before saving.

Create a `movie.model.js` file in the `models` folder:

* Define movie schema with fields for title and time slots.
* Define time slot schema with fields for slot, capacity, and booked.

6. **Creating Controllers**

Create an `auth.controller.js` file in the `controllers` folder:

* Define `registerUser` function to register new users.
* Define `loginUser` function to log in users and generate JWT tokens.

Create a `movie.controller.js` file in the `controllers` folder:

* Define `getMovies` function to retrieve a list of movies.
* Define `checkAvailability` function to check the availability of a specific time slot.
* Define `reserveTimeSlot` function to reserve a time slot for a movie.

7. **Creating Middleware**

Create an `auth.js` file in the `middleware` folder:

* Define `authenticateToken` function to verify JWT tokens and protect routes.

8. **Creating Routes**

Create an `auth.routes.js` file in the `routes` folder:

* Define routes for user registration and login.

Create a `movie.routes.js` file in the `routes` folder:

* Define routes for movie listing, checking availability, and reserving time slots.
* Protect routes with `authenticateToken` middleware.

9. **Server Setup**

Create a `server.js` file in the root directory:

* Import required modules and initialize Express app.
* Connect to MongoDB using Mongoose.
* Define routes for user and movie APIs.
* Start the server on the specified port.

**Conclusion**

By following these steps, you have created a movie reservation system with Node.js, Express.js, and MongoDB. This project demonstrates how to build a RESTful API, implement user authentication with JWT, and perform CRUD operations on a MongoDB database. Feel free to explore the code, enhance it with additional features, and fork the repo to enhanced it!
