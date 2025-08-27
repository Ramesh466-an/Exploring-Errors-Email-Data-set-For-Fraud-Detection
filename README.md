# Email Fraud Detection Tool (Python)
import re

# A simple knowledge base of fraudulent keywords and phrases.
# In a real-world scenario, this would be a much larger, dynamic dataset
# and would be used with a more sophisticated machine learning model.
fraudulent_keywords = {
    "phishing": [
        "click here", "verify your account", "urgent", "immediate action",
        "your account has been compromised", "suspicious activity", "security alert"
    ],
    "financial": [
        "wire transfer", "payment", "bank account", "credit card",
        "invoice", "money", "funds"
    ],
    "impersonation": [
        "account suspension", "account locked", "password reset",
        "unauthorized login", "security team", "it department"
    ]
}

def analyze_email_for_fraud(email_content):
    """
    Analyzes an email for potential fraud based on a list of keywords.

    Args:
        email_content (str): The full text content of the email.

    Returns:
        dict: A dictionary containing the analysis result and reasons.
    """
    email_content_lower = email_content.lower()
    found_reasons = []

    # Check for keywords from our predefined knowledge base
    for category, keywords in fraudulent_keywords.items():
        for keyword in keywords:
            if keyword in email_content_lower:
                found_reasons.append(f"Contains keywords related to {category}: \"{keyword}\"")

    # Check for suspicious links (a simple pattern check)
    # This regex looks for URLs
    url_regex = r"https?:\/\/[^\s]+"
    if re.search(url_regex, email_content_lower):
        found_reasons.append("Contains suspicious links.")

    # Return the analysis result
    if len(found_reasons) > 0:
        return {
            "is_fraud": True,
            "message": "FRAUD DETECTED",
            "reasons": found_reasons
        }
    else:
        return {
            "is_fraud": False,
            "message": "No obvious signs of fraud detected.",
            "reasons": []
        }

if __name__ == "__main__":
    # Example 1: A fraudulent email
    fraud_email = """
    Subject: URGENT: Your Account Has Been Compromised

    Dear User,

    We have detected suspicious activity on your account. To prevent unauthorized access,
    you must click on the following link immediately to verify your details and avoid
    account suspension.

    https://suspicious-link.com/verify

    Failure to do so will result in the permanent suspension of your account. Please act now.

    Sincerely,
    The Account Security Team
    """

    print("--- Analyzing Fraudulent Email ---")
    fraud_result = analyze_email_for_fraud(fraud_email)
    print(f"Result: {fraud_result['message']}")
    if fraud_result['is_fraud']:
        print("Reasons found:")
        for reason in fraud_result['reasons']:
            print(f"- {reason}")
    print("\n" + "="*30 + "\n")

    # Example 2: A legitimate email
    legit_email = """
    Subject: Meeting Tomorrow

    Hi,

    Just a friendly reminder about our meeting tomorrow at 10 AM.
    Please bring the project notes.

    Thanks,
    John
    """

    print("--- Analyzing Legitimate Email ---")
    legit_result = analyze_email_for_fraud(legit_email)
    print(f"Result: {legit_result['message']}")
    if legit_result['is_fraud']:
        print("Reasons found:")
        for reason in legit_result['reasons']:
            print(f"- {reason}")

