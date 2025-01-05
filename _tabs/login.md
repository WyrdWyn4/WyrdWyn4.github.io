---
# the default layout is 'page'
layout: default
icon: fas fa-user-circle
order: 5
---

<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Login Page</title>
    <script>
      function loadAuth0Script() {
        console.log("Loading Auth0 script...");
        return new Promise((resolve, reject) => {
          const script = document.createElement("script");
          script.src = "https://cdn.auth0.com/js/auth0/9.21/auth0.min.js";
          script.onload = () => {
            console.log("Auth0 script loaded successfully.");
            resolve();
          };
          script.onerror = (error) => {
            console.error("Error loading Auth0 script:", error);
            reject(error);
          };
          document.head.appendChild(script);
        });
      }

      async function initializeAuth0() {
        try {
          console.log("Initializing Auth0...");
          await loadAuth0Script();

          const auth = new auth0.WebAuth({
            domain: "wyrdwyn4.ca.auth0.com",
            clientID: "hRGbOJutwauypUmF8Wsg702DC3Dfi8UT",
            redirectUri: "https://wyrdwyn4.github.io/",
            responseType: "token id_token",
            scope: "openid profile email"
          });

          console.log("Auth0 initialized:", auth);

          document.getElementById("login-btn").addEventListener("click", () => {
            console.log("Login button clicked.");
            auth.authorize();
          });

          document.getElementById("logout-btn").addEventListener("click", () => {
            console.log("Logout button clicked.");
            logout(auth);
          });
        } catch (error) {
          console.error("Error initializing Auth0:", error);
          alert("An error occurred while loading the login page. Please try again later.");
        }
      }

      function logout(auth) {
        console.log("Logging out...");
        localStorage.clear();
        const auth0LogoutUrl = `https://wyrdwyn4.ca.auth0.com/v2/logout?client_id=hRGbOJutwauypUmF8Wsg702DC3Dfi8UT&returnTo=https://wyrdwyn4.github.io/`;
        window.location.href = auth0LogoutUrl;
      }

      document.addEventListener("DOMContentLoaded", initializeAuth0);
    </script>

    <style>
      h1 {
        color: #7D7D7D;
      }
      .login-container {
        margin-top: 50px;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
      }
      .icon {
        margin-bottom: 20px;
      }
      button {
        background-color: #2196F3;
        color: white;
        padding: 10px 20px;
        border: none;
        border-radius: 5px;
        cursor: pointer;
        font-size: 16px;
        margin: 10px 0;
        width: 150px;
      }
      button.logout {
        background-color: #F44336;
      }
      button:hover {
        opacity: 0.9;
      }
      .icon svg {
        width: 100px;
        height: 100px;
        fill: #333;
      }
    </style>
  </head>
  <body>
    <div class="login-container">
      <i class="fas fa-user-circle" style="font-size: 100px;"></i>
      <h1 style="text-align: center;">Welcome to the Login Page</h1>
      <p style="text-align: center;">Please log in to access restricted content.</p>
      <button id="login-btn">Login</button>
      <button id="logout-btn" class="logout">Logout</button>
    </div>
  </body>
</html>

<script>
  document.addEventListener("DOMContentLoaded", () => {
    const toggleButton = document.getElementById("mode-toggle");
    const body = document.body;

    toggleButton.addEventListener("click", () => {
      applyThemeColors();
    });
  });

  function applyThemeColors() {
    const themeMapper = Theme.getThemeMapper('default', 'dark');
    const newTheme = themeMapper[Theme.visualState];

    console.log("Theme mode: " + newTheme);
    const login = document.querySelector('.login-container');

    if (newTheme === "dark") {
      login.style.color = 'white';
      console.log("Dark mode enabled");
    } else {
      login.style.color = 'black';
      console.log("Light mode enabled");
    }
  }
</script>

<br><br><br><br><br>

> Access to some resources are restricted. Please request [The Administrator](/about/) for access.
{: .prompt-tip }