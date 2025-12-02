<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PromptLayer Registry & Lucio AI Demo</title>
    <style>
        :root {
            /* White Theme Variables */
            --bg-body: #f3f4f6;
            --bg-app: #ffffff;
            --bg-panel: #f9fafb;
            --bg-input: #ffffff;
            --text-main: #111827;
            --text-muted: #6b7280;
            --accent: #6366f1;
            /* Indigo */
            --accent-hover: #4f46e5;
            --border: #e5e7eb;
            --success: #10b981;
            --font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;

            /* Word Theme */
            --word-blue: #2b579a;
            --word-bg: #f0f0f0;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            background-color: var(--bg-body);
            color: var(--text-main);
            font-family: var(--font-family);
            height: 100vh;
            overflow: hidden;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        /* --- App Container --- */
        .app-window {
            width: 1200px;
            height: 800px;
            background-color: var(--bg-app);
            border: 1px solid var(--border);
            border-radius: 12px;
            box-shadow: 0 20px 50px rgba(0, 0, 0, 0.15);
            display: flex;
            overflow: hidden;
            position: relative;
        }

        /* --- Sidebar --- */
        .sidebar {
            width: 240px;
            background-color: var(--bg-panel);
            border-right: 1px solid var(--border);
            padding: 20px;
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .logo {
            font-size: 18px;
            font-weight: 700;
            color: var(--text-main);
            display: flex;
            align-items: center;
            gap: 10px;
            margin-bottom: 20px;
        }

        .logo-icon {
            width: 24px;
            height: 24px;
            background: linear-gradient(135deg, #ec4899, #8b5cf6);
            border-radius: 6px;
        }

        .nav-item {
            padding: 10px 12px;
            border-radius: 6px;
            color: var(--text-muted);
            cursor: pointer;
            transition: all 0.2s;
            display: flex;
            align-items: center;
            gap: 10px;
            font-size: 14px;
            font-weight: 500;
        }

        .nav-item.active,
        .nav-item:hover {
            background-color: #eef2ff;
            /* Light indigo tint */
            color: var(--accent);
        }

        /* --- Main Content --- */
        .main-content {
            flex: 1;
            display: flex;
            flex-direction: column;
            position: relative;
            background-color: var(--bg-app);
        }

        .header {
            height: 60px;
            border-bottom: 1px solid var(--border);
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 0 24px;
            background-color: var(--bg-app);
        }

        .breadcrumb {
            font-size: 14px;
            color: var(--text-muted);
        }

        .breadcrumb span {
            color: var(--text-main);
            font-weight: 600;
        }

        .btn {
            background-color: var(--accent);
            color: white;
            border: none;
            padding: 8px 16px;
            border-radius: 6px;
            font-size: 13px;
            font-weight: 500;
            cursor: pointer;
            transition: background 0.2s;
            box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05);
        }

        .btn:hover {
            background-color: var(--accent-hover);
        }

        .btn-secondary {
            background-color: white;
            border: 1px solid var(--border);
            color: var(--text-main);
        }

        .btn-secondary:hover {
            background-color: var(--bg-panel);
        }

        /* --- Views --- */
        .view {
            flex: 1;
            padding: 24px;
            display: none;
            opacity: 0;
            transition: opacity 0.3s ease;
            height: 100%;
            overflow-y: auto;
        }

        .view.active {
            display: block;
            opacity: 1;
        }

        /* Registry List View */
        .registry-table {
            width: 100%;
            border-collapse: collapse;
        }

        .registry-table th {
            text-align: left;
            padding: 12px;
            color: var(--text-muted);
            font-size: 12px;
            font-weight: 600;
            border-bottom: 1px solid var(--border);
            text-transform: uppercase;
            letter-spacing: 0.05em;
        }

        .registry-table td {
            padding: 16px 12px;
            border-bottom: 1px solid var(--border);
            font-size: 14px;
            color: var(--text-main);
        }

        .tag {
            background-color: #dcfce7;
            color: #166534;
            padding: 4px 8px;
            border-radius: 4px;
            font-size: 11px;
            font-weight: 600;
        }

        /* Editor View */
        .editor-layout {
            display: flex;
            height: 100%;
            gap: 24px;
        }

        .editor-left {
            flex: 2;
            display: flex;
            flex-direction: column;
            gap: 16px;
        }

        .editor-right {
            flex: 1;
            background-color: var(--bg-panel);
            border-radius: 8px;
            padding: 20px;
            border: 1px solid var(--border);
        }

        .input-group {
            display: flex;
            flex-direction: column;
            gap: 8px;
        }

        .label {
            font-size: 12px;
            color: var(--text-muted);
            font-weight: 600;
            text-transform: uppercase;
        }

        .input-field {
            background-color: var(--bg-input);
            border: 1px solid var(--border);
            color: var(--text-main);
            padding: 10px;
            border-radius: 6px;
            font-family: inherit;
            font-size: 14px;
            transition: border-color 0.2s;
        }

        .input-field:focus {
            outline: none;
            border-color: var(--accent);
            box-shadow: 0 0 0 3px rgba(99, 102, 241, 0.1);
        }

        .prompt-area {
            flex: 1;
            background-color: var(--bg-input);
            border: 1px solid var(--border);
            border-radius: 6px;
            padding: 16px;
            font-family: 'Monaco', 'Consolas', monospace;
            font-size: 14px;
            line-height: 1.6;
            color: var(--text-main);
            resize: none;
        }

        /* Code View */
        .code-container {
            background-color: #1e1e1e;
            color: #d4d4d4;
            padding: 24px;
            border-radius: 8px;
            font-family: 'Consolas', monospace;
            font-size: 14px;
            line-height: 1.5;
            position: relative;
        }

        .code-keyword {
            color: #569cd6;
        }

        .code-string {
            color: #ce9178;
        }

        .code-function {
            color: #dcdcaa;
        }

        .code-comment {
            color: #6a9955;
        }

        /* Word View */
        .word-app {
            display: flex;
            flex-direction: column;
            height: 100%;
            background-color: var(--word-bg);
        }

        .word-ribbon {
            height: 50px;
            background-color: var(--word-blue);
            display: flex;
            align-items: center;
            padding: 0 20px;
            color: white;
            font-weight: 600;
            font-size: 14px;
            gap: 20px;
        }

        .word-ribbon-item {
            opacity: 0.8;
            font-weight: 400;
        }

        .word-ribbon-item.active {
            opacity: 1;
            font-weight: 600;
            border-bottom: 2px solid white;
            padding-bottom: 13px;
            margin-top: 15px;
        }

        .word-workspace {
            flex: 1;
            display: flex;
            overflow: hidden;
        }

        .word-doc-area {
            flex: 1;
            display: flex;
            justify-content: center;
            padding: 40px;
            overflow-y: auto;
        }

        .word-paper {
            width: 600px;
            height: 800px;
            /* A4-ish */
            background-color: white;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
            padding: 60px;
            font-family: 'Calibri', sans-serif;
            font-size: 12pt;
            color: black;
            line-height: 1.5;
        }

        .lucio-sidebar {
            width: 300px;
            background-color: white;
            border-left: 1px solid #ccc;
            display: flex;
            flex-direction: column;
        }

        .lucio-header {
            padding: 15px;
            border-bottom: 1px solid #eee;
            font-weight: 700;
            color: var(--accent);
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .lucio-body {
            padding: 20px;
            flex: 1;
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        /* --- Cursor & Overlays --- */
        .cursor {
            width: 24px;
            height: 24px;
            position: absolute;
            top: 0;
            left: 0;
            pointer-events: none;
            z-index: 9999;
            transition: transform 0.1s;
            filter: drop-shadow(0 2px 4px rgba(0, 0, 0, 0.2));
        }

        .cursor::after {
            content: '';
            position: absolute;
            top: 50%;
            left: 50%;
            width: 0;
            height: 0;
            border-left: 6px solid transparent;
            border-right: 6px solid transparent;
            border-top: 18px solid #000;
            /* Black cursor for visibility on white */
            transform: translate(-50%, -50%) rotate(-15deg);
        }

        .click-effect {
            position: absolute;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background-color: rgba(99, 102, 241, 0.3);
            transform: scale(0);
            animation: clickRipple 0.4s ease-out;
            pointer-events: none;
            z-index: 9998;
        }

        @keyframes clickRipple {
            0% {
                transform: scale(0);
                opacity: 1;
            }

            100% {
                transform: scale(2);
                opacity: 0;
            }
        }

        .guide-overlay {
            position: absolute;
            bottom: 40px;
            left: 50%;
            transform: translateX(-50%);
            background-color: rgba(17, 24, 39, 0.9);
            padding: 12px 24px;
            border-radius: 30px;
            color: white;
            font-size: 16px;
            font-weight: 500;
            pointer-events: none;
            opacity: 0;
            transition: opacity 0.5s;
            z-index: 10000;
            text-align: center;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
        }

        .toast {
            position: absolute;
            top: 20px;
            right: 20px;
            background-color: white;
            border-left: 4px solid var(--success);
            color: var(--text-main);
            padding: 16px 24px;
            border-radius: 4px;
            display: flex;
            align-items: center;
            gap: 12px;
            transform: translateY(-100px);
            transition: transform 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            z-index: 10001;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            font-weight: 500;
        }

        .toast.show {
            transform: translateY(0);
        }

        .replay-btn {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%) scale(0);
            background-color: var(--accent);
            color: white;
            padding: 16px 32px;
            border-radius: 50px;
            font-size: 18px;
            font-weight: 600;
            border: none;
            cursor: pointer;
            z-index: 10002;
            transition: transform 0.3s;
            box-shadow: 0 10px 30px rgba(99, 102, 241, 0.4);
        }

        .replay-btn.show {
            transform: translate(-50%, -50%) scale(1);
        }
    </style>
</head>

<body>

    <div class="app-window" id="app">

        <!-- SCENE 1: PromptLayer Registry -->
        <div class="scene-container" id="scene-registry" style="display: flex; width: 100%; height: 100%;">
            <!-- Sidebar -->
            <div class="sidebar">
                <div class="logo">
                    <div class="logo-icon"></div>
                    PromptLayer
                </div>
                <div class="nav-item active"><span>📂</span> Registry</div>
                <div class="nav-item"><span>📊</span> Analytics</div>
                <div class="nav-item"><span>📜</span> History</div>
                <div class="nav-item"><span>⚙️</span> Settings</div>
            </div>

            <!-- Main Content -->
            <div class="main-content">

                <!-- Editor View -->
                <div class="view active" id="view-editor">
                    <div class="header">
                        <div class="breadcrumb">Registry / <span>Onboarding Email</span></div>
                        <div style="display: flex; gap: 10px;">
                            <button class="btn btn-secondary">History</button>
                            <button class="btn" id="btn-save">Save Version</button>
                        </div>
                    </div>
                    <div class="editor-layout" style="margin-top: 20px; padding: 0 24px;">
                        <div class="editor-left">
                            <div class="input-group">
                                <label class="label">TEMPLATE NAME</label>
                                <input type="text" class="input-field" value="Onboarding Email" readonly>
                            </div>
                            <div class="input-group" style="flex: 1;">
                                <label class="label">PROMPT TEMPLATE</label>
                                <textarea class="prompt-area"
                                    id="input-prompt">Write a professional welcome email to {name}. Keep it formal.</textarea>
                            </div>
                        </div>
                        <div class="editor-right">
                            <div class="input-group">
                                <label class="label">MODEL</label>
                                <select class="input-field">
                                    <option>gpt-4</option>
                                </select>
                            </div>
                            <div style="margin-top: 20px;">
                                <label class="label">VARIABLES</label>
                                <div style="margin-top: 10px;">
                                    <span
                                        style="background: #e0e7ff; color: var(--accent); padding: 4px 8px; border-radius: 4px; font-size: 12px;">name</span>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Code View (Hidden initially) -->
                <div class="view" id="view-code">
                    <div class="header">
                        <h1>Integration</h1>
                    </div>
                    <div style="padding: 40px;">
                        <h3 style="margin-bottom: 20px;">Use this prompt in your code</h3>
                        <div class="code-container">
                            <div><span class="code-keyword">import</span> promptlayer</div>
                            <br>
                            <div><span class="code-comment"># Fetch the latest version automatically</span></div>
                            <div>prompt = promptlayer.prompts.<span class="code-function">get</span>(<span
                                    class="code-string">"onboarding-email"</span>)</div>
                            <br>
                            <div><span class="code-comment"># Use with your LLM</span></div>
                            <div>response = openai.Completion.create(</div>
                            <div>&nbsp;&nbsp;prompt=prompt,</div>
                            <div>&nbsp;&nbsp;engine=<span class="code-string">"gpt-4"</span></div>
                            <div>)</div>
                        </div>
                        <p style="margin-top: 20px; color: var(--text-muted);">
                            ✨ No code changes required when you update the prompt in the registry.
                        </p>
                    </div>
                </div>

            </div>
        </div>

        <!-- SCENE 2: Word Plugin (Hidden initially) -->
        <div class="scene-container" id="scene-word" style="display: none; width: 100%; height: 100%;">
            <div class="word-app">
                <div class="word-ribbon">
                    <div>Word</div>
                    <div class="word-ribbon-item">File</div>
                    <div class="word-ribbon-item active">Home</div>
                    <div class="word-ribbon-item">Insert</div>
                    <div class="word-ribbon-item">Layout</div>
                </div>
                <div class="word-workspace">
                    <div class="word-doc-area">
                        <div class="word-paper" id="word-doc-content">
                            <!-- Content will be typed here -->
                        </div>
                    </div>
                    <div class="lucio-sidebar">
                        <div class="lucio-header">
                            <div style="width: 20px; height: 20px; background: var(--accent); border-radius: 4px;">
                            </div>
                            Lucio AI
                        </div>
                        <div class="lucio-body">
                            <div class="input-group">
                                <label class="label">Action</label>
                                <select class="input-field">
                                    <option>Generate Email</option>
                                    <option>Summarize</option>
                                </select>
                            </div>
                            <div class="input-group">
                                <label class="label">Recipient Name</label>
                                <input type="text" class="input-field" value="Sarah" id="word-input-name">
                            </div>
                            <div style="flex: 1;"></div>
                            <button class="btn" id="btn-word-generate" style="width: 100%;">Generate</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Overlays -->
        <div class="cursor" id="cursor"></div>
        <div class="guide-overlay" id="guide"></div>
        <div class="toast" id="toast">
            <span style="font-size: 20px;">✅</span>
            <div>
                <div style="font-weight: 600;">Saved Successfully</div>
                <div style="font-size: 12px; opacity: 0.8;">v2 is now live</div>
            </div>
        </div>
        <button class="replay-btn" id="replay-btn" onclick="location.reload()">Replay Demo</button>
    </div>

    <script>
        // --- Animation Engine ---
        const cursor = document.getElementById('cursor');
        const guide = document.getElementById('guide');

        function sleep(ms) { return new Promise(r => setTimeout(r, ms)); }

        async function moveCursorTo(selector, offsetX = 0, offsetY = 0, duration = 800) {
            const el = document.querySelector(selector);
            if (!el) return;
            const rect = el.getBoundingClientRect();
            const targetX = rect.left + rect.width / 2 + offsetX;
            const targetY = rect.top + rect.height / 2 + offsetY;

            cursor.style.transition = `transform ${duration}ms cubic-bezier(0.25, 1, 0.5, 1)`;
            cursor.style.transform = `translate(${targetX}px, ${targetY}px)`;

            await sleep(duration);
        }

        async function clickElement(selector) {
            const el = document.querySelector(selector);
            if (!el) return;

            const rect = el.getBoundingClientRect();
            const ripple = document.createElement('div');
            ripple.className = 'click-effect';
            ripple.style.left = (rect.left + rect.width / 2 - 20) + 'px';
            ripple.style.top = (rect.top + rect.height / 2 - 20) + 'px';
            document.body.appendChild(ripple);

            cursor.style.transform += ' scale(0.9)';
            await sleep(100);
            cursor.style.transform = cursor.style.transform.replace(' scale(0.9)', '');

            setTimeout(() => ripple.remove(), 500);
            await sleep(300);
        }

        async function typeText(selector, text, speed = 50, clear = false) {
            const el = document.querySelector(selector);
            if (!el) return;
            el.focus();
            if (clear) el.value = "";
            for (let char of text) {
                el.value += char;
                await sleep(speed + Math.random() * 30);
            }
            await sleep(500);
        }

        function showGuide(text) {
            guide.innerText = text;
            guide.style.opacity = 1;
        }

        function hideGuide() {
            guide.style.opacity = 0;
        }

        function switchScene(sceneId) {
            document.querySelectorAll('.scene-container').forEach(s => s.style.display = 'none');
            document.getElementById(sceneId).style.display = 'flex';
        }

        function switchView(viewId) {
            document.querySelectorAll('.view').forEach(v => v.classList.remove('active'));
            document.getElementById(viewId).classList.add('active');
        }

        // --- Demo Logic ---

        async function runDemo() {
            // Initial State
            await sleep(1000);

            // Step 1: Edit Prompt
            showGuide("Let's update our 'Onboarding Email' prompt.");
            await moveCursorTo('#input-prompt');
            await clickElement('#input-prompt');

            // Clear and type new prompt
            const el = document.getElementById('input-prompt');
            el.value = "";
            await typeText('#input-prompt', "Write a funny and witty welcome email to {name}. Make them laugh!");

            // Step 2: Save
            showGuide("Save the new version to the Registry.");
            await moveCursorTo('#btn-save');
            await clickElement('#btn-save');

            // Toast
            document.getElementById('toast').classList.add('show');
            await sleep(2000);
            document.getElementById('toast').classList.remove('show');

            // Step 3: Show Integration
            showGuide("How do we update the app? We don't.");
            switchView('view-code');
            await sleep(1000);
            showGuide("The code fetches the latest version automatically.");
            await sleep(3000);

            // Step 4: Switch to Word
            hideGuide();
            switchScene('scene-word');
            // Reset cursor position for new scene
            cursor.style.transition = 'none';
            cursor.style.transform = 'translate(100px, 100px)';
            await sleep(100);

            showGuide("Now let's see it in the Lucio AI Word Plugin.");
            await sleep(1000);

            // Step 5: Generate in Word
            await moveCursorTo('#btn-word-generate');
            await clickElement('#btn-word-generate');

            // Simulate Output
            showGuide("Generating with the NEW prompt...");
            const wordContent = document.getElementById('word-doc-content');
            const response = "Subject: Welcome to the party, Sarah! 🎉\n\nHey Sarah,\n\nYou finally made it! We were just about to send a search party. Welcome to the team!\n\nBuckle up, it's going to be a wild ride...";

            for (let char of response) {
                if (char === '\n') wordContent.innerHTML += '<br>';
                else wordContent.innerHTML += char;
                await sleep(20);
            }

            await sleep(1000);
            showGuide("The change is reflected instantly!");
            await sleep(2000);
            hideGuide();

            document.getElementById('replay-btn').classList.add('show');
        }

        // Start
        window.onload = () => {
            const app = document.getElementById('app');
            const rect = app.getBoundingClientRect();
            cursor.style.transform = `translate(${rect.left + rect.width / 2}px, ${rect.top + rect.height / 2}px)`;

            setTimeout(runDemo, 500);
        };

    </script>
</body>

</html>
