1. Project Structure
```
energy-sharing-platform/
├── backend/
│   ├── app.py
│   ├── requirements.txt
│   └── models.py
└── frontend/
    ├── index.html
    └── script.js
```

 2. Backend Code (Flask)
app.py
```python
from flask import Flask, jsonify, request
from models import EnergyShare

app = Flask(__name__)

energy_shares = []

@app.route('/share', methods=['POST'])
def share_energy():
    data = request.json
    energy_share = EnergyShare(data['user'], data['amount'])
    energy_shares.append(energy_share)
    return jsonify({"message": "Energy shared successfully!"}), 201

@app.route('/shares', methods=['GET'])
def get_shares():
    return jsonify([share.to_dict() for share in energy_shares])

if __name__ == '__main__':
    app.run(debug=True)
```

models.py
```python
class EnergyShare:
    def __init__(self, user, amount):
        self.user = user
        self.amount = amount

    def to_dict(self):
        return {'user': self.user, 'amount': self.amount}
```

requirements.txt
```
Flask
```

 3. Frontend Code
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dynamic Energy Sharing Platform</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>

    <!-- Login Page -->
    <div id="login-page" class="login-page">
        <h1>Login</h1>
        <input type="text" id="username" placeholder="Enter Username">
        <button class="cta-btn" onclick="login()">Login</button>
    </div>

    <!-- Dashboard Selection -->
    <div id="dashboard-selection" class="hidden">
        <h2>Choose Your Dashboard</h2>
        <button class="dashboard-btn buyer-btn" onclick="showDashboard('buyer')">Buyer Dashboard</button>
        <button class="dashboard-btn seller-btn" onclick="showDashboard('seller')">Seller Dashboard</button>
    </div>

    <!-- Buyer Dashboard -->
    <div id="buyer-dashboard" class="hidden dashboard">
        <h2>Buyer Dashboard</h2>
        <div class="dashboard-content">
            <img src="https://example.com/buyer-energy.jpg" alt="Buyer Energy Image" class="dashboard-img">
            <p>Welcome, <span id="buyer-username"></span>! Find nearby energy sources to purchase.</p>
            <ul id="buyer-connections-list"></ul>
            <button class="cta-btn" onclick="findBuyerConnections()">Find Energy Providers</button>
        </div>
    </div>

    <!-- Seller Dashboard -->
    <div id="seller-dashboard" class="hidden dashboard">
        <h2>Seller Dashboard</h2>
        <div class="dashboard-content">
            <img src="https://example.com/seller-energy.jpg" alt="Seller Energy Image" class="dashboard-img">
            <p>Welcome, <span id="seller-username"></span>! Share your available energy with others.</p>
            <ul id="seller-connections-list"></ul>
            <button class="cta-btn" onclick="findSellerConnections()">Find Energy Consumers</button>
        </div>
    </div>
    
    <script src="scripts.js"></script>
</body>
</html>
<style>
    /* General Styling */
body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    background-color: #f4f4f4;
    color: #333;
    text-align: center;
}

/* Login Page */
.login-page {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background-color: #5cb85c;
    color: white;
}

.login-page h1 {
    font-size: 2.5rem;
}

input {
    padding: 10px;
    margin: 20px 0;
    width: 250px;
    border: none;
    border-radius: 5px;
}

.cta-btn {
    background-color: #333;
    color: #fff;
    padding: 10px 20px;
    border: none;
    cursor: pointer;
    font-size: 1rem;
}

.cta-btn:hover {
    background-color: #555;
}

/* Dashboard Selection */
.hidden {
    display: none;
}
#dashboard-selection {
    padding: 50px;
    background-color: #f9f9f9;
}

.dashboard-btn {
    padding: 15px 30px;
    margin: 20px;
    font-size: 1.2rem;
    border: none;
    cursor: pointer;
    border-radius: 10px;
    transition: background-color 0.3s ease;
}

.buyer-btn {
    background-color: #5cb85c;
    color: white;
}

.buyer-btn:hover {
    background-color: #4a994a;
}

.seller-btn {
    background-color: #0275d8;
    color: white;
}

.seller-btn:hover {
    background-color: #025aa5;
}

/* Buyer and Seller Dashboard */
.dashboard {
    padding: 50px;
    background-color: #fff;
}

.dashboard-content {
    display: flex;
    flex-direction: column;
    align-items: center;
    max-width: 600px;
    margin: auto;
}

.dashboard-content img {
    width: 300px;
    height: auto;
    margin-bottom: 20px;
}

.dashboard ul {
    list-style-type: none;
    padding: 0;
}

.dashboard ul li {
    background-color: #ddd;
    padding: 10px;
    margin: 10px 0;
    border-radius: 5px;
}
</style>
<script>
// Login function
function login() {
    const username = document.getElementById('username').value;
    if (username) {
        // Hide login page
        document.getElementById('login-page').classList.add('hidden');
        // Show dashboard selection
        document.getElementById('dashboard-selection').classList.remove('hidden');
    } else {
        alert("Please enter a username");
    }
}

// Show selected dashboard
function showDashboard(type) {
    // Hide dashboard selection
    document.getElementById('dashboard-selection').classList.add('hidden');

    // Show appropriate dashboard based on user selection
    if (type === 'buyer') {
        document.getElementById('buyer-username').textContent = document.getElementById('username').value;
        document.getElementById('buyer-dashboard').classList.remove('hidden');
    } else if (type === 'seller') {
        document.getElementById('seller-username').textContent = document.getElementById('username').value;
        document.getElementById('seller-dashboard').classList.remove('hidden');
    }
}

// Find Buyer Connections (Dummy Data)
function findBuyerConnections() {
    const buyerConnectionsList = document.getElementById('buyer-connections-list');
    const connections = [
        'Energy Provider 1 (500 meters away)',
        'Energy Provider 2 (1 km away)',
        'Energy Provider 3 (1.5 km away)'
    ];

    buyerConnectionsList.innerHTML = ''; // Clear previous connections
    connections.forEach(connection => {
        const li = document.createElement('li');
        li.textContent = connection;
        buyerConnectionsList.appendChild(li);
    });
}

// Find Seller Connections (Dummy Data)
function findSellerConnections() {
    const sellerConnectionsList = document.getElementById('seller-connections-list');
    const connections = [
        'Energy Consumer 1 (300 meters away)',
        'Energy Consumer 2 (700 meters away)',
        'Energy Consumer 3 (1.2 km away)'
    ];

    sellerConnectionsList.innerHTML = ''; // Clear previous connections
    connections.forEach(connection => {
        const li = document.createElement('li');
        li.textContent = connection;
        sellerConnectionsList.appendChild(li);
    });
}
</script>
