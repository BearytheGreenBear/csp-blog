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
  <link rel="stylesheet" href="styles.css">
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
    </form>ak
  </div>
</body>
</html>
