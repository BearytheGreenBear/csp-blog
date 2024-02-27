---
toc: true
comments: true
title: CPT Review
type: tangibles
courses: { compsci: {week: 22} }
---

```
🔴🟡🟢                      table-of-contents.txt
_________________________________________________________________________________
1️⃣ DESIGN: Preparation of key features prior to coding                         
2️⃣ CPT: Features map to College Board requirements                      
3️⃣ PULL REQUESTS: Pulls from fork/branch to main                             
4️⃣ CODE COMMITS: Comments and changes                          
5️⃣ 1-MIN VIDEO: CPT style video showcasing feature                              
```
# Design

Team Member | Tangibles
-- | --
Kyle | [Project Mapping](https://github.com/SAAK-APCSP/SAAK_repo/blob/main/images/csp_checkpoint_ideation_1.png), [UML Class Interactions](https://github.com/SAAK-APCSP/SAAK_repo/blob/main/images/UML%20Class%20diagram.png)
Shuban | [Posting Concept Design](https://github.com/SAAK-APCSP/SAAK_repo/blob/main/images/image.png)
Akhil | [Project Review Ticket](https://saak-apcsp.github.io/SAAK_repo//2023/12/19/CPT-brainstorm.html)
Aashray | [Login Page Concept Design](https://saak-apcsp.github.io/SAAK_repo/images/image_720.png)

# CPT

CPT Requirement | Feature(s) That Map to CB Requirements
-- | --
Program Input | Our project allows the user to input their own messages/posts, and the ability to like, edit, and reply. Javascript buttons and text-inputs are used to achieve this purpose. (In addition, our login and sign-up features are other examples of program input).
Use of list (or other collection type) to represent data | JSON is used to transfer data from backend (in python) to frontend (in javascript), keeping track of information like the message itself (a string), number of likes (an integer), and the user (a string). In the backend, an SQLite database is used to keep track of users and messages.
One or more procedure contributing to program's purpose | Like, edit, and reply functions are all procedures supporting the program's purpose. Each function takes in the respective paramaters (e.g. number of likes for the like function and the new message for the edit function, along with other data like user id), fetches the backend to complete its purpose, and recieves confirmation from the backend (e.g. if there were any errors or not).
Algorithm with sequencing, selection, and iteration | One notable example of iteration in the project is the function to display posts, where the procedure iterates through all posts in the database to display.
Calls to student-developed procedure | Use of Javascript functions and fetches to backend API.
Output based on input | Like, edit, and reply functions all have unique outputs based on user-input: updating like count, editing the message, or adding a reply to another message.

# Pull Requests

🔗[SASS Formatting](https://github.com/SAAK-APCSP/Bluejay-Frontend/pull/1)

# Code Commits

🔗[Posting and Viewing Feature](https://github.com/SAAK-APCSP/Bluejay-Frontend/commit/8ee79456be60a6f75404ca051128beb1a00a09d8)

🔗[Initial Like System Commit](https://github.com/SAAK-APCSP/Bluejay-Frontend/commit/a6330e37b71f09257cefbc907ada357ee4ce13ea)

🔗[Messages Sort/Search Feature](https://github.com/SAAK-APCSP/Bluejay-Frontend/commit/1bbe548be4a852d34706eb2b5507dae6c3c1a3eb)

🔗[Like System (Fixing Small Bugs, WIP)](https://github.com/SAAK-APCSP/Bluejay-Frontend/commit/a6330e37b71f09257cefbc907ada357ee4ce13ea)

🔗[Final CSS/SASS Styling](https://github.com/SAAK-APCSP/Bluejay-Frontend/commit/66a0068d9124ac13c8191d899d50cb63c1e64ae8)

🔗[Edit Feature (WIP)](https://github.com/SAAK-APCSP/Bluejay-Frontend/commit/66a0068d9124ac13c8191d899d50cb63c1e64ae8)

🔗[Reply Feature (WIP)](https://github.com/SAAK-APCSP/Bluejay-Frontend/commit/66a0068d9124ac13c8191d899d50cb63c1e64ae8)

🔗[Delete Feature](https://github.com/SAAK-APCSP/Bluejay-Frontend/commit/a92b16b9544f5fa010c50546c0ac40b1985dd830)

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
        /* Other Styling Goes Here */
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
    <!-- Other Functions Go Here -->
    </script>
```

# 1-Minute Video

🔗[Video Link (Google Drive)](https://drive.google.com/file/d/1axGI-L8X8oCt5i0HckZ7R1jSKcPp2fR8/view?usp=sharing)
