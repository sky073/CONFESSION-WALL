# CONFESSION-WALL

# 🤫 Anonymous Confession Wall

A simple, lightweight web application where users can share their secrets, thoughts, or stories anonymously. There are no accounts, no profiles, and no judgments.

## ✨ Features
* **Total Anonymity:** No login required. No names are stored.
* **Real-time Sharing:** Post your thoughts and see them appear on the wall instantly.
* **Clean UI:** Simple, distraction-free design for reading and writing.
* **Mobile Friendly:** Works on phones, tablets, and desktops.

## 🚀 How to Use
1.  Visit the website link.
2.  Type your confession in the text box.
3.  Click **"Post Anonymously"**.
4.  Scroll down to see what others have shared!

## 🛠️ Technical Setup
* **Frontend:** HTML5, CSS3, and Vanilla JavaScript.
* **Storage:** Currently uses `localStorage` (browser-based). 
    * *Note: To share confessions across different devices, a backend like Firebase or Supabase is required.*

## 📜 Rules & Guidelines
To keep this community safe and helpful:
1.  **Be Kind:** No bullying, harassment, or hate speech.
2.  **No Doxing:** Do not share real names, phone numbers, or private info of yourself or others.
3.  **No Spam:** Please don't flood the wall with the same message.

## 📄 License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.



