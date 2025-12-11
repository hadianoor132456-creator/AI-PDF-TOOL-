# AI-PDF-TOOL-
 My AI Tool PDF
from flask import Flask, request, render_template, jsonify
import pdfplumber
import openai

app = Flask(__name__, template_folder="../frontend")
openai.api_key = "PASTE_YOUR_OPENAI_KEY_HERE"

@app.route('/')
def index():
    return render_template("index.html")

@app.route('/upload', methods=['POST'])
def upload_pdf():
    file = request.files['pdf']
    with pdfplumber.open(file) as pdf:
        text = "\n".join(page.extract_text() or "" for page in pdf.pages)
    return jsonify({"text": text})

@app.route('/ask', methods=['POST'])
def ask():
    data = request.json
    text = data.get("text")
    question = data.get("question")
    response = openai.chat.completions.create(
        model="gpt-4o-mini",
        messages=[
            {"role":"system","content":"You are an AI for PDF Q&A."},
            {"role":"user","content":f"PDF: {text}\nQuestion: {question}"}
        ]
    )
    return jsonify({"answer": response.choices[0].message['content']})

if __name__ == '__main__':
    app.run(host="0.0.0.0", port=800
text
Copy code
Flask
pdfplumber
openai

html
Copy code
<!DOCTYPE html>
<html>
<head>
<title>AI PDF Tool</title>
<meta name="viewport" content="width=device-width, initial-scale=1">
<style>
body{font-family:Arial;margin:20px;max-width:600px}
textarea{width:100%;height:150px}
</style>
</head>
<body>
<h2>AI PDF Tool</h2>
<input type="file" id="pdfFile">
<button onclick="uploadPDF()">Upload PDF</button>
<hr>
<textarea id="pdfText"></textarea><br>
<input id="question" placeholder="Ask question"><button onclick="askAI()">Ask</button>
<p id="answer"></p>

<script>
const BACKEND_URL = "" // Set backend URL after Render deployment
function uploadPDF(){
 let f=document.getElementById('pdfFile').files[0];
 if(!f) return alert('Select PDF')
 let fd=new FormData(); fd.append('pdf',f);
 fetch(BACKEND_URL+'/upload',{method:'POST',body:fd})
 .then(r=>r.json()).then(d=> pdfText.value=d.text);
}
function askAI(){
 fetch(BACKEND_URL+'/ask',{
  method:'POST',
  headers:{'Content-Type':'application/json'},
  body:JSON.stringify({text:pdfText.value,question:question.value})
 }).then(r=>r.json()).then(d=> answer.innerText=d.answer);
}
</script>
</body>
</html>



gitignore
Copy code
# Python
__pycache__/
*.pyc
*.pyo
*.pyd
env/
venv/
*.env

# VS Code
.vscode/

# OS
.DS_Store 























ChatGPT can make mistakes
