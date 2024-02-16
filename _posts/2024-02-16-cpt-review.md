---
toc: true
comments: true
title: CPT Review
type: tangibles
courses: { compsci: {week: 22} }
---

# Main Features to Check out
```
🔴🟡🟢                                            features.txt
______________________________________________________________________________________________________________________
1️⃣ Login Page                                    
2️⃣ Post feature                                  
3️⃣ Like feature                                  
4️⃣ Sign up feature                               
5️⃣ Delete feature                                
```
# Backend
Team Member | Tangibles
-- | --
Kyle | [Project Mapping](https://github.com/SAAK-APCSP/SAAK_repo/blob/main/images/csp_checkpoint_ideation_1.png), [UML Class Interactions](https://github.com/SAAK-APCSP/SAAK_repo/blob/main/images/UML%20Class%20diagram.png)
Shuban | [Posting Concept Design](https://github.com/SAAK-APCSP/SAAK_repo/blob/main/images/image.png)
Akhil | [Project Review Ticket](https://saak-apcsp.github.io/SAAK_repo//2023/12/19/CPT-brainstorm.html)
Aashray | [Login Page Concept Design](https://saak-apcsp.github.io/SAAK_repo/images/image_720.png)



`user.py`
```py
import json, jwt
from flask import Blueprint, request, jsonify, current_app, Response
from flask_restful import Api, Resource # used for REST API building
from datetime import datetime
from auth_middleware import token_required

from model.users import User

user_api = Blueprint('user_api', __name__,
                   url_prefix='/api/users')


# API docs https://flask-restful.readthedocs.io/en/latest/api.html
api = Api(user_api)

class UserAPI:        
    class _CRUD(Resource):  # User API operation for Create, Read.  THe Update, Delete methods need to be implemeented
        @token_required
        def post(self, current_user): # Create method
            ''' Read data for json body '''
            body = request.get_json()
            
            ''' Avoid garbage in, error checking '''
            # validate name
            name = body.get('name')
            if name is None or len(name) < 2:
                return {'message': f'Name is missing, or is less than 2 characters'}, 400
            # validate uid
            uid = body.get('uid')
            if uid is None or len(uid) < 2:
                return {'message': f'User ID is missing, or is less than 2 characters'}, 400
            # look for password and dob
            password = body.get('password')
            dob = body.get('dob')
            ''' #1: Key code block, setup USER OBJECT '''
            uo = User(name=name, 
                      uid=uid)
            
            ''' Additional garbage error checking '''
            # set password if provided
            if password is not None:
                uo.set_password(password)
            # convert to date type
            if dob is not None:
                try:
                    uo.dob = datetime.strptime(dob, '%Y-%m-%d').date()
                except:
                    return {'message': f'Date of birth format error {dob}, must be mm-dd-yyyy'}, 400
            
            ''' #2: Key Code block to add user to database '''
            # create user in database
            user = uo.create()
            # success returns json of user
            if user:
                return jsonify(user.read())
            # failure returns error
            return {'message': f'Processed {name}, either a format error or User ID {uid} is duplicate'}, 400
        @token_required
        def get(self, current_user): # Read Method
            users = User.query.all()    # read/extract all users from database
            json_ready = [user.read() for user in users]  # prepare output in json
            return jsonify(json_ready)  # jsonify creates Flask response object, more specific to APIs than json.dumps

        
        def put(self, user_id):
            '''Update a user'''
            user = User.query.get(user_id)
            if not user:
                return {'message': 'User not found'}, 404
            body = request.get_json()
            user.name = body.get('name', user.name)
            user.uid = body.get('uid', user.uid)
            db.session.commit()
            return user.read(), 200

        def delete(self, user_id):
            '''Delete a user'''
            user = User.query.get(user_id)
            if not user:
                return {'message': 'User not found'}, 404
            db.session.delete(user)
            db.session.commit()
            return {'message': 'User deleted'}, 200
    
    class _Security(Resource):
        def post(self):
            try:
                body = request.get_json()
                if not body:
                    return {
                        "message": "Please provide user details",
                        "data": None,
                        "error": "Bad request"
                    }, 400
                ''' Get Data '''
                uid = body.get('uid')
                if uid is None:
                    return {'message': f'User ID is missing'}, 400
                password = body.get('password')
                
                ''' Find user '''
                user = User.query.filter_by(_uid=uid).first()
                if user is None or not user.is_password(password):
                    return {'message': f"Invalid user id or password"}, 400
                if user:
                    try:
                        token_payload = {
                            "_uid": user._uid,
                            "role": user.role 
                        }
                        token = jwt.encode(
                            token_payload,
                            current_app.config["SECRET_KEY"],
                            algorithm="HS256"
                        )
                        resp = Response("Authentication for %s successful" % (user._uid))
                        resp.set_cookie("jwt", token,
                                max_age=3600,
                                secure=True,
                                httponly=True,
                                path='/',
                                samesite='None'  # This is the key part for cross-site requests

                                # domain="frontend.com"
                                )
                        return resp
                    except Exception as e:
                        return {
                            "error": "Something went wrong",
                            "message": str(e)
                        }, 500
                return {
                    "message": "Error fetching auth token!",
                    "data": None,
                    "error": "Unauthorized"
                }, 404
            except Exception as e:
                return {
                        "message": "Something went wrong!",
                        "error": str(e),
                        "data": None
                }, 500
    class Login(Resource):
        def post(self):
            data = request.get_json()

            uid = data.get('uid')
            password = data.get('password')

            if not uid or not password:
                response = {'message': 'Invalid creds'}
                return make_response(jsonify(response), 401)

            user = User.query.filter_by(_uid=uid).first()

            if user and user.is_password(password):
         
                response = {
                    'message': 'Logged in successfully',
                    'user': {
                        'name': user.name,  
                        'id': user.id
                    }
                }
                return make_response(jsonify(response), 200)

            response = {'message': 'Invalid id or pass'}
            return make_response(jsonify(response), 401)
    class _Create(Resource):
        def post(self):
            body = request.get_json()
            # Fetch data from the form
            name = body.get('name')
            uid = body.get('uid')
            password = body.get('password')
            role = body.get('role')
            if uid is not None:
                new_user = User(name=name, uid=uid, password=password, role=role)
            user = new_user.create()
            if user:
                return user.read()
            return {'message': f'Processed {name}, either a format error or User ID {uid} is duplicate'}, 400
    class _Delete(Resource):
        def post(self): # Delete Method
            body = request.get_json()
            uid = body.get('uid')
            password = body.get('password')
            user = User.query.filter_by(_uid=uid).first()
            if user is None or not user.is_password(password):
                return {'message': f'User {uid} not found'}, 404
            json = user.read()
            if user:
                try:
                    user.delete() 
                except Exception as e:
                    return {
                        "error": "Something went wrong!",
                        "message": str(e)
                    }, 500
            # 204 is the status code for delete with no json response
            return f"Deleted user: {json}", 204 # use 200 to test with Postman
            
    # building RESTapi endpoint
    api.add_resource(_CRUD, '/')
    api.add_resource(_Security, '/authenticate')
    api.add_resource(Login, '/login')
    api.add_resource(_Create, '/create')
    api.add_resource(_Delete, '/delete')
    ```
# Front

`dashboard.md`
```markdown
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Social Media Chat</title>
    <style>
        @keyframes strobe {
            0%, 20%, 50%, 80%, 100% {
                border-color: #FF0000; /* Red */
            }
            40% {
                border-color: #FF7F00; /* Orange */
            }
            60% {
                border-color: #FFFF00; /* Yellow */
            }
            80% {
                border-color: #00FF00; /* Green */
            }
        }
        body {
            margin: 0;
            font-family: Arial, sans-serif;
            background-color: #171515;
            color: #39FF14;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }
        .login-container {
            border-radius: 15px;
            padding: 20px;
            border: 5px solid transparent;
            background-clip: padding-box;
            background-color: #171515;
            color: #39FF14;
            animation: strobe 2s infinite; /* Apply strobe light effect to the border */
            width: 300px; /* Adjusted width */
        }
        input[type=text], input[type=password] {
            width: 100%;
            padding: 12px 20px;
            margin: 8px 0;
            display: inline-block;
            border: 1px solid #39FF14;
            box-sizing: border-box;
            background-color: #171515;
            color: #39FF14;
        }
        button {
            background-color: #39FF14;
            color: #171515;
            padding: 14px 20px;
            margin: 8px 0;
            border: none;
            cursor: pointer;
            width: 100%; /* Adjusted width */
        }
        button:hover {
            opacity: 0.8;
        }
        span.psw {
            display: block;
            text-align: center;
            margin-top: 16px;
            color: #39FF14;
        }
        @media screen and (max-width: 300px) {
            span.psw {
                display: block;
                float: none;
            }
        }
        .container {
            display: flex;
            justify-content: space-between;
            max-width: 800px;
            margin: auto;
            padding: 20px;
        }
        .input-container,
        .posts-container {
            flex: 1;
            padding: 10px;
        }
        .post-container {
            position: relative;
            border: 1px solid #ccc;
            margin-bottom: 10px;
            padding: 10px;
            background-color: #fff;
        }
        .post-actions {
            position: absolute;
            top: 10px;
            right: 10px;
        }
        .post-actions button {
            margin-right: 5px;
            cursor: pointer;
            background-color: transparent;
            border: none;
        }
        .input-container textarea {
            width: 100%;
            margin-bottom: 10px;
            padding: 10px;
            border: 1px solid #ccc;
            resize: vertical;
        }
        .input-container button {
            display: block;
            width: 100%;
            padding: 10px;
            background-color: $primary-color;
            color: #fff;
            border: none;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }
        .input-container button:hover {
            background-color: darken($primary-color, 10%);
        }
        .latest-posts {
            margin-top: 20px;
            border-top: 1px solid #ccc;
            padding-top: 20px;
        }
        .latestPost {
            border: 1px solid #ccc;
            padding: 10px;
            margin-bottom: 5px;
        }
        .post-container {
        position: relative;
        border: 1px solid #ccc;
        margin-bottom: 10px;
        padding: 10px;
        background-color: #000; /* Black background color */
        color: #fff; /* Text color */
    }
    .post-content {
        margin: 0; /* Remove default margin for <p> */
    }
    #postsWrapper {
            max-height: 300px; /* Adjust the maximum height as needed */
            overflow-y: auto; /* Enable vertical scrolling */
        }
    </style>
</head>
<body onload="fetchPosts();">
    <div class="container">
        <div class="input-container">
        <form action="javascript:createPost()" id="postButton">
            <h2>Post Your Message</h2>
            <input type="text" id="uid" placeholder="Enter UID...">
            <textarea id="message" placeholder="Type your post..."></textarea>
            <button id="postButton">Post</button>
        </form>
        </div>
        <div class="posts-container" id="postsWrapper">
            <h2>Posts</h2>
            <input type="text" id="searchInput" oninput="searchPosts()" placeholder="Search posts...">
            <div id="posts"></div>
        </div>
    </div>
    <div id="latestPosts" class="latest-posts"></div>
   <script>
    function createPost() {
        var myHeaders = new Headers();
        myHeaders.append("Content-Type", "application/json");
        const uid = document.getElementById("uid").value;
        const message = document.getElementById("message").value;
        const body = {
            uid: uid,
            message: message,
            likes: 0
        };
        const authOptions = {
            method: 'POST',
            cache: 'no-cache',
            headers: myHeaders,
            body: JSON.stringify(body),
            credentials: 'include'
        };
        fetch('http://127.0.0.1:8086/api/messages/send', authOptions)
            .then(response => {
                if (!response.ok) {
                    console.error('Failed to create post:', response.status);
                    return null;
                }
                const contentType = response.headers.get('Content-Type');
                if (contentType && contentType.includes('application/json')) {
                    return response.json();
                } else {
                    return response.text();
                }
            })
            .then(data => {
                if (data !== null) {
                    console.log('Response:', data);
                    // Update the posts container with the new post
                    updatePostsContainer(uid, message, 0);
                }
            })
            .catch(error => {
                console.error('Error:', error);
            });
    }
    function fetchPosts() {
        var myHeaders = new Headers();
        myHeaders.append("Content-Type", "application/json");
        const authOptions = {
            method: 'GET',
            cache: 'no-cache',
            headers: myHeaders,
            credentials: 'include'
        };
        fetch('http://127.0.0.1:8086/api/messages/', authOptions)
            .then(response => {
                if (!response.ok) {
                    console.error('Failed to fetch posts:', response.status);
                    return null;
                }
                return response.json();
            })
            .then(posts => {
                if (posts === null || posts === undefined) {
                    console.warn('Received null or undefined posts.');
                    return;
                }
                console.log('Fetched Posts:', posts);
                displayPosts(posts);
            })
            .catch(error => {
                console.error('Error:', error);
            });
    }
    function displayPosts(posts) {
        const postsContainer = document.getElementById('posts');
        postsContainer.innerHTML = ''; // Clear existing posts
        posts.forEach(post => {
            updatePostsContainer(post.uid, post.message, post.likes);
        });
    }
    function updatePostsContainer(uid, message, likes) {
        const postsContainer = document.getElementById('posts');
        const postDiv = document.createElement('div');
        postDiv.className = 'post-container';
        const postContent = document.createElement('p');
        postContent.className = 'post-content';
        postContent.textContent = `UID: ${uid}, Message: ${message}`;
        const replyButton = document.createElement('button');
        replyButton.textContent = 'Reply';
        replyButton.addEventListener('click', () => showReplyForm(uid));
        const likeButton = document.createElement('button'); // Like button
        likeButton.textContent = 'Like';
        likeButton.addEventListener('click', () => likePost(uid));
        const likesCountSpan = document.createElement('span');
        likesCountSpan.className = 'likes-count';
        likesCountSpan.textContent = `${likes} 👍`; // Display likes count
        postDiv.appendChild(postContent);
        postDiv.appendChild(replyButton);
        postDiv.appendChild(likeButton);
        postDiv.appendChild(likesCountSpan); // Include likes count
        postsContainer.appendChild(postDiv);
    }
    function showReplyForm(parentUID) {
        const replyFormContainer = document.getElementById('replyFormContainer');
        replyFormContainer.innerHTML = ''; // Clear existing content
        const replyForm = document.createElement('form');
        replyForm.className = 'reply-form-container';
        replyForm.innerHTML = `
            <h3>Reply to UID: ${parentUID}</h3>
            <textarea id="replyMessage" placeholder="Type your reply..."></textarea>
            <button type="button" onclick="postReply('${parentUID}')">Post Reply</button>
        `;
        replyFormContainer.appendChild(replyForm);
    }
    function postReply(parentUID) {
        const replyMessage = document.getElementById('replyMessage').value;
        var myHeaders = new Headers();
        myHeaders.append("Content-Type", "application/json");
        const body = {
            uid: parentUID,
            message: replyMessage,
            likes: 0
        };
        const authOptions = {
            method: 'POST',
            cache: 'no-cache',
            headers: myHeaders,
            body: JSON.stringify(body),
            credentials: 'include'
        };
        fetch('http://127.0.0.1:8086/api/messages/send', authOptions)
            .then(response => {
                if (!response.ok) {
                    console.error('Failed to create reply:', response.status);
                    return null;
                }
                const contentType = response.headers.get('Content-Type');
                if (contentType && contentType.includes('application/json')) {
                    return response.json();
                } else {
                    return response.text();
                }
            })
            .then(data => {
                if (data !== null) {
                    console.log('Reply Response:', data);
                    // Clear the reply form after posting
                    const replyFormContainer = document.getElementById('replyFormContainer');
                    replyFormContainer.innerHTML = '';
                    // Fetch and update posts after posting a reply
                }
            })
            .catch(error => {
                console.error('Error:', error);
            });
    }
    function likePost(uid) {
        // Increment the like count in the DOM immediately
        const likesCountSpan = document.querySelector(`.post-container[data-uid="${uid}"] .likes-count`);
        if (likesCountSpan) {
            const currentLikes = parseInt(likesCountSpan.textContent, 10) || 0;
            likesCountSpan.textContent = `${currentLikes + 1} 👍`;
        }
        // Now, send the request to the server to update likes
        var myHeaders = new Headers();
        myHeaders.append("Content-Type", "application/json");
        // Prepare the request body
        const body = {
            uid: uid,
        };
        const authOptions = {
            method: 'PUT', // Assuming you are using a PUT request to update likes
            cache: 'no-cache',
            headers: myHeaders,
            body: JSON.stringify(body),
            credentials: 'include'
        };
        fetch(`http://127.0.0.1:8086/api/messages/like/${uid}`, authOptions)
            .then(response => {
                if (!response.ok) {
                    console.error('Failed to like post:', response.status);
                    // Revert the like count in case of an error
                    if (likesCountSpan) {
                        likesCountSpan.textContent = `${currentLikes} 👍`;
                    }
                    return null;
                }
                const contentType = response.headers.get('Content-Type');
                if (contentType && contentType.includes('application/json')) {
                    return response.json();
                } else {
                    return response.text();
                }
            })
            .then(data => {
                if (data !== null) {
                    console.log('Like Response:', data);
                    // Update the like count in the DOM if needed
                }
            })
            .catch(error => {
                console.error('Error:', error);
            });
    }
    function searchPosts() {
        const searchInput = document.getElementById('searchInput').value.toLowerCase();
        const postsContainer = document.getElementById('posts');
        const allPosts = postsContainer.querySelectorAll('.post-container');
        allPosts.forEach(post => {
            const postContent = post.querySelector('.post-content').textContent.toLowerCase();
            if (postContent.includes(searchInput)) {
                post.style.display = 'block';
            } else {
                post.style.display = 'none';
            }
        });
    }
</script>

<!-- Add a container for the reply form -->
<div id="replyFormContainer"></div>
```

# Links

🔗[this link leads to our most important backend file](https://github.com/SAAK-APCSP/Bluejay-Backend/blob/main/api/user.py)
🔗[this link leads to our most important frontend file](https://github.com/SAAK-APCSP/Bluejay-Frontend/blob/main/_posts/2024-02-09-dashboard.md)
🔗[this link has nothing to do with the project](https://www.youtube.com/watch?v=tB7YPnHp-Rc)