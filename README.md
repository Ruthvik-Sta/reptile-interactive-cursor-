# reptile-interactive-cursor-
the overall outcome of this source code is nothing but a cursor which interacts as per ur so designed reptile 

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Animated Reptile Cursor</title>
    <style>
        * {
            cursor: none; /* Hide default cursor */
        }

        body {
            margin: 0;
            height: 100vh;
            background-color: #222;
            color: white;
            font-family: Arial, sans-serif;
            overflow: hidden;
        }

        .content {
            padding: 2rem;
            max-width: 800px;
            margin: 0 auto;
            text-align: center;
        }

        .cursor-container {
            position: fixed;
            pointer-events: none;
            z-index: 9999;
            display: flex;
            flex-direction: row;
            transform-origin: left center;
        }

        .lizard-head {
            width: 30px;
            height: 20px;
            background-color: #5cb85c;
            border-radius: 50% 50% 50% 10%;
            position: relative;
            transform: rotate(0deg);
            animation: headBob 0.3s infinite alternate;
        }

        .eye {
            position: absolute;
            width: 5px;
            height: 5px;
            background-color: black;
            border-radius: 50%;
            top: 5px;
        }

        .eye.left {
            right: 7px;
        }

        .eye.right {
            right: 2px;
        }

        .tongue {
            position: absolute;
            width: 10px;
            height: 2px;
            background-color: #ff6b6b;
            border-radius: 10px;
            bottom: 3px;
            right: -5px;
            transform-origin: right center;
            animation: tongueFlick 3s infinite;
            opacity: 0.8;
        }

        .tail-section {
            width: 20px;
            height: 10px;
            background-color: #5cb85c;
            border-radius: 0 50% 50% 0;
            margin-left: -5px;
            transform-origin: left center;
        }

        @keyframes headBob {
            0% { transform: rotate(-5deg); }
            100% { transform: rotate(5deg); }
        }

        @keyframes tongueFlick {
            0%, 95% { 
                transform: rotate(0deg);
                width: 10px;
            }
            97% { 
                transform: rotate(-20deg);
                width: 15px;
            }
            98% { 
                transform: rotate(20deg);
                width: 15px;
            }
            100% { 
                transform: rotate(0deg);
                width: 10px;
            }
        }

        .demo-text {
            margin-top: 20px;
            font-size: 1.5rem;
            line-height: 1.6;
        }

        .highlight {
            background-color: rgba(92, 184, 92, 0.3);
            padding: 2px 5px;
            border-radius: 3px;
        }

        .demo-box {
            margin-top: 50px;
            padding: 20px;
            background-color: rgba(255, 255, 255, 0.1);
            border-radius: 10px;
            transition: all 0.3s;
        }

        .demo-box:hover {
            background-color: rgba(92, 184, 92, 0.2);
            transform: scale(1.02);
        }
    </style>
</head>
<body>
    <div class="content">
        <h1>Animated Reptile Cursor</h1>
        <p class="demo-text">
            Watch your cursor transform into a <span class="highlight">lively lizard</span> with 
            <span class="highlight">animated movements</span> and a 
            <span class="highlight">wagging tail!</span>
        </p>
        
        <div class="demo-box">
            <h2>Hover over this box to see an interaction effect</h2>
            <p>The lizard will react to different elements on the page.</p>
        </div>
    </div>

    <div class="cursor-container">
        <div class="lizard-head">
            <div class="eye left"></div>
            <div class="eye right"></div>
            <div class="tongue"></div>
        </div>
        <div class="tail-section" id="tail1"></div>
        <div class="tail-section" id="tail2"></div>
        <div class="tail-section" id="tail3"></div>
    </div>

    <script>
        const cursor = document.querySelector('.cursor-container');
        const tail1 = document.getElementById('tail1');
        const tail2 = document.getElementById('tail2');
        const tail3 = document.getElementById('tail3');
        
        let mouseX = 0;
        let mouseY = 0;
        let cursorX = 0;
        let cursorY = 0;
        let tailAngle1 = 0;
        let tailAngle2 = 0;
        let tailAngle3 = 0;
        let previousMouseX = 0;
        let speed = 0;
        
        document.addEventListener('mousemove', (e) => {
            mouseX = e.clientX;
            mouseY = e.clientY;
            
            // Calculate speed based on mouse movement
            speed = Math.abs(e.clientX - previousMouseX);
            previousMouseX = e.clientX;
        });
        
        function animateCursor() {
            // Smooth follow effect
            cursorX += (mouseX - cursorX) * 0.2;
            cursorY += (mouseY - cursorY) * 0.2;
            
            // Position the cursor container
            cursor.style.left = cursorX + 'px';
            cursor.style.top = cursorY + 'px';
            
            // Calculate rotation based on movement direction
            const dx = mouseX - cursorX;
            const dy = mouseY - cursorY;
            let angle = Math.atan2(dy, dx) * 180 / Math.PI;
            
            // Adjust tail angles based on movement
            tailAngle1 = Math.sin(Date.now() * 0.005) * 15;
            tailAngle2 = Math.sin(Date.now() * 0.005 + 0.5) * 20;
            tailAngle3 = Math.sin(Date.now() * 0.005 + 1) * 25;
            
            // Increase tail movement when mouse moves fast
            if (speed > 2) {
                const speedFactor = Math.min(speed / 10, 3);
                tailAngle1 *= speedFactor;
                tailAngle2 *= speedFactor;
                tailAngle3 *= speedFactor;
            }
            
            // Apply transformations
            tail1.style.transform = `rotate(${tailAngle1}deg)`;
            tail2.style.transform = `rotate(${tailAngle2}deg)`;
            tail3.style.transform = `rotate(${tailAngle3}deg)`;
            cursor.style.transform = `rotate(${angle}deg)`;
            
            requestAnimationFrame(animateCursor);
        }
        
        // Handle hover states
        const interactiveElements = document.querySelectorAll('.demo-box, .highlight, button, a');
        interactiveElements.forEach(el => {
            el.addEventListener('mouseenter', () => {
                cursor.style.transform += ' scale(1.2)';
                document.querySelector('.tongue').style.opacity = '1';
            });
            
            el.addEventListener('mouseleave', () => {
                cursor.style.transform = cursor.style.transform.replace(' scale(1.2)', '');
                document.querySelector('.tongue').style.opacity = '0.8';
            });
        });
        
        animateCursor();
    </script>
</body>
</html>
