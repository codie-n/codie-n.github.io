---
layout: essay
type: essay
title: "Learning How to Ask Questions"
# All dates must be YYYY-MM-DD format!
date: 2025-01-30
published: true
labels:
  - Software Engineering
  - Learning
  - StackOverflow
---

<img width="300px" class="rounded float-start pe-4" src="../img/thinking.jpg">

## Intro

Everyone's always been given the advice to "not be afraid to ask questions" because asking questions is an essential part of learning. And learning, is an essential part of life. But many get caught into the trap of asking unproductive questions or lack the skill to ask questions in a courteous manner. It's crucial to have the ability to ask "smart" questions in order to streamline your learning process and make things easier for everybody involved. Making things frustrating will only burn you out.

## What makes a question "smart"?

Here is a question someone posted that is written well and got large amounts of interaction.

Q: [How to write custom validation rule in controller Laravel?](https://stackoverflow.com/questions/41283702/how-to-write-custom-validation-rule-in-controller-laravel).

```
I have default validation rule in controller Laravel:

$validator = Validator::make($request->all(), [
    'email' => 'required|email',
    'phone' => 'required|numeric',
    'code' => 'required|string|min:3|max:4',
    'timezone' => 'required|numeric',
    'country' => 'required|integer',
    'agreement' => 'accepted'
]);
I tried this, but dont know how to transfer some parameters inside function:

public function boot(){
    Validator::extend('phone_unique', function($attribute, $value, $parameters) {
        return substr($value, 0, 3) == '+44';
    });
}
How can I extent this validation by my own rule? For example I need to validate concatination of inputs:

$phone = $request->code.' '.$request->phone;
After check if $phone are exists in database

I want to use this method:

$validator->sometimes('phone', 'required|alpha_dash|max:25', function($input) {
    if ((Auth::user()->phone == $input->phone)) {
        return false;
    } else {
        $t = User::where("phone", $input->phone)->get();
        return ($t->count() > 0) ? false : false; 
    }
});
It does not work under all conditions (True, False) inside.

I added new validation nickname_unique:

$validator = Validator::make($request->all(), [
    'email' => 'required|email',
    'code' => 'required|string|min:3|max:4',
    'phone' => 'required|phone_unique',
    'timezone' => 'required|numeric',
    'country' => 'required|integer',
    'nickname' => 'required|alpha_dash|max:25',
    'agreement' => 'accepted'
], [
    'phone_unique' => 'Phone already exists!',
    'nickname_unique' => 'Nickname is busy!',
]);
It does not work, even not call validation rule below previos:

Validator::extend('nickname_unique', function ($attribute, $value, $parameters, $validator) {
    dd("Here");
});
```

This question is very clear on what the exact problem the user is facing. They provide the necessary amount of code while explaining how each part is relevant to the issue. The whole post is almost the user detailing and going through their entire process so far of tackling this issue. All of these points along with the use of proper grammar leads to a very clean and pleasant question. There's no pointless confusion or frustration being passed on.

## Now what would be "not smart"?

Here is a question someone posted that has some glaring problems. This post at the time of writing, had very little views and no answers at all.

Q: [no token found or no token provided](https://stackoverflow.com/questions/79401833/no-token-found-or-no-token-provided).

```
the problem i am facing is that everything works fine locally but when it comes to production it gives this error ...

...front end code

import React, { useState } from 'react';
import { motion } from 'framer-motion';
import { axiosInstance } from '../../lib/axio';
import { useNavigate } from 'react-router-dom';
import { useAuth } from '../../context/AuthContext';

const AuthPage = () => {
  const navigate = useNavigate();
  const { login } = useAuth();
  const [isLogin, setIsLogin] = useState(true);
  const [name, setName] = useState('');
  const [username, setUsername] = useState('');
  const [password, setPassword] = useState('');


  const handleOnSubmit = async (e) => {
    e.preventDefault();
    if (!username || !password) {
      alert('All fields are required.');
      return;
    }
    if (isLogin) {
      try {
        // Simulate an API request for login
        // Replace this with your actual login logic, such as an API call to check credentials
        const response = await axiosInstance.post('/user/login', { username, password });
        console.log("login response:", response);
        if (response.status === 200) {
          login();
          alert("Logged in successfully!");
          navigate("/blogs/create");
          window.scrollTo(0, 0);
        } else {
          alert(response.data.message);
          console.log("response error : ", response);
        }
      } catch (error) {
        alert(error.response?.data?.message || 'Unable to log in. Check your credentials.');
        console.log(error)
      }
    }
    else {
      try {
        const response = await axiosInstance.post("/user/signup", {
          name,
          username,
          password,
        });
        console.log("signup response:", response);
        if (response.status === 201) {
          login();
          alert("Signed up successfully!");

          navigate('/blogs/create')
          window.scrollTo(0, 0);
        } else {
          alert(response.data.message || 'Something went wrong. Please try again.');
        }
      } catch (error) {
        alert(error.response?.data?.message || 'Unable to sign up. Please try again.');
      }
    }
  }

  const toggleAuthMode = () => {
    setIsLogin((prevMode) => !prevMode);
  };

  return (
    <div className="flex justify-center items-center h-screen bg-gradient-to-r from-blue-500 via-purple-500 to-pink-500">
      <motion.div
        className="w-full max-w-md p-6"
        initial={{ opacity: 0, y: 50 }}
        animate={{ opacity: 1, y: 0 }}
        transition={{ duration: 0.6 }}
      >
        <div className="bg-white shadow-2xl rounded-2xl">
          <div className="p-8">
            <h2 className="text-2xl font-bold text-center mb-6 text-gray-800">
              {isLogin ? 'Login' : 'Sign Up'}
            </h2>

            <form onSubmit={handleOnSubmit}>
              {!isLogin && (
                <div className="mb-4">
                  <label htmlFor="name" className="block text-gray-700 text-sm font-bold mb-2">
                    Name
                  </label>
                  <input
                    type="text"
                    id="name"
                    value={name}
                    onChange={(e) => { setName(e.target.value) }}
                    className="w-full px-3 py-2 text-gray-700 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-400"
                    placeholder="Enter your name"
                  />
                </div>
              )}

              <div className="mb-4">
                <label htmlFor="username" className="block text-gray-700 text-sm font-bold mb-2">
                  username
                </label>
                <input
                  type="username"
                  id="username"
                  value={username}
                  onChange={(e) => { setUsername(e.target.value) }}
                  className="w-full px-3 py-2 text-gray-700 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-400"
                  placeholder="Enter your username"
                />
              </div>

              <div className="mb-4">
                <label htmlFor="password" className="block text-gray-700 text-sm font-bold mb-2">
                  Password
                </label>
                <input
                  type="password"
                  id="password"
                  value={password}
                  onChange={(e) => { setPassword(e.target.value) }}
                  className="w-full px-3 py-2 text-gray-700 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-400"
                  placeholder="Enter your password"
                />
              </div>

              <button className="w-full py-2 text-white bg-blue-600 hover:bg-blue-700 rounded-lg">
                {isLogin ? 'Login' : 'Sign Up'}
              </button>
            </form>

            <p className="text-center text-sm text-gray-600 mt-4">
              {isLogin ? "Don't have an account?" : 'Already have an account?'}{' '}
              <button
                onClick={toggleAuthMode}
                className="text-blue-600 hover:underline focus:outline-none"
              >
                {isLogin ? 'Sign Up' : 'Login'}
              </button>
            </p>
          </div>
        </div>
      </motion.div>
    </div>
  );
};

export default AuthPage;


authMiddleware

const jwt = require("jsonwebtoken");
const User = require("../models/User");

const protectRoute = async (req, res, next) => {
    try {
        const token = req.cookies["jwt-booklore-blog"];
        console.log("token:", token);
        if (!token) {
            return res.status(401).json({ message: "Unauthorized - No Token Provided", success: false });
        }

        const decoded = jwt.verify(token, process.env.JWT_SECRET);
        console.log("decoded:", decoded);
        if (!decoded) {
            return res.status(401).json({ message: "Unauthorized - Invalid Token", success: false });
        }

        const user = await User.findById(decoded.userId).select("-password");

        if (!user) {
            return res.status(401).json({ message: "User not found", success: false });
        }

        req.user = user;

        next();
    } catch (error) {
        console.log("Error in protectRoute middleware:", error.message);
        if (error.name === 'TokenExpiredError') {
            return res.status(401).json({ message: 'Token expired', success: false });
        } else if (error.name === 'JsonWebTokenError') {
            return res.status(401).json({ message: 'Invalid token', success: false });
        } else {
            return res.status(500).json({ message: "Internal server error", success: false });
        }
    }
};

module.exports = { protectRoute };
authController

const bcrypt = require("bcryptjs");
const jwt = require("jsonwebtoken");
const User = require("../models/User");


const signup = async (req, res) => {
    try {
        const { name, username, password } = req.body;

        if (!name || !username || !password) {
            return res.status(400).json({ message: "All fields are required" });
        }

        const existingUsername = await User.findOne({ username });
        if (existingUsername) {
            return res.status(400).json({ message: "Username already exists" });
        }

        if (password.length < 6) {
            return res.status(400).json({ message: "Password must be at least 6 characters" });
        }

        const salt = await bcrypt.genSalt(10);
        const hashedPassword = await bcrypt.hash(password, salt);

        const user = new User({
            name,
            username,
            password: hashedPassword,
        });

        await user.save();

        const token = jwt.sign({ userId: user._id }, process.env.JWT_SECRET, { expiresIn: "30d" });

        res.cookie("jwt-booklore-blog", token, {
            httpOnly: true, // prevent XSS attack
            maxAge: 30 * 24 * 60 * 60 * 1000,
            sameSite: "strict", // prevent CSRF attacks,
            secure: process.env.NODE_ENV === "production", // prevents man-in-the-middle attacks
        });

        res.status(201).json({ message: "User registered successfully", success: true, reDirectUrl: '/' });

        // const profileUrl = process.env.CLIENT_URL + "/profile/" + user.username;

        // try {
        //  await sendWelcomeEmail(user.email, user.name, profileUrl);
        // } catch (emailError) {
        //  console.error("Error sending welcome Email", emailError);
        // }
    } catch (error) {
        console.log("Error in signup: ", error);
        res.status(500).json({ message: "Internal server error", reDirectUrl: '/', success: false });
    }
};



const login = async (req, res) => {
    try {
        const { username, password } = req.body;

        // Check if user exists
        const user = await User.findOne({ username });
        if (!user) {
            return res.status(400).json({ message: "Invalid username", success: false });
        }

        // Check password
        const isMatch = await bcrypt.compare(password, user.password);
        if (!isMatch) {
            return res.status(400).json({ message: "Invalid Password", success: false });
        }

        // Create and send token
        const token = jwt.sign({ userId: user._id }, process.env.JWT_SECRET, { expiresIn: "30d" });
        await res.cookie("jwt-booklore-blog", token, {
            httpOnly: true,
            maxAge: 30 * 24 * 60 * 60 * 1000,
            sameSite: "strict",
            secure: process.env.NODE_ENV === "production",
        });
        console.log("Cookie set:", req.cookies["jwt-booklore-blog"])

        res.json({ message: "Logged in successfully", success: true, reDirectUrl: '/' });
    } catch (error) {
        console.error("Error in login controller:", error);
        res.status(500).json({ message: "Server error", success: false });
    }
};

const logout = (req, res) => {
    res.clearCookie("jwt-booklore-blog");
    res.json({ message: "Logged out successfully", success: true, reDirectUrl: '/' });
};

const getCurrentUser = async (req, res) => {
    try {
        console.log("Current User: ", req.user);
        res.json(req.user);
    } catch (error) {
        console.error("Error in getCurrentUser controller:", error);
        res.status(500).json({ message: "Server error", success: false });
    }
};

module.exports = { signup, login, logout, getCurrentUser };
..everything works locally .. but after production this error is coming .... if tried the secure:false....adding domain in cookie .. i tried everything i can .. still stuck

... expecting to work everything on production
```

First of all, the title is not even asking a proper question and is just throwing out some statements without any context. Then there's the lack of any real explanation of the problem. This user very vaguely lists their issue and just leaves some code without any description of it so that presumably, someone will read all of it in order figure everything out for them. The way in which this entire post is written just exudes a lack of care and passes on a lot of frustration. This person needed to explain their problem further as well as detail how all of the code they gave is relevant. That along with better grammar and being more polite would not only help someone else figure out what's going on but give them more motivation to help with it.

## Conclusion

At the end of the day, even if it's just online on a forum, your question is reaching a real person. Someone who is taking the time out of their day to read through your question. To get useful, productive answers your questions need to be clear and detailed so that people can actually answer them without having to pull out their hairs. The impression you make is imporant as well, if you come across as someone who's done their research and appreciates the help, people will respond in kind.
