# Chatbot Setup Instructions

Follow these simple steps to get the AI chatbot up and running.

## Step 1: Install Dependencies

Run this command in your terminal:

```bash
npm install @google/generative-ai
```

## Step 2: Get Your Gemini API Key

1. Go to [Google AI Studio](https://aistudio.google.com/app/apikey)
2. Sign in with your Google account
3. Click **"Create API Key"**
4. Copy the generated API key

## Step 3: Create .env File

Create a file named `.env` in the root of your project (same folder as `package.json`):

```bash
# Create the .env file (you can also copy from .env.example)
cp .env.example .env
```

## Step 4: Add Your API Key

Open the `.env` file and add your API key:

```
VITE_GEMINI_API_KEY=your_actual_api_key_here
```

Replace `your_actual_api_key_here` with the API key you copied in Step 2.

## Step 5: Start the Development Server

```bash
npm run dev
```

That's it! The chatbot should now be fully functional.

## How to Use the Chatbot

1. Open your app in the browser (usually `http://localhost:5173`)
2. Look for the **chat icon** in the top-right corner
3. Click the icon to open the chat sidebar
4. Start asking questions about vehicles, financing, or your options!

**Features:**
- 📝 Full markdown support (bold, italic, headings, lists, code blocks)
- 💬 Responses up to 8192 tokens (very long detailed answers)
- 🖥️ All conversations logged to your terminal
- 📱 Responsive message display with proper word wrapping

## Example Questions

- "Which vehicle is best for my budget?"
- "Should I lease or finance the RAV4?"
- "How does my credit score affect my monthly payment?"
- "What down payment should I make on the Corolla?"
- "Is the 4Runner a good financial decision for me?"

## Troubleshooting

### Chatbot shows "not configured" message

- Make sure you created the `.env` file in the project root
- Verify your API key is correct
- Restart the development server after creating/editing `.env`

### API key not working

- Check that your API key doesn't have extra spaces
- Ensure you're using `VITE_GEMINI_API_KEY` (not just `GEMINI_API_KEY`)
- The key should start with `AIza...`

### Need help?

The chatbot uses a comprehensive knowledge base that includes:
- All Toyota vehicle details
- Your selected cars and financing options
- Your financial profile
- Financial guidelines and best practices

It's designed to help you make informed decisions about your vehicle purchase!
