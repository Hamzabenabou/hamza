from flask import Flask, request, jsonify
from firebase_admin import messaging, credentials, initialize_app

# Initialisation de Flask
app = Flask(__name__)

# Initialisation de Firebase Admin SDK pour les notifications push
cred = credentials.Certificate("path/to/firebase-credentials.json")
initialize_app(cred)

# Simuler une base de données en mémoire
users = [
    {"id": 1, "name": "User1", "criteria": "Xsara", "token": "user_fcm_token"}
]

# Endpoint pour ajouter un utilisateur avec ses critères
@app.route('/add_user', methods=['POST'])
def add_user():
    data = request.json
    users.append({
        "id": len(users) + 1,
        "name": data["name"],
        "criteria": data["criteria"],
        "token": data["token"]
    })
    return jsonify({"message": "User added successfully"}), 201

# Endpoint pour récupérer les utilisateurs
@app.route('/users', methods=['GET'])
def get_users():
    return jsonify(users)

# Fonction pour envoyer une notification
def send_notification(token, title, body, link):
    message = messaging.Message(
        notification=messaging.Notification(title=title, body=body),
        token=token,
        data={"link": link}
    )
    response = messaging.send(message)
    return response

# Fonction pour tester les notifications
@app.route('/test_notification', methods=['POST'])
def test_notification():
    data = request.json
    token = data["token"]
    title = data["title"]
    body = data["body"]
    link = data["link"]
    response = send_notification(token, title, body, link)
    return jsonify({"message": "Notification sent", "response": response}), 200

if __name__ == '__main__':
    app.run(debug=True)
