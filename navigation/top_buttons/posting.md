---
layout: post
title: Discover other recipes
search_exclude: true
hide: true
permalink: /navigation/buttons/posting
---

<head>
    <title>Posted Recipes</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
            background-color: #fdfdfd;
        }
        .post-container {
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            background: #ffffff;
            border: 1px solid #ddd;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }
        .post {
            border-bottom: 1px solid #ddd;
            padding: 10px 0;
        }
        .post:last-child {
            border-bottom: none;
        }
        h1 {
            text-align: center;
            color: #333;
        }
        .back-button {
            display: inline-block;
            margin-top: 20px;
            padding: 10px 15px;
            background-color: #007BFF;
            color: white;
            text-decoration: none;
            border-radius: 5px;
            font-size: 1rem;
        }
        .back-button:hover {
            background-color: #0056b3;
        }
        .clear-all-button {
            display: inline-block;
            margin-bottom: 20px;
            padding: 10px 15px;
            background-color: #dc3545; /* Red for delete */
            color: white;
            text-decoration: none;
            border: none;
            border-radius: 5px;
            font-size: 1rem;
            cursor: pointer;
        }
        .clear-all-button:hover {
            background-color: #a71d2a;
}
    </style>
</head>
<body>
    <div class="post-container">
        <h1>All Posted Recipes</h1>
        <button id="clearAllButton" class="clear-all-button">Clear All Posts</button>
        <div id="postsContainer">
            <!-- Posts will load here -->
        </div>
        <a href="/flocker_frontend/" class="back-button">Go Back to Home</a>
    </div>
    <script>
    // Load posts from localStorage
    const postsContainer = document.getElementById("postsContainer");
    const posts = JSON.parse(localStorage.getItem("posts")) || [];
    // Function to render posts
    function renderPosts() {
        postsContainer.innerHTML = ""; // Clear the container
        // Display each post
        if (posts.length === 0) {
            postsContainer.innerHTML = "<p>No recipes posted yet.</p>";
        } else {
            posts.forEach((post, index) => {
                const postElement = document.createElement("div");
                postElement.classList.add("post");
                postElement.innerHTML = `
                    <p><strong>Name:</strong> ${post.name}</p>
                    <p><strong>Dish:</strong> ${post.dish}</p>
                    <p><strong>Cuisine:</strong> ${post.cuisine}</p>
                    <p><strong>Link:</strong> <a href="${post.link}" target="_blank">${post.link}</a></p>
                    <p><strong>Comments:</strong> ${post.comments}</p>
                    <button class="delete-button" data-index="${index}">Delete</button>
                `;
                postsContainer.appendChild(postElement);
            });
        }
        // Attach delete event listeners
        const deleteButtons = document.querySelectorAll(".delete-button");
        deleteButtons.forEach(button => {
            button.addEventListener("click", function () {
                const index = this.getAttribute("data-index");
                deletePost(index);
            });
        });
    }
    // Function to delete a specific post
    function deletePost(index) {
        posts.splice(index, 1); // Remove the post at the given index
        localStorage.setItem("posts", JSON.stringify(posts)); // Update localStorage
        renderPosts(); // Re-render the posts
    }
    // Clear all posts
    document.getElementById("clearAllButton").addEventListener("click", function () {
        if (confirm("Are you sure you want to delete all posts?")) {
            localStorage.removeItem("posts"); // Remove all posts from localStorage
            posts.length = 0; // Clear the local array
            renderPosts(); // Re-render the posts
        }
    });
    // Render posts initially
    renderPosts();
</script>
</body>