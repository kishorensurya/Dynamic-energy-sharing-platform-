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
index.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Energy Sharing Platform</title>
</head>
<body>
    <h1>Energy Sharing Platform</h1>
    <form id="share-form">
        <input type="text" id="user" placeholder="User" required>
        <input type="number" id="amount" placeholder="Amount" required>
        <button type="submit">Share Energy</button>
    </form>
    <ul id="shares-list"></ul>
    <script src="script.js"></script>
</body>
</html>
```

script.js
```javascript
document.getElementById('share-form').addEventListener('submit', async (e) => {
    e.preventDefault();
    const user = document.getElementById('user').value;
    const amount = document.getElementById('amount').value;

    await fetch('/share', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ user, amount })
    });

    document.getElementById('user').value = '';
    document.getElementById('amount').value = '';
    loadShares();
});

async function loadShares() {
    const response = await fetch('/shares');
    const shares = await response.json();
    const list = document.getElementById('shares-list');
    list.innerHTML = '';
    shares.forEach(share => {
        const item = document.createElement('li');
        item.textContent = `${share.user} shared ${share.amount} energy.`;
        list.appendChild(item);
    });
}

loadShares();
```

 4. Instructions
1. Create the directory structure.
2. Save the code snippets in the appropriate files.
3. Install Flask with `pip install -r requirements.txt`.
4. Run the backend with `python app.py`.
5. Open `index.html` in a browser.