<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Duscian Confessions</title>
    @import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@900&family=Inter:wght@400;600&display=swap');

    <style>
        :root {
            --primary: #6c5ce7;
            --text-dark: #2d3436;
            --bg-light: #ffffff;
            --border: #edf2f7;
        }

        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--bg-light);
            color: var(--text-dark);
            margin: 0;
            padding: 0;
            line-height: 1.6;
        }

        .container {
            max-width: 700px;
            margin: 0 auto;
            padding: 40px 20px;
        }

        header {
            text-align: center;
            margin-bottom: 50px;
        }

        header h1 {
            font-family: 'Montserrat', sans-serif;
            font-size: 3.5rem;
            font-weight: 900;
            color: var(--text-dark);
            text-transform: uppercase;
            letter-spacing: -2px;
            margin: 0;
        }

        header h1 span {
            color: var(--primary);
        }

        header p {
            font-weight: 600;
            color: #b2bec3;
            letter-spacing: 1px;
            margin-top: 10px;
        }

        /* Input Styling */
        .post-box {
            background: #fff;
            border: 2px solid var(--border);
            border-radius: 16px;
            padding: 24px;
            margin-bottom: 40px;
            transition: border-color 0.3s ease;
        }

        .post-box:focus-within {
            border-color: var(--primary);
        }

        textarea {
            width: 100%;
            height: 120px;
            border: none;
            outline: none;
            font-size: 1.1rem;
            resize: none;
            font-family: inherit;
        }

        .post-footer {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-top: 15px;
            padding-top: 15px;
            border-top: 1px solid var(--border);
        }

        button {
            background: var(--primary);
            color: white;
            border: none;
            padding: 12px 24px;
            border-radius: 12px;
            font-weight: 700;
            cursor: pointer;
            transition: transform 0.2s, opacity 0.2s;
        }

        button:hover {
            transform: translateY(-2px);
            opacity: 0.9;
        }

        /* Confession Cards */
        .wall {
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        .card {
            background: #fff;
            border: 1px solid var(--border);
            border-radius: 16px;
            padding: 24px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.02);
            animation: slideUp 0.4s ease-out;
        }

        @keyframes slideUp {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .card-header {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-bottom: 12px;
        }

        .avatar {
            width: 35px;
            height: 35px;
            background: var(--border);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.2rem;
        }

        .username {
            font-weight: 700;
            font-size: 0.95rem;
            color: var(--primary);
        }

        .card-content {
            font-size: 1.05rem;
            color: #4a4a4a;
            word-wrap: break-word;
        }

        .card-time {
            font-size: 0.8rem;
            color: #a0aec0;
            margin-top: 15px;
            display: block;
        }
    </style>
</head>
<body>

<div class="container">
    <header>
        <h1>DUSCIAN <span>WALL</span></h1>
        <p>SPEAK YOUR TRUTH ANONYMOUSLY</p>
    </header>

    <div class="post-box">
        <textarea id="confessionInput" placeholder="What's the tea, Duscian?"></textarea>
        <div class="post-footer">
            <small style="color: #a0aec0;">Your identity is hidden.</small>
            <button onclick="addConfession()">Post Confession</button>
        </div>
    </div>

    <div id="wall" class="wall">
        </div>
</div>

<script>
    const adjectives = ["Secret", "Mysterious", "Silent", "Brave", "Hidden", "Shadow", "Wandering"];
    const names = ["Duscian", "Scholar", "Dreamer", "Thinker", "Peer"];

    function generateAnonName() {
        const adj = adjectives[Math.floor(Math.random() * adjectives.length)];
        const name = names[Math.floor(Math.random() * names.length)];
        return `${adj} ${name}`;
    }

    // Load from storage
    window.onload = () => {
        const saved = JSON.parse(localStorage.getItem('duscian_confessions') || '[]');
        saved.forEach(c => renderCard(c));
    };

    function addConfession() {
        const text = document.getElementById('confessionInput').value.trim();
        if(!text) return alert("Write something first!");

        const confession = {
            id: Date.now(),
            user: generateAnonName(),
            content: text,
            time: new Date().toLocaleDateString() + ' ' + new Date().toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'})
        };

        const saved = JSON.parse(localStorage.getItem('duscian_confessions') || '[]');
        saved.push(confession);
        localStorage.setItem('duscian_confessions', JSON.stringify(saved));

        renderCard(confession, true);
        document.getElementById('confessionInput').value = '';
    }

    function renderCard(c, isNew = false) {
        const wall = document.getElementById('wall');
        const card = document.createElement('div');
        card.className = 'card';
        card.innerHTML = `
            <div class="card-header">
                <div class="avatar">👤</div>
                <div class="username">${c.user}</div>
            </div>
            <div class="card-content">${c.content}</div>
            <span class="card-time">${c.time}</span>
        `;
        
        if(isNew) {
            wall.prepend(card);
        } else {
            wall.appendChild(card);
        }
    }
</script>

</body>
</html>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Duscian Confessions</title>
    @import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@900&family=Inter:wght@400;600&display=swap');

    <style>
        :root {
            --primary: #6c5ce7;
            --text-dark: #2d3436;
            --bg-light: #ffffff;
            --border: #edf2f7;
        }

        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--bg-light);
            color: var(--text-dark);
            margin: 0;
            padding: 0;
            line-height: 1.6;
        }

        .container {
            max-width: 700px;
            margin: 0 auto;
            padding: 40px 20px;
        }

        header {
            text-align: center;
            margin-bottom: 50px;
        }

        header h1 {
            font-family: 'Montserrat', sans-serif;
            font-size: 3.5rem;
            font-weight: 900;
            color: var(--text-dark);
            text-transform: uppercase;
            letter-spacing: -2px;
            margin: 0;
        }

        header h1 span {
            color: var(--primary);
        }

        header p {
            font-weight: 600;
            color: #b2bec3;
            letter-spacing: 1px;
            margin-top: 10px;
        }

        /* Input Styling */
        .post-box {
            background: #fff;
            border: 2px solid var(--border);
            border-radius: 16px;
            padding: 24px;
            margin-bottom: 40px;
            transition: border-color 0.3s ease;
        }

        .post-box:focus-within {
            border-color: var(--primary);
        }

        textarea {
            width: 100%;
            height: 120px;
            border: none;
            outline: none;
            font-size: 1.1rem;
            resize: none;
            font-family: inherit;
        }

        .post-footer {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-top: 15px;
            padding-top: 15px;
            border-top: 1px solid var(--border);
        }

        button {
            background: var(--primary);
            color: white;
            border: none;
            padding: 12px 24px;
            border-radius: 12px;
            font-weight: 700;
            cursor: pointer;
            transition: transform 0.2s, opacity 0.2s;
        }

        button:hover {
            transform: translateY(-2px);
            opacity: 0.9;
        }

        /* Confession Cards */
        .wall {
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        .card {
            background: #fff;
            border: 1px solid var(--border);
            border-radius: 16px;
            padding: 24px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.02);
            animation: slideUp 0.4s ease-out;
        }

        @keyframes slideUp {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .card-header {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-bottom: 12px;
        }

        .avatar {
            width: 35px;
            height: 35px;
            background: var(--border);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.2rem;
        }

        .username {
            font-weight: 700;
            font-size: 0.95rem;
            color: var(--primary);
        }

        .card-content {
            font-size: 1.05rem;
            color: #4a4a4a;
            word-wrap: break-word;
        }

        .card-time {
            font-size: 0.8rem;
            color: #a0aec0;
            margin-top: 15px;
            display: block;
        }
    </style>
</head>
<body>

<div class="container">
    <header>
        <h1>DUSCIAN <span>WALL</span></h1>
        <p>SPEAK YOUR TRUTH ANONYMOUSLY</p>
    </header>

    <div class="post-box">
        <textarea id="confessionInput" placeholder="What's the tea, Duscian?"></textarea>
        <div class="post-footer">
            <small style="color: #a0aec0;">Your identity is hidden.</small>
            <button onclick="addConfession()">Post Confession</button>
        </div>
    </div>

    <div id="wall" class="wall">
        </div>
</div>

<script>
    const adjectives = ["Secret", "Mysterious", "Silent", "Brave", "Hidden", "Shadow", "Wandering"];
    const names = ["Duscian", "Scholar", "Dreamer", "Thinker", "Peer"];

    function generateAnonName() {
        const adj = adjectives[Math.floor(Math.random() * adjectives.length)];
        const name = names[Math.floor(Math.random() * names.length)];
        return `${adj} ${name}`;
    }

    // Load from storage
    window.onload = () => {
        const saved = JSON.parse(localStorage.getItem('duscian_confessions') || '[]');
        saved.forEach(c => renderCard(c));
    };

    function addConfession() {
        const text = document.getElementById('confessionInput').value.trim();
        if(!text) return alert("Write something first!");

        const confession = {
            id: Date.now(),
            user: generateAnonName(),
            content: text,
            time: new Date().toLocaleDateString() + ' ' + new Date().toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'})
        };

        const saved = JSON.parse(localStorage.getItem('duscian_confessions') || '[]');
        saved.push(confession);
        localStorage.setItem('duscian_confessions', JSON.stringify(saved));

        renderCard(confession, true);
        document.getElementById('confessionInput').value = '';
    }

    function renderCard(c, isNew = false) {
        const wall = document.getElementById('wall');
        const card = document.createElement('div');
        card.className = 'card';
        card.innerHTML = `
            <div class="card-header">
                <div class="avatar">👤</div>
                <div class="username">${c.user}</div>
            </div>
            <div class="card-content">${c.content}</div>
            <span class="card-time">${c.time}</span>
        `;
        
        if(isNew) {
            wall.prepend(card);
        } else {
            wall.appendChild(card);
        }
    }
</script>

</body>
</html>
