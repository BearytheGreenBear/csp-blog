---
toc: false
comments: true
title: Login Page
type: hacks
courses: { compsci: {week: 19} }
---

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Login Page</title>
    <style>
        /* Add your SCSS styles here */
        .CONTAINER {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }
        .CARD {
            background-color: #e6bff2;
            padding: 20px;
            border-radius: 5px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        h3 {
            text-align: center;
        }
        .input {
            width: 100%;
            margin-bottom: 10px;
            padding: 8px;
            box-sizing: border-box;
        }
        .signInButton {
            width: 100%;
            padding: 10px;
            background-color: #007bff;
            color: #fff;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
        }
        .signInButton:hover {
            background-color: #410b52;
        }
    </style>
</head>
<body>

<div class="CONTAINER">
    <!-- This is the card that holds the login fields and button -->
    <div class="CARD">
        <h3>Login</h3> <!-- The title of the form -->
        <!-- The input field for the email, with a placeholder for user guidance -->
        <input id="uid" class="input" placeholder="User">
        <!-- The input field for the password -->
        <input id="password" class="input" placeholder="Password">
        <!-- The login button with an onclick attribute that calls the login_user() function -->
        <button class="signInButton" onclick="login_user()">Login</button>
        <!-- The sign-up button with an onclick attribute that calls the sign_up_user() function -->
        <button class="signUpButton" onclick="sign_up_user()">Sign Up</button>
    </div>
    <script>
        function userDbRequest() {
        // prepare HTML result container for new output
        const resultContainer = document.getElementById("result");
        // set options for cross origin header request
        const options = {
          method: 'GET', // *GET, POST, PUT, DELETE, etc.
          mode: 'cors', // no-cors, *cors, same-origin
          cache: 'default', // *default, no-cache, reload, force-cache, only-if-cached
          credentials: 'include', // include, *same-origin, omit
          headers: {
            'Content-Type': 'application/json',
          },
        };
        fetch("http://localhost:8086/api/users/authenticate", options)
          .then(response => {
            if (response.status !== 200) {
                const errorMsg = 'Database response error: ' + response.status;
                console.log(errorMsg);
                const tr = document.createElement("tr");
                const td = document.createElement("td");
                td.innerHTML = errorMsg;
                tr.appendChild(td);
                resultContainer.appendChild(tr);
                return;
            }
            // valid response will contain json data
            response.json().then(data => {
                console.log(data);
                for (const row of data) {
                  // tr and td build out for each row
                  const tr = document.createElement("tr");
                  const name = document.createElement("td");
                  const id = document.createElement("td");
                  const age = document.createElement("td");
                  // data is specific to the API
                  name.innerHTML = row.name;
                  id.innerHTML = row.email;
                  age.innerHTML = row.age;
                  // this build td's into tr
                  tr.appendChild(name);
                  tr.appendChild(id);
                  tr.appendChild(age);
                  // add HTML to container
                  resultContainer.appendChild(tr);
                }
            })
        })
        // catch fetch errors (ie ACCESS to server blocked)
        .catch(err => {
          console.error(err);
          const tr = document.createElement("tr");
          const td = document.createElement("td");
          td.innerHTML = err + ": " + url;
          tr.appendChild(td);
          resultContainer.appendChild(tr);
        });
      }
      // This function is called when the user clicks the login button.
    function login_user() {
        // STEP ONE: PREPARE THE REQUEST
        // Create a Headers object to set the type of content we're sending, which is JSON.
        var myHeaders = new Headers();
        myHeaders.append("Content-Type", "application/json");
        // Collect user input from the login form fields for email and password.
        var raw = JSON.stringify({
            "uid": document.getElementById("uid").value,
            "password": document.getElementById("password").value
            // Uncomment the following lines for quick testing with pre-defined credentials.
            //"email": "test@gmail.com",
            //"password": "123Lebron!"
        });
        // Print the collected data to the console for debugging purposes.
        console.log(raw);
        // Set up the options for the fetch request, including method, headers, and body.
        var requestOptions = {
            method: 'POST', // The method is POST because we're sending data.
            headers: myHeaders, // Attach the headers, including our content type.
            credentials: 'include', // Include credentials in case of cookies, etc.
            body: raw, // Attach the user input data as the request body.
            redirect: 'follow' // Follow any redirects automatically.
        };
        // STEP TWO: MAKE THE REQUEST TO THE SERVER
        // Send the request to the backend to authenticate the user.
        fetch("http://localhost:8086/api/users/authenticate", requestOptions)
        .then(response => {
            // If the response is not OK, handle the different kinds of login errors.
            if (!response.ok) {
                const errorMsg = 'Login error: ' + response.status;
                console.log(errorMsg);
                // Switch statement to handle different HTTP status codes.
                switch (response.status) {
                    case 401:
                        // Status 401 means unauthorized, indicating wrong credentials.
                        alert("Incorrect username or password");
                        break;
                    case 403:
                        // Status 403 means forbidden, indicating lack of permission.
                        alert("Access forbidden. You do not have permission to access this resource.");
                        break;
                    case 404:
                        // Status 404 means not found, indicating the user doesn't exist.
                        alert("User not found. Please check your credentials.");
                        break;
                    // More cases can be added for other HTTP status codes as needed.
                    default:
                        // A default case to handle any other errors.
                        alert("Login failed. Please try again later.");
                }
                // Reject the promise if there is an error.
                return Promise.reject('Login failed');
            }
            // If the response is OK, convert it from JSON to a text format.
            return response.text()
        })
        .then(result => {
            // If the login is successful, print the result to the console.
            console.log(result);
        })
        .catch(error => {
            // If there is a problem during the fetch or during processing, log the error.
            console.error('Error during login:', error);
        });
    }
    // Sign Up Code
    function sign_up_user() {
        // STEP ONE: PREPARE THE REQUEST
        // Create a Headers object to set the type of content we're sending, which is JSON.
        var myHeaders = new Headers();
        myHeaders.append("Content-Type", "application/json");
        // Collect user input from the login form fields for email and password.
        var raw = JSON.stringify({
            "uid": document.getElementById("uid").value,
            "password": document.getElementById("password").value
            // Uncomment the following lines for quick testing with pre-defined credentials.
            //"email": "test@gmail.com",
            //"password": "123Lebron!"
        });
        // Print the collected data to the console for debugging purposes.
        console.log(raw);
        // Set up the options for the fetch request, including method, headers, and body.
        var requestOptions = {
            method: "POST",
            headers: myHeaders, // Attach the headers, including our content type.
            credentials: 'include', // Include credentials in case of cookies, etc.
            body: raw,
            redirect: 'follow' // Follow any redirects automatically.
        };
        // Collect user input
        // Posting in backend only works if user input is sent as query parameters
        fetch("http://localhost:8086/api/users/create", requestOptions)
        .then(response => {
            if (!response.ok) {
                return response.text().then(errorMsg => {
                    alert('Error: ' + errorMsg);
                });
            }
            // Success!!!
            alert("Signup Complete");
            // Redirect to Database location
            location.reload();
        })
        .catch(error => {
            alert('An unexpected error occurred: ' + error.message);
        });
    }
</script>

    