<!doctype html>
<html>
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Breaking Buggs — AI Debugging Agent (Gemini)</title>
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">

<style>
  body{
    margin:0; padding:0;
    font-family:'Poppins',sans-serif;
    background: linear-gradient(135deg,#4e54c8,#8f94fb);
    display:flex; justify-content:center; align-items:center;
    min-height:100vh;
  }
  .app{
    width:650px; max-width:95%;
    backdrop-filter: blur(20px);
    background: rgba(255,255,255,0.15);
    border-radius:25px;
    padding:20px;
    box-shadow:0 10px 50px rgba(0,0,0,0.35);
  }
  .header{
    text-align:center; color:#fff; margin-bottom:15px;
  }
  .header .logo{
    font-size:48px; animation:spinLogo 3s linear infinite;
  }
  .header .title{
    font-size:28px; font-weight:700; margin-top:6px;
  }
  .messages{
    height:450px; overflow-y:auto;
    padding:15px; display:flex; flex-direction:column; gap:10px;
  }
  .msg{
    max-width:80%; padding:12px 14px; border-radius:18px;
  }
  .agent{background:#ffffffaa; color:#111; align-self:flex-start;}
  .user{background:#4e54c8; color:#fff; align-self:flex-end;}
  .controls{display:flex; margin-top:15px;}
  .controls input{
    flex:1; padding:12px; border-radius:12px; border:none; outline:none;
  }
  .controls button{
    margin-left:10px; padding:12px 18px; background:#111; color:#fff; border:none;
    border-radius:12px; cursor:pointer;
  }
</style>
</head>
<body>

<div class="app">
  <div class="header">
    <div class="logo">🌸</div>
    <div class="title">Breaking Buggs — AI Debugging Agent</div>
  </div>

  <div id="messages" class="messages"></div>

  <div class="controls">
    <input id="input" placeholder="Paste code or type a query..." />
    <button onclick="sendMsg()">Send</button>
  </div>
</div>

<script>
const API_KEY = "AIzaSyDo3z-JofXzSP33WKDNdp0r73MiuYa6k3s";  // <-- paste your Gemini key here
const MODEL  = "gemini-2.0-flash";      // FREE + FAST model

function addMsg(text, who="agent"){
  const div = document.createElement("div");
  div.className = `msg ${who}`;
  div.innerHTML = text;
  document.getElementById("messages").appendChild(div);
  document.getElementById("messages").scrollTop = document.getElementById("messages").scrollHeight;
}

async function sendMsg(){
  const input = document.getElementById("input");
  const text = input.value.trim();
  if(!text) return;

  addMsg(text, "user");
  input.value = "";

  try{
    const res = await fetch(
      `https://generativelanguage.googleapis.com/v1beta/models/${MODEL}:generateContent?key=${API_KEY}`,
      {
        method: "POST",
        headers: {"Content-Type":"application/json"},
        body: JSON.stringify({
          contents: [{ role:"user", parts:[{text:text}] }]
        })
      }
    );

    const data = await res.json();
    const reply = data?.candidates?.[0]?.content?.parts?.[0]?.text || "(No response)";
    addMsg(reply.replace(/\n/g,"<br>"), "agent");

  } catch (err){
    addMsg("❌ Error: " + err.message, "agent");
  }
}
</script>

</body>
</html>    if(!text) return;

    addMsg(text, "user");
    inp.value = "";

    // Payload for OpenAI API
    const payload = {
      model: "gpt-4o-mini",
      messages: [
        {role:"system", content:"You are an AI debugging assistant. Detect bugs, errors, vulnerabilities, and performance issues in any programming language. Provide the corrected, error-free code with explanations in a professional, friendly tone."},
        {role:"user", content:text}
      ]
    };

    try {
      const res = await fetch("https://api.openai.com/v1/chat/completions",{
        method:"POST",
        headers:{
          "Content-Type":"application/json",
          "Authorization":"Bearer "+API_KEY
        },
        body: JSON.stringify(payload)
      });
      const data = await res.json();
      const reply = data.choices?.[0]?.message?.content || "(No response)";
      agentReply(reply);
    } catch(err){
      agentReply("❌ Error connecting to OpenAI API: "+err.message);
    }
  }
</script>
</body>
</html>
