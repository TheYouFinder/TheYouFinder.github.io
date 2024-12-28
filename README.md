<!DOCTYPE html>
<html lang="en-US">
  <head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">

    <title>TheYouFinder</title>
    <meta name="generator" content="Jekyll v3.10.0" />
    <meta property="og:title" content="TheYouFinder" />
    <meta property="og:locale" content="en_US" />
    <link rel="canonical" href="https://theyoufinder.github.io/" />
    <meta property="og:url" content="https://theyoufinder.github.io/" />
    <meta property="og:site_name" content="TheYouFinder" />
    <meta property="og:type" content="website" />
    <meta name="twitter:card" content="summary" />
    <meta property="twitter:title" content="TheYouFinder" />
    <script type="application/ld+json">
      {"@context":"https://schema.org","@type":"WebSite","headline":"TheYouFinder","name":"TheYouFinder","url":"https://theyoufinder.github.io/"}
    </script>

    <link rel="stylesheet" href="/assets/css/style.css?v=5b46ff5c62af4723e2764f07c3261cf696391545">
  </head>
  <body>
    <div class="container-lg px-3 my-5 markdown-body">
      
      <h1><a href="https://theyoufinder.github.io/">TheYouFinder</a></h1>

      <html lang="en">
      <head>
          <meta charset="UTF-8" />
          <meta name="viewport" content="width=device-width, initial-scale=1.0" />
          <title>YouFinder</title>
          <link rel="icon" href="icon.png" type="image/png" />
          <style>
              body {
                  font-family: Arial, sans-serif;
                  margin: 0;
                  background-color: #f1f1f1;
              }
              header {
                  background-color: #ff0000;
                  color: white;
                  padding: 10px 20px;
                  text-align: center;
              }
              #searchSection {
                  text-align: center;
                  padding: 20px;
                  background-color: #fff;
                  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
              }
              #searchQuery {
                  padding: 10px;
                  width: 50%;
                  font-size: 16px;
                  border-radius: 5px;
                  border: 1px solid #ccc;
              }
              #searchButton {
                  padding: 10px 20px;
                  font-size: 16px;
                  background-color: #4CAF50;
                  color: white;
                  border: none;
                  border-radius: 5px;
                  cursor: pointer;
                  margin-left: 10px;
              }
              #searchButton:hover {
                  background-color: #45a049;
              }
              #results {
                  display: flex;
                  flex-wrap: wrap;
                  justify-content: center;
                  margin-top: 20px;
              }
              .video {
                  margin: 10px;
                  width: 300px;
                  background-color: white;
                  border-radius: 5px;
                  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
                  overflow: hidden;
                  cursor: pointer;
              }
              .video img {
                  width: 100%;
                  height: auto;
              }
              .video p {
                  padding: 10px;
                  font-size: 14px;
                  background-color: #f1f1f1;
                  margin: 0;
                  text-align: center;
              }

              /* Fallback for IE Flexbox */
              .video-container {
                  display: block;
              }
              .video-container .video {
                  display: inline-block;
              }
          </style>
      </head>
      <body>
          <header>
              <h1>YouFinder</h1>
          </header>

          <div id="searchSection">
              <input type="text" id="searchQuery" placeholder="Search YouTube..." />
              <button id="searchButton">Search</button>
          </div>

          <div id="results" class="video-container"></div>

          <script>
              // Function to handle search on button click
              function searchVideos() {
                  var query = document.getElementById('searchQuery').value;
                  if (!query) {
                      alert('Please enter a search term.');
                      return;
                  }

                  // Clear previous results
                  var resultsDiv = document.getElementById('results');
                  resultsDiv.innerHTML = '';

                  // YouTube Data API Request
                  var apiKey = 'AIzaSyBMccnFOmGA4g9-mqXlZsuRbzYb8aPtWsY'; // Your API Key
                  var apiUrl = 'https://www.googleapis.com/youtube/v3/search?part=snippet&type=video&maxResults=50&q=' + encodeURIComponent(query) + '&key=' + apiKey;

                  // Using XMLHttpRequest to make an API call
                  var xhr = new XMLHttpRequest();
                  xhr.open('GET', apiUrl, true);

                  xhr.onreadystatechange = function () {
                      if (xhr.readyState === 4 && xhr.status === 200) {
                          var response = JSON.parse(xhr.responseText);
                          displayResults(response);
                      } else if (xhr.readyState === 4) {
                          console.error('API call failed with status:', xhr.status);
                          alert('Failed to fetch results. Please check the console for errors.');
                      }
                  };
                  xhr.send();
              }

              // Function to display the API results
              function displayResults(data) {
                  var resultsDiv = document.getElementById('results');

                  if (data.items.length === 0) {
                      resultsDiv.innerHTML = '<p>No results found.</p>';
                      return;
                  }

                  for (var i = 0; i < data.items.length; i++) {
                      var item = data.items[i];
                      var videoId = item.id.videoId;
                      var title = item.snippet.title;
                      var thumbnail = item.snippet.thumbnails.high.url;

                      // Create a container for the video
                      var videoDiv = document.createElement('div');
                      videoDiv.className = 'video';

                      // Add thumbnail image
                      var img = document.createElement('img');
                      img.src = thumbnail;
                      videoDiv.appendChild(img);

                      // Add the title
                      var titleElement = document.createElement('p');
                      titleElement.textContent = title;
                      videoDiv.appendChild(titleElement);

                      // Add click event to open in a new tab
                      (function(videoId) {
                          videoDiv.onclick = function() {
                              window.open('https://www.youtube.com/embed/' + videoId, '_blank');
                          };
                      })(videoId);

                      // Append the video div to the results section
                      resultsDiv.appendChild(videoDiv);
                  }
              }

              // Event binding for the Search button (Cross-browser compatibility)
              var searchButton = document.getElementById('searchButton');
              if (searchButton.addEventListener) {
                  searchButton.addEventListener('click', searchVideos);
              } else if (searchButton.attachEvent) {
                  searchButton.attachEvent('onclick', searchVideos); // For IE
              }

              // Load random videos initially (for cross-browser compatibility)
              window.onload = function () {
                  var randomQueries = ['funny videos', 'tech reviews', 'music videos', 'cats', 'travel'];
                  var randomQuery = randomQueries[Math.floor(Math.random() * randomQueries.length)];
                  document.getElementById('searchQuery').value = randomQuery;
                  searchVideos();
              };
          </script>
      </body>
      </html>
    </div>
  </body>
</html>
