# CYOA Banana 🍌

An AI-powered Choose-Your-Own-Adventure game that generates dynamic stories and
accompanying images using Google's Gemini AI models. Experience immersive,
personalized adventures with visual storytelling and seamless conversation
management.

> **Inspiration**: This project was created in inspiration of the
> [Kaggle Banana Competition](https://www.kaggle.com/competitions/banana) 🍌

## Features

- **🤖 AI-Powered Story Generation**: Dynamic, context-aware story creation
  using Gemini 2.5 Flash
- **🎨 Visual Storytelling**: AI-generated scene illustrations with visual
  continuity
- **📱 Mobile-Responsive Design**: Optimized for both desktop and mobile devices
- **💬 Multi-Conversation Management**: Create, switch between, and manage
  multiple adventures
- **🎯 Custom Prompts**: Start adventures with your own scenarios or let AI
  surprise you
- **🔗 Adventure Sharing**: Share your adventures with others via shareable
  links
- **♿ Accessibility**: AI-generated alt text for all images and full keyboard
  navigation
- **💾 Persistent Storage**: All progress saved locally in your browser

## Quick Start

### Prerequisites

- A modern web browser (Chrome, Firefox, Safari, Edge)
- A Google GenAI API key (free at
  [Google AI Studio](https://aistudio.google.com/app/apikey))

### Running the Application

1. **Clone or download** this repository
2. **Open `index.html`** in your web browser, or serve it using a local server:

```sh
# Using Deno (recommended)
deno -A jsr:@std/http/file-server

# Using Python
python -m http.server 8000

# Using Node.js (if you have http-server installed)
npx http-server
```

3. **Enter your API key** when prompted
4. **Start your adventure!** Choose between random or custom prompts

## Demo Video

Watch on YouTube: https://youtu.be/itpfYMkGe40?si=cbqUqAnLSmjjbRV4

## How to Play

### Starting an Adventure

1. **Click the hamburger menu** (☰) to access conversations
2. **Click "+ New Adventure"** to create a new story
3. **Choose your adventure type**:
   - **Random Adventure**: Let AI create a surprise scenario
   - **Custom Prompt**: Describe your own starting scenario

### Playing the Game

- **Read the story** and view the generated scene illustration
- **Choose your action** by clicking one of the four choice buttons
- **Or type a custom action** in the text area and click "Send"
- **Watch the story unfold** with new scenes and images

### Managing Adventures

- **Switch between conversations** using the sidebar
- **Edit conversation titles** by clicking on them
- **Delete conversations** using the trash icon
- **Share adventures** using the share button to create shareable links

## Technical Details

### Technology Stack

- **Frontend**: HTML5, CSS3, JavaScript (ES6+)
- **Styling**: Tailwind CSS
- **AI Models**:
  - Gemini 2.5 Flash Lite (text generation)
  - Gemini 2.5 Flash Image Preview (image generation)
- **Storage**: Browser localStorage
- **Deployment**: Single-file application

### Key Features

- **Visual Continuity**: New images reference previous scenes for cohesive
  storytelling
- **Robust Choice Extraction**: Multiple parsing methods ensure choices are
  always available
- **Error Handling**: Graceful fallbacks for API failures and network issues
- **Responsive Design**: Optimized for mobile and desktop experiences

## Configuration

### API Key Setup

1. Visit [Google AI Studio](https://aistudio.google.com/app/apikey)
2. Create a free API key
3. Enter the key when prompted in the application
4. For this demo, the key is stored locally in your browser

### Customization

The application is a single HTML file, making it easy to customize:

- Modify the styling in the `<style>` section
- Adjust AI prompts in the JavaScript functions
- Change the color scheme using Tailwind CSS classes

## Documentation

## 🤝 Contributing

This is a single-file application designed for simplicity and ease of
deployment. Contributions are welcome:

1. Fork the repository
2. Make your changes to `index.html`
3. Test thoroughly across different browsers
4. Submit a pull request

## 📄 License

This project is open source and available under the MIT License.

## 🆘 Troubleshooting

### Common Issues

- **"Invalid API key"**: Check your Google GenAI API key and ensure it has the
  correct permissions
- **"Rate limit exceeded"**: Wait a moment and try again, or check your API
  quota
- **Images not loading**: The game continues without images if generation fails
- **Choices not appearing**: Default choices will be used as a fallback

### Getting Help

- Check the browser console for detailed error messages
- Ensure you're using a modern browser with JavaScript enabled
- Verify your API key is valid and has sufficient quota

---

**Ready to start your adventure?** 🍌
