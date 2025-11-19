# Ex.08 Design of Interactive Image Gallery

## AIM
  To design a web application for an inteactive image gallery with minimum five images.

## DESIGN STEPS

## Step 1:

Clone the github repository and create Django admin interface

## Step 2:

Change settings.py file to allow request from all hosts.

## Step 3:

Use CSS for positioning and styling.

## Step 4:

Write JavaScript program for implementing interactivit

## Step 5:

Validate the HTML and CSS code

## Step 6:

Publish the website in the given URL.

## PROGRAM
## Gallery.html :
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Fav</title>
    <link rel="stylesheet" href="Gallery.css">
</head>
<body>
    <h1>Today Pics</h1>

    <div class="gallery" onclick="openLightbox(event)">
        <img src="1.jpg"
            alt="me">
        <img src="WhatsApp Image 2025-11-19 at 10.57.57_0b12a8fb.jpg"
            alt="sample1">
        <img src="WhatsApp Image 2025-11-19 at 10.57.58_18eacebe.jpg"
            alt="sample2">
        <img src="WhatsApp Image 2025-11-19 at 10.57.59_cf364028.jpg"
            alt="sample3">
    </div>

    <div id="lightbox">
        <span id="close-btn" onclick="closeLightbox()">&times;</span>

        <img id="lightbox-img" src="" alt="lightbox image">

        <div id="thumbnail-container">
        </div>
        <button id="prev-btn" onclick="changeImage(-1)">&lt; Prev</button>
        <button id="next-btn" onclick="changeImage(1)">Next &gt;</button>
    </div>
    <script src="Gallery.js"></script>

</body>
</html>
```

## Gallery.css:
```
@import url('https://fonts.googleapis.com/css2?family=Cormorant+Garamond&family=DM+Mono:wght@400;500&display=swap');


/* Basic styling for the gallery */
        body {
            font-family: 'Cormorant Garamond', sans-serif;
            margin: 0;
            padding: 0;  
            background: #131212;
        }

        h1 {
            text-align: center;
            padding: 40px;
            margin: 0;
            color: beige;
            font-size: 3em;
        }

        .gallery {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            padding: 20px;
        }

        .gallery img {
            margin: 10px;
            cursor: pointer;
            max-width: 300px;
            width: 50%;
            height: 50%;
          border-radius: 10px;
        }

        /* Lightbox styles */
        #lightbox {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.8);
            justify-content: center;
            align-items: center;
            overflow: hidden;
            flex-direction: column;
        }

        #lightbox img {
            max-width: 80%;
            max-height: 60vh;
            box-shadow: 0 0 25px rgba(0, 0, 0, 0.8);
          border-radius: 10px;
        }

        #close-btn {
            position: absolute;
            top: 10px;
            right: 10px;
            font-size: 24px;
            color: #fff;
            cursor: pointer;
            z-index: 2;
        }

        /* Style for navigation buttons */
        #prev-btn,
        #next-btn {
            position: absolute;
            top: 50%;
            transform: translateY(-50%);
            font-size: 20px;
            color: #fff;
            background-color: rgba(0, 0, 0, 0.5);
            border: none;
            padding: 10px;
            cursor: pointer;
            transition: background-color 0.3s;
        }

        #prev-btn {
            left: 10px;
        }

        #next-btn {
            right: 10px;
        }

        #prev-btn:hover,
        #next-btn:hover {
            background-color: rgba(0, 0, 0, 0.8);
        }

        /* Styles for thumbnails */
        .thumbnail-container {
            display: flex;
            flex-direction: row;
            flex-wrap: wrap;
            justify-content: center;
        }
        .thumbnail {
            max-width: 50px;
            width: 100px;
            cursor: pointer;
            margin-top: 40px;
            margin-left: 5px;
            margin-right: 5px;
            border: 2px solid #fff;
            transition: opacity 0.3s;
        }

        .thumbnail:hover,
        .thumbnail.active-thumbnail {
            opacity: 0.7;
        }
        #thumbnail-container {
            display: flex;
            flex-direction: row;
            flex-wrap: wrap;
            justify-content: center;
            padding: 10px;
        }
```

## Gallery.js :
```
let currentIndex = 0;
        const images = document.querySelectorAll('.gallery img');
        const totalImages = images.length;

        // Open the lightbox
        function openLightbox(event) {
            if (event.target.tagName === 'IMG') {
                const clickedIndex = Array.from(images).indexOf(event.target);
                currentIndex = clickedIndex;
                updateLightboxImage();
                document.getElementById('lightbox').style.display = 'flex';
            }
        }

        // Close the lightbox
        function closeLightbox() {
            document.getElementById('lightbox').style.display = 'none';
        }

        // Change the lightbox image based on direction (1 for next, -1 for prev)
        function changeImage(direction) {
            currentIndex += direction;
            if (currentIndex >= totalImages) {
                currentIndex = 0;
            } else if (currentIndex < 0) {
                currentIndex = totalImages - 1;
            }
            updateLightboxImage();
        }

        // Update the lightbox image and thumbnails
        function updateLightboxImage() {
            const lightboxImg = document.getElementById('lightbox-img');
            const thumbnailContainer = document.getElementById('thumbnail-container');

            // Update the main lightbox image
            lightboxImg.src = images[currentIndex].src;

            // Clear existing thumbnails
            thumbnailContainer.innerHTML = '';

            // Add new thumbnails
            images.forEach((image, index) => {
                const thumbnail = document.createElement('img');
                thumbnail.src = image.src;
                thumbnail.alt = `Thumbnail ${index + 1}`;
                thumbnail.classList.add('thumbnail');
                thumbnail.addEventListener('click', () => updateMainImage(index));
                thumbnailContainer.appendChild(thumbnail);
            });

            // Highlight the current thumbnail
            const thumbnails = document.querySelectorAll('.thumbnail');
            thumbnails[currentIndex].classList.add('active-thumbnail');
        }

        // Update the main lightbox image when a thumbnail is clicked
        function updateMainImage(index) {
            currentIndex = index;
            updateLightboxImage();
        }

        // Add initial thumbnails
        updateLightboxImage();


        // To add keyboard navigation (left/right arrow keys)
        document.addEventListener('keydown', function (e) {
            if (document.getElementById('lightbox').style.display === 'flex') {
                if (e.key === 'ArrowLeft') {
                    changeImage(-1);
                } else if (e.key === 'ArrowRight') {
                    changeImage(1);
                }
            }
        });

                // Wait for DOM to be fully loaded
        document.addEventListener('DOMContentLoaded', function() {
            let currentIndex = 0;
            const images = document.querySelectorAll('.gallery img');
            const totalImages = images.length;

            // ...existing code...

            // Add escape key support
            document.addEventListener('keydown', function(e) {
                if (document.getElementById('lightbox').style.display === 'flex') {
                    if (e.key === 'ArrowLeft') {
                        changeImage(-1);
                    } else if (e.key === 'ArrowRight') {
                        changeImage(1);
                    } else if (e.key === 'Escape') {
                        closeLightbox();
                    }
                }
            });

            // Initialize thumbnails only after DOM is loaded
            updateLightboxImage();
        });
```

## OUTPUT
![alt text](ex81.png)
![alt text](ex82.png)
## RESULT
  The program for designing an interactive image gallery using HTML, CSS and JavaScript is executed successfully.
