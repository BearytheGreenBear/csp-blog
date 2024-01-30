---
toc: true
comments: true
title: Login Page
type: hacks
courses: { compsci: {week: 19} }
---
```
/your-project
├── login.md
├── styles
│   ├── main.scss
│   └── _variables.scss
└── styles.css
```

<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body {
      font-family: Helvetica, sans-serif;
      background-color: #ff008c;
      margin: 0;
      padding: 0;
      display: flex;
      align-items: center;
      justify-content: center;
      height: 100vh;
    }
    .login-container {
      background-color: #00fff7;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
      width: 300px;
    }
  </style>
  <title>Login Page</title>
</head>
<body>
  <div class="login-container">
    <form>
      <div class="form-group">
        <label for="username">Username:</label>
        <input type="text" id="username" name="username">
      </div>
      <div class="form-group">
        <label for="password">Password:</label>
        <input type="password" id="password" name="password">
      </div>
      <button type="submit">Login</button>
    </form>
  </div>
</body>
</html>
