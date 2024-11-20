<!DOCTYPE html>
<html>
<head>
    <title>Healthcare Trading Demo</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 20px auto;
            padding: 20px;
            background-color: #f5f5f5;
        }
        .container {
            background-color: white;
            border: 1px solid #ddd;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
            margin-bottom: 20px;
        }
        .form-group {
            margin-bottom: 15px;
        }
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
            color: #333;
        }
        select, input[type="number"] {
            width: 100%;
            padding: 10px;
            margin-bottom: 10px;
            border: 1px solid #ddd;
            border-radius: 4px;
            font-size: 16px;
        }
        .radio-group {
            display: flex;
            gap: 20px;
            margin: 10px 0;
        }
        .radio-group label {
            display: flex;
            align-items: center;
            font-weight: normal;
        }
        button {
            padding: 12px 24px;
            margin: 5px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
            transition: all 0.3s ease;
        }
        button:hover {
            transform: translateY(-2px);
            box-shadow: 0 2px 4px rgba(0,0,0,0.2);
        }
        .btn-success {
            background-color: #28a745;
            color: white;
        }
        .btn-info {
            background-color: #17a2b8;
            color: white;
        }
        .btn-warning {
            background-color: #ffc107;
            color: black;
        }
        .output-area {
            background-color: white;
            padding: 20px;
            border-radius: 8px;
            margin-top: 20px;
            min-height: 200px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }
        .market-status {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 20px;
            margin-top: 15px;
        }
        .service-card {
            background-color: #f8f9fa;
            border: 1px solid #ddd;
            padding: 15px;
            border-radius: 8px;
            text-align: center;
            transition: all 0.3s ease;
        }
        .service-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
        }
        .trade-history {
            margin-top: 20px;
        }
        .trade-item {
            padding: 10px;
            border-bottom: 1px solid #ddd;
        }
        .price-change {
            font-weight: bold;
        }
        .price-up {
            color: #28a745;
        }
        .price-down {
            color: #dc3545;
        }
    </style>
</head>
<body>
    <h1>Healthcare Trading System Demo</h1>
    <p>Welcome! This demo shows how healthcare services can be traded like financial instruments.</p>
    
    <div class="container">
        <div class="form-group">
            <label for="service">Select Healthcare Service:</label>
            <select id="service">
                <option value="GC">General Checkup (GC) - Basic health assessment</option>
                <option value="DC">Dental Cleaning (DC) - Professional dental care</option>
                <option value="PT">Physical Therapy (PT) - Rehabilitation services</option>
            </select>
        </div>
        
        <div class="form-group">
            <label>Order Type:</label>
            <div class="radio-group">
                <label>
                    <input type="radio" id="buy" name="orderType" value="buy" checked>
                    Buy Service
                </label>
                <label>
                    <input type="radio" id="sell" name="orderType" value="sell">
                    Sell Service
                </label>
            </div>
        </div>
        
        <div class="form-group">
            <label for="quantity">Number of Sessions:</label>
            <input type="number" id="quantity" min="1" value="1">
        </div>
        
        <div class="form-group">
            <label for="price">Price per Session ($):</label>
            <input type="number" id="price" step="0.01" placeholder="Enter price...">
        </div>
        
        <button class="btn-success" onclick="placeOrder()">Place Order</button>
        <button class="btn-info" onclick="showMarketStatus()">Market Status</button>
        <button class="btn-warning" onclick="showTradeHistory()">Trade History</button>
    </div>
    
    <div class="output-area" id="output">
        <h3>Live Market Status</h3>
        <div class="market-status">
            <div class="service-card">
                <h4>General Checkup</h4>
                <p>Current Price: $150.00</p>
                <p class="price-change price-up">\u2191 2.3%</p>
            </div>
            <div class="service-card">
                <h4>Dental Cleaning</h4>
                <p>Current Price: $200.00</p>
                <p class="price-change price-down">\u2193 1.5%</p>
            </div>
            <div class="service-card">
                <h4>Physical Therapy</h4>
                <p>Current Price: $85.00</p>
                <p class="price-change price-up">\u2191 3.1%</p>
            </div>
        </div>
    </div>

    <script>
        let trades = [];
        let currentPrices = {
            GC: 150.00,
            DC: 200.00,
            PT: 85.00
        };

        function placeOrder() {
            const service = document.getElementById('service').value;
            const orderType = document.querySelector('input[name="orderType"]:checked').value;
            const quantity = parseInt(document.getElementById('quantity').value);
            const price = parseFloat(document.getElementById('price').value);
            
            if (!price || !quantity) {
                alert('Please enter valid price and quantity!');
                return;
            }

            // Add trade to history
            const trade = {
                service,
                orderType,
                quantity,
                price,
                timestamp: new Date().toLocaleString()
            };
            trades.push(trade);

            // Update market price (simplified simulation)
            const priceImpact = (orderType === 'buy' ? 1 : -1) * (quantity * 0.01);
            currentPrices[service] *= (1 + priceImpact);

            const output = document.getElementById('output');
            output.innerHTML = `
                <h3>Order Confirmation</h3>
                <div class="trade-item">
                    <p><strong>${orderType.toUpperCase()} Order Executed:</strong></p>
                    <p>Service: ${getServiceName(service)}</p>
                    <p>Quantity: ${quantity} session(s)</p>
                    <p>Price per Session: $${price.toFixed(2)}</p>
                    <p>Total Value: $${(price * quantity).toFixed(2)}</p>
                    <p>Time: ${trade.timestamp}</p>
                </div>
            `;
        }
        
        function showMarketStatus() {
            const output = document.getElementById('output');
            output.innerHTML = `
                <h3>Live Market Status</h3>
                <div class="market-status">
                    ${Object.entries(currentPrices).map(([service, price]) => `
                        <div class="service-card">
                            <h4>${getServiceName(service)}</h4>
                            <p>Current Price: $${price.toFixed(2)}</p>
                            <p class="price-change ${Math.random() > 0.5 ? 'price-up' : 'price-down'}">
                                ${Math.random() > 0.5 ? '\u2191' : '\u2193'} ${(Math.random() * 5).toFixed(1)}%
                            </p>
                        </div>
                    `).join('')}
                </div>
            `;
        }
        
        function showTradeHistory() {
            const output = document.getElementById('output');
            if (trades.length === 0) {
                output.innerHTML = `
                    <h3>Trade History</h3>
                    <p>No trades executed yet</p>
                `;
                return;
            }

            output.innerHTML = `
                <h3>Trade History</h3>
                ${trades.reverse().map(trade => `
                    <div class="trade-item">
                        <p><strong>${trade.orderType.toUpperCase()}</strong> - ${getServiceName(trade.service)}</p>
                        <p>Quantity: ${trade.quantity} | Price: $${trade.price.toFixed(2)}</p>
                        <p>Time: ${trade.timestamp}</p>
                    </div>
                `).join('')}
            `;
        }

        function getServiceName(code) {
            const names = {
                GC: 'General Checkup',
                DC: 'Dental Cleaning',
                PT: 'Physical Therapy'
            };
            return names[code];
        }

        // Update market prices periodically
        setInterval(() => {
            Object.keys(currentPrices).forEach(service => {
                const change = (Math.random() - 0.5) * 2;
                currentPrices[service] *= (1 + change * 0.01);
            });
        }, 5000);
    </script>
</body>
</html>- 👋 Hi, I’m @decranio
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
decranio/decranio is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
