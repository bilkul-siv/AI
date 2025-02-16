# AI
My fast AI chat bot 

from flask import Flask, request, jsonify
import openai  

app = Flask(__name__)

# यहाँ अपने OpenAI API Key डालो
OPENAI_API_KEY = "your-api-key"

def get_ai_response(user_input):
    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",  # या कोई और मॉडल जैसे "gpt-4"
        messages=[{"role": "user", "content": user_input}],
        api_key=OPENAI_API_KEY,
    )
    return response["choices"][0]["message"]["content"]

@app.route("/chat", methods=["POST"])
def chat():
    data = request.get_json()
    user_message = data.get("message", "")
    if not user_message:
        return jsonify({"error": "Message cannot be empty"}), 400

    ai_response = get_ai_response(user_message)
    return jsonify({"response": ai_response})

if __name__ == "__main__":
    app.run(debug=True, port=5000)
