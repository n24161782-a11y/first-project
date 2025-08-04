import nltk
import random
from nltk.stem import WordNetLemmatizer
from nltk.tokenize import word_tokenize

# Download necessary NLTK data
nltk.download('punkt')
nltk.download('wordnet')

# Initialize lemmatizer
lemmatizer = WordNetLemmatizer()

# Preprocessing function
def preprocess(sentence):
    tokens = word_tokenize(sentence.lower())
    return [lemmatizer.lemmatize(token) for token in tokens]

# Define intents and responses
intents = {
    "greeting": {
        "patterns": ["hello", "hi", "hey", "good morning", "good evening"],
        "responses": ["Hello!", "Hi there!", "Hey!", "Greetings!"]
    },
    "goodbye": {
        "patterns": ["bye", "see you", "goodbye", "take care"],
        "responses": ["Goodbye!", "See you later!", "Take care!"]
    },
    "thanks": {
        "patterns": ["thanks", "thank you", "thx"],
        "responses": ["You're welcome!", "No problem!", "Anytime!"]
    },
    "name": {
        "patterns": ["what is your name", "who are you"],
        "responses": ["I'm your friendly chatbot!", "You can call me Chatster."]
    }
}

# Function to get response based on user input
def get_response(user_input):
    user_tokens = preprocess(user_input)
    
    for intent, data in intents.items():
        for pattern in data["patterns"]:
            pattern_tokens = preprocess(pattern)
            if any(token in user_tokens for token in pattern_tokens):
                return random.choice(data["responses"])
    
    return "I'm not sure how to respond to that."

# Main chat loop
def chat():
    print("🤖 Chatbot: Hello! Type 'quit' to exit.")
    while True:
        user_input = input("You: ")
        if user_input.lower() == "quit":
            print("🤖 Chatbot: Bye!")
            break
        response = get_response(user_input)
        print("🤖 Chatbot:", response)

# Run the chatbot
if __name__ == "__main__":
    chat()

