from flask import Flask, request, jsonify
import requests
import base64
import os

app = Flask(__name__)

HF_API_TOKEN = os.getenv("HF_API_TOKEN")

@app.route('/')
def index():
    return "Audio to Newsletter API is running!"

@app.route('/transcribe', methods=['POST'])
def transcribe():
    if 'audio' not in request.files:
        return jsonify({'error': 'No audio file provided'}), 400

    audio_file = request.files['audio']
    audio_bytes = audio_file.read()

    headers = {
        "Authorization": f"Bearer {HF_API_TOKEN}"
    }

    response = requests.post(
        "https://api-inference.huggingface.co/models/openai/whisper-large",
        headers=headers,
        data=audio_bytes
    )

    if response.status_code != 200:
        return jsonify({'error': 'Transcription failed', 'details': response.json()}), 500

    result = response.json()
    transcript = result.get('text', '')

    # Basic summary logic (for now, just truncates)
    summary = summarize(transcript)

    return jsonify({
        'transcript': transcript,
        'summary': summary
    })

def summarize(text):
    # Very simple "summary" logic (replace this with a real model later)
    if len(text) > 200:
        return text[:200] + '...'
    return text

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0', port=5000)
