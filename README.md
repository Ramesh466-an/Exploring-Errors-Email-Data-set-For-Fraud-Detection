# Exploring-Errors-Email-Data-set-For-Fraud-Detection
Exploring Errors Email Data set For Fraud Detection Description
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Email Fraud Detector</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f3f4f6;
        }
        .container {
            max-width: 800px;
        }
        .card {
            background-color: #ffffff;
            border-radius: 1rem;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            padding: 2rem;
        }
    </style>
</head>
<body class="bg-gray-100 min-h-screen flex items-center justify-center py-12 px-4 sm:px-6 lg:px-8">
    <div class="container mx-auto">
        <h1 class="text-center text-3xl sm:text-4xl font-bold text-gray-800 mb-8">Email Fraud Detection Tool</h1>

        <div class="card space-y-6">
            <!-- Email Input Section -->
            <div class="mb-4">
                <label for="emailInput" class="block text-gray-700 font-semibold mb-2">Paste Email Content Here:</label>
                <textarea id="emailInput" class="w-full h-48 p-4 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-red-500 transition duration-200" placeholder="Enter the full content of the email you want to analyze..."></textarea>
            </div>

            <!-- Analyze Button -->
            <button id="analyzeBtn" class="w-full bg-red-600 text-white font-medium py-3 px-6 rounded-lg hover:bg-red-700 transition-colors duration-200 focus:outline-none focus:ring-2 focus:ring-red-500 focus:ring-offset-2">
                Analyze for Fraud
            </button>

            <!-- Results Display Section -->
            <div id="resultsContainer" class="mt-6 p-4 rounded-lg border border-gray-200 text-gray-700 font-semibold hidden">
                <p id="resultText" class="text-center text-lg"></p>
                <div id="fraudReasons" class="mt-4 text-sm text-gray-600"></div>
            </div>
            
            <!-- Example Email for easy testing -->
            <div class="mt-6 p-4 bg-gray-50 border border-gray-200 rounded-lg text-sm text-gray-500">
                <h3 class="font-semibold mb-2">Example Fraudulent Email:</h3>
                <pre class="whitespace-pre-wrap">Subject: URGENT: Your Account Has Been Compromised

Dear User,

We have detected unusual activity on your account. To prevent unauthorized access, you must click on the following link immediately to verify your details and avoid account suspension.

<a href="http://suspicious-link.com/verify">http://suspicious-link.com/verify</a>

Failure to do so will result in the permanent suspension of your account. Please act now.

Sincerely,
The Account Security Team</pre>
            </div>
        </div>
    </div>

    <script>
        // Define a simple knowledge base of fraudulent keywords and phrases.
        // In a real-world scenario, this would be a much larger, dynamic dataset
        // and would be used with a more sophisticated algorithm or ML model.
        const fraudulentKeywords = {
            phishing: [
                "click here", "verify your account", "urgent", "immediate action",
                "your account has been compromised", "suspicious activity", "security alert"
            ],
            financial: [
                "wire transfer", "payment", "bank account", "credit card",
                "invoice", "money", "funds"
            ],
            impersonation: [
                "account suspension", "account locked", "password reset",
                "unauthorized login", "security team", "it department"
            ]
        };

        // DOM elements
        const emailInput = document.getElementById('emailInput');
        const analyzeBtn = document.getElementById('analyzeBtn');
        const resultsContainer = document.getElementById('resultsContainer');
        const resultText = document.getElementById('resultText');
        const fraudReasons = document.getElementById('fraudReasons');

        // Function to analyze the email content
        function analyzeEmail() {
            const emailContent = emailInput.value.toLowerCase();
            const foundReasons = [];

            // Check for keywords and add reasons to the list
            for (const category in fraudulentKeywords) {
                const keywords = fraudulentKeywords[category];
                keywords.forEach(keyword => {
                    if (emailContent.includes(keyword)) {
                        foundReasons.push(`• Contains keywords related to ${category}: "${keyword}"`);
                    }
                });
            }
            
            // Check for common signs of phishing, such as links with mismatched text or domain.
            const urlRegex = /(https?:\/\/[^\s]+)/g;
            const foundUrls = emailContent.match(urlRegex);
            if (foundUrls && foundUrls.length > 0) {
                 foundReasons.push(`• Contains suspicious links.`);
            }

            // Display results
            resultsContainer.classList.remove('hidden');
            fraudReasons.innerHTML = ''; // Clear previous results

            if (foundReasons.length > 0) {
                resultText.textContent = "FRAUD DETECTED";
                resultText.className = "text-center text-lg text-red-600 font-bold";
                foundReasons.forEach(reason => {
                    const li = document.createElement('li');
                    li.textContent = reason;
                    fraudReasons.appendChild(li);
                });
            } else {
                resultText.textContent = "No obvious signs of fraud detected.";
                resultText.className = "text-center text-lg text-green-600 font-bold";
            }
        }

        // Add event listener to the analyze button
        analyzeBtn.addEventListener('click', analyzeEmail);

    </script>
</body>
</html>
