# steno.hindi
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hindi Shorthand Audio Detection</title>
    <style>
        body { font-family: Arial, sans-serif; padding: 20px; background: #f2f2f2; }
        h1 { color: #333; }
        #output { background: #fff; padding: 10px; border: 1px solid #ccc; margin-top: 20px; }
        button { padding: 10px 15px; margin-left: 5px; cursor: pointer; }
        input[type="file"] { margin: 10px 0; }
        textarea { width: 100%; height: 200px; }
    </style>
</head>
<body>

<h1>Hindi Shorthand Audio Detection & Transcription</h1>

<button onclick="startListening()">🎤 Start Dictation</button>
<input type="file" id="fileInput" accept=".txt">
<button onclick="compareText()">Submit for Comparison</button>

<h2>Detected Audio:</h2>
<textarea id="detectedText" readonly></textarea>

<h2>Your Transcription (Kruti Dev 010):</h2>
<textarea id="uploadedText" placeholder="Paste your Hindi transcription here"></textarea>

<h2>Comparison Result:</h2>
<div id="output"></div>

<script>
    const detectedTextArea = document.getElementById("detectedText");
    const uploadedTextArea = document.getElementById("uploadedText");
    const output = document.getElementById("output");

    let detectedText = "";
    
    // Function to start speech recognition
    function startListening() {
        const recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
        recognition.lang = "hi-IN"; // Hindi Language
        recognition.start();
        
        recognition.onresult = function(event) {
            detectedText = event.results[0][0].transcript;
            detectedTextArea.value = detectedText; // Show detected text in the text area
        };

        recognition.onerror = function(event) {
            alert("Speech recognition error: " + event.error);
        };
    }

    // Function to compare the detected text with uploaded transcription
    function compareText() {
        const uploadedText = uploadedTextArea.value.trim();
        const comparisonResult = compareStrings(detectedText.trim(), uploadedText);
        output.innerHTML = comparisonResult;
    }

    // Function to compare two strings and highlight mistakes
    function compareStrings(str1, str2) {
        let mistakes = 0;
        const maxLength = Math.max(str1.length, str2.length);
        let resultHTML = "<h3>Comparison Result:</h3><ul>";
        
        for (let i = 0; i < maxLength; i++) {
            const char1 = str1[i] || "";
            const char2 = str2[i] || "";
            
            if (char1 !== char2) {
                mistakes++;
                resultHTML += `<li style="color: red;">Mismatch at position ${i + 1}: <b>${char1}</b> vs <b>${char2}</b></li>`;
            }
        }

        resultHTML += `</ul><p>Total mistakes: ${mistakes}</p>`;
        return resultHTML;
    }

    // Function to handle file upload and read the contents of the uploaded .txt file
    document.getElementById('fileInput').addEventListener('change', handleFileUpload);

    function handleFileUpload(event) {
        const file = event.target.files[0];
        const reader = new FileReader();
        
        reader.onload = function(e) {
            uploadedTextArea.value = e.target.result;  // Fill the transcription textarea with file content
        };

        if (file) {
            reader.readAsText(file);
        }
    }
</script>

</body>
</html>
