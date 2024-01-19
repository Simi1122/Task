Server-side Pagination: Repositories are paginated server-side, with a default of 10 repositories per page. Users can choose to display up to 100 repositories per page.

Loader: A loader is displayed while the API calls are in progress, providing feedback to the user.

Search Bar: A search bar is available to filter repositories based on user input.

File Structure
index.html: Main HTML file containing the structure of the webpage.

styles.css: CSS file for styling the webpage.

main.js: JavaScript file containing the logic to fetch and display repositories.

Dependencies
No external dependencies are required. The application uses vanilla HTML, CSS, and JavaScript.

API Documentation
The GitHub REST API documentation can be found here.

License
This project is licensed under the MIT License.


**#index.html**
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="styles.css">
    <title>GitHub Repositories</title>
</head>
<body>
    <div class="container">
        <h1>GitHub Repositories</h1>
        <div>
            <label for="username">Enter GitHub Username:</label>
            <input type="text" id="username" placeholder="Enter username">
            <button onclick="getRepositories()">Get Repositories</button>
        </div>
        <div id="loader" class="loader"></div>
        <div id="repositories"></div>
    </div>

    <script src="main.js"></script>
</body>
</html>


**styles.css**
body {
    font-family: 'Arial', sans-serif;
    margin: 0;
    padding: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100vh;
    background-color: #f4f4f4;
}

.container {
    text-align: center;
}

input, button {
    margin: 10px;
    padding: 8px;
}

.loader {
    border: 8px solid #f3f3f3;
    border-top: 8px solid #3498db;
    border-radius: 50%;
    width: 50px;
    height: 50px;
    animation: spin 1s linear infinite;
    display: none;
}

@keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
}


**main.js**
async function getRepositories() {
    const username = document.getElementById('username').value;
    const repositoriesContainer = document.getElementById('repositories');
    const loader = document.getElementById('loader');

    loader.style.display = 'block';
    repositoriesContainer.innerHTML = '';

    try {
        const response = await fetch(`https://api.github.com/users/${username}/repos`);
        const repositories = await response.json();

        repositories.forEach(repo => {
            const repoDiv = document.createElement('div');
            repoDiv.innerHTML = `<h3>${repo.name}</h3>
                                 <p>Topics: ${repo.topics.join(', ')}</p>`;
            repositoriesContainer.appendChild(repoDiv);
        });
    } catch (error) {
        console.error('Error fetching repositories:', error);
        repositoriesContainer.innerHTML = '<p>Error fetching repositories. Please try again.</p>';
    } finally {
        loader.style.display = 'none';
    }
}


