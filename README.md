<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>The Modifier Lab</title>
    <style>
        :root {
            /* All UI colors set to black */
            --primary: #000000;
            --text-main: #000000;
            --bg: #f0f2f5;
        }

        body { 
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; 
            background: var(--bg); 
            display: flex; 
            flex-direction: column; 
            align-items: center; 
            padding: 40px; 
            color: var(--text-main); 
        }
        
        .container { 
            background: white; 
            padding: 40px; 
            border-radius: 20px; 
            box-shadow: 0 10px 30px rgba(0,0,0,0.1); 
            max-width: 700px; 
            width: 100%; 
            text-align: center; 
        }

        h1, p { color: var(--text-main); }
        
        .progress-bar { width: 100%; height: 10px; background: #ddd; border-radius: 5px; margin-bottom: 30px; overflow: hidden; }
        .progress-fill { height: 100%; background: var(--primary); width: 0%; transition: 0.5s; }

        .game-screen { display: none; }
        .game-screen.active { display: block; }

        .sentence-display { 
            font-size: 1.4rem; 
            padding: 30px; 
            background: #f8f9fa; 
            border-left: 5px solid var(--primary); 
            margin: 20px 0; 
            line-height: 1.6; 
            color: var(--text-main);
        }

        .btn-group { display: flex; gap: 15px; justify-content: center; margin-top: 20px; }
        button { 
            padding: 12px 25px; 
            border: 2px solid #000000; 
            border-radius: 8px; 
            cursor: pointer; 
            font-size: 1rem; 
            font-weight: bold; 
            background: white; 
            color: #000000; 
            transition: 0.3s; 
        }
        
        button:hover { background: #eeeeee; }

        /* Level 2 Styles */
        .drop-zone { 
            border-bottom: 3px dashed #000000; 
            color: #000000; 
            padding: 0 10px; 
            font-weight: bold; 
        }
        
        .draggables { display: flex; gap: 10px; justify-content: center; margin-top: 30px; }
        
        .draggable { 
            background: white; 
            border: 2px solid #000000; 
            padding: 10px 20px; 
            border-radius: 8px; 
            cursor: grab; 
            font-weight: bold; 
            color: #000000; 
        }

        #feedback { 
            margin-top: 20px; 
            font-weight: bold; 
            min-height: 24px; 
            color: #000000; /* Feedback is now black */
        }
    </style>
</head>
<body>

    <div class="container">
        <div class="progress-bar"><div class="progress-fill" id="progress"></div></div>
        
        <div id="level1" class="game-screen active">
            <h1>Level 1: Detective Work</h1>
            <p>Is the person doing the action actually in the sentence?</p>
            <div class="sentence-display" id="l1-sentence">
                "Hoping to garner favor, my parent's approval was sought."
            </div>
            <div class="btn-group">
                <button onclick="checkL1(true)">Dangling Modifier!</button>
                <button onclick="checkL1(false)">Looks Correct</button>
            </div>
        </div>

        <div id="level2" class="game-screen">
            <h1>Level 2: The Repair Shop</h1>
            <p>Drag the <b>correct subject</b> to anchor the modifier.</p>
            <div class="sentence-display">
                "After rotting in the cellar for weeks, <span id="dropzone" class="drop-zone">_______</span> were finally thrown out."
            </div>
            <div class="draggables">
                <div class="draggable" draggable="true" id="opt1">the moldy apples</div>
                <div class="draggable" draggable="true" id="opt2">the farmer</div>
                <div class="draggable" draggable="true" id="opt3">the hungry kids</div>
            </div>
        </div>

        <div id="feedback"></div>
    </div>

    <script>
        const feedback = document.getElementById('feedback');
        const progress = document.getElementById('progress');

        // Level 1 Logic
        function checkL1(isDangling) {
            if (isDangling) {
                feedback.innerHTML = "Correct! 'My parent's approval' can't hope to garner favor—only a person can.";
                progress.style.width = "50%";
                setTimeout(() => {
                    document.getElementById('level1').classList.remove('active');
                    document.getElementById('level2').classList.add('active');
                    feedback.innerHTML = "";
                }, 2000);
            } else {
                feedback.innerHTML = "Not quite. Who is 'hoping'? It's missing!";
            }
        }

        // Level 2 Logic
        const draggables = document.querySelectorAll('.draggable');
        const dropzone = document.getElementById('dropzone');

        draggables.forEach(drag => {
            drag.addEventListener('dragstart', (e) => {
                e.dataTransfer.setData('text', e.target.id);
            });
        });

        dropzone.addEventListener('dragover', (e) => e.preventDefault());

        dropzone.addEventListener('drop', (e) => {
            e.preventDefault();
            const id = e.dataTransfer.getData('text');
            if (id === 'opt1') {
                dropzone.innerText = "the moldy apples";
                dropzone.style.border = "none";
                feedback.innerHTML = "Excellent! The apples were rotting, not the farmer!";
                progress.style.width = "100%";
            } else {
                feedback.innerHTML = "Try again! Did the person rot for weeks?";
            }
        });
    </script>
</body>
</html># DanglingModifiers2
