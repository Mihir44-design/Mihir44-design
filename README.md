from flask import Flask, request, jsonify
import openai
import os

# Initialize Flask app
app = Flask(__name__)

# Set your OpenAI API key
openai.api_key = 'YOUR_OPENAI_API_KEY'  # Replace with your actual API key

@app.route('/chat', methods=['POST'])
def chat():
    user_input = request.json.get('message')
    
    try:
        response = openai.ChatCompletion.create(
            model="gpt-3.5-turbo",  # You can choose a different model if desired
            messages=[
                {"role": "user", "content": user_input}
            ]
        )
        
        reply = response.choices[0].message['content']
        return jsonify({"reply": reply})
    except Exception as e:
        return jsonify({"error": str(e)}), 500

if __name__ == '__main__':
    app.run(debug=True)
