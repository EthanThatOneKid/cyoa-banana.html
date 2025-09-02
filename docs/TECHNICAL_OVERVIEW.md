# CYOA Banana - Technical Documentation

## Overview

CYOA Banana is a single-file, web-based Choose-Your-Own-Adventure (CYOA) game
that leverages Google's Gemini AI models for dynamic story generation and
accompanying AI-generated images. The application features a mobile-responsive
interface, multi-conversation management, and visual continuity system.

## Architecture

### Technology Stack

- **Frontend**: HTML5, CSS3, JavaScript (ES6+)
- **Styling**: Tailwind CSS (CDN)
- **Fonts**: Google Fonts (Crimson Text, Source Sans Pro)
- **AI Models**:
  - `gemini-2.5-flash` (Text generation)
  - `gemini-2.5-flash-image-preview` (Image generation)
- **Storage**: localStorage
- **API**: Google Gemini API

### File Structure

```
cyoa-banana/
├── index.html (Main application file)
├── README.md
├── TECHNICAL_DOCUMENTATION.md
└── docs/
    └── screen.png
```

## Core Features

### 1. Multi-Conversation System

- **Collapsible Sidebar**: 320px wide sidebar for conversation management
- **Conversation Persistence**: All conversations saved to localStorage
- **Title Management**: Auto-generated and user-editable conversation titles
- **Conversation Switching**: Seamless switching between different adventures

### 2. AI-Powered Story Generation

- **Random Adventures**: AI-generated opening scenarios
- **Custom Prompts**: User-defined story starting points
- **Dynamic Choices**: AI-generated relevant action options
- **Story Continuity**: Context-aware story progression

### 3. Visual System

- **AI-Generated Images**: Scene illustrations for each story segment
- **Visual Continuity**: New images reference previous scenes
- **Alt Text Generation**: AI-generated accessibility descriptions
- **Responsive Design**: Mobile-optimized image display

### 4. User Interface

- **Mobile-Responsive**: Touch-friendly interface
- **Custom Scrollbars**: Sleek, dark-themed scrollbars
- **Smooth Animations**: 300ms transitions throughout
- **Keyboard Support**: Full keyboard navigation

## Technical Implementation

### Data Structures

#### Conversation Object

```javascript
{
  id: "conv_1234567890",
  storyHistory: [...],
  previousImageBase64: "...",
  isInitialized: true,
  lastUpdated: 1234567890,
  title: "Adventure Title"
}
```

#### Story History Format

```javascript
[
  {
    role: "user",
    parts: [{ text: "User action" }],
  },
  {
    role: "model",
    parts: [{ text: "AI response" }],
  },
];
```

### API Integration

#### Text Generation

- **Endpoint**:
  `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash:generateContent`
- **Authentication**: API key as query parameter
- **Retry Logic**: Exponential backoff (5 attempts)
- **Error Handling**: Specific messages for 401, 403, 429 status codes

#### Image Generation

- **Endpoint**:
  `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-image-preview:generateContent`
- **Continuity**: Previous image included in prompt
- **Fallback**: Game continues without image if generation fails
- **Format**: Base64 encoded JPEG data

### Choice Extraction System

#### Method 1: Inline CHOICES Format

```javascript
// Handles: "CHOICES: 1. Choice 1. 2. Choice 2. 3. Choice 3. 4. Choice 4."
const regex = /CHOICES:\s*((?:\d+\.\s*[^.]*\.\s*)*)/i;
```

#### Method 2: Newline CHOICES Format

```javascript
// Handles: "CHOICES:\n1. Choice 1\n2. Choice 2..."
const regex = /CHOICES:\s*\n((?:\d+\.\s*.*\n?)*)/i;
```

#### Method 3: Numbered Choices Anywhere

```javascript
// Handles: "1. Choice 1. 2. Choice 2. 3. Choice 3. 4. Choice 4."
const regex = /(\d+\.\s*[^.]*\.\s*){4}/i;
```

#### Method 4: Fallback Line-by-Line

```javascript
// Handles: Any numbered lines in the text
const regex = /^\d+\.\s*.*$/gm;
```

### Story Cleaning Process

1. **Extract Choices**: Parse choice text from story
2. **Remove Choices**: Clean story text of choice formatting
3. **Return Both**: `{ choices, cleanedStory }`
4. **Display Clean**: Show only narrative content
5. **Button Choices**: Display choices as separate buttons

## Comprehensive Application Scenarios

### 1. Initial Setup & Configuration

#### First-Time User Experience

1. **User opens app** → No API key detected
2. **Prompt appears** → "Please enter your Google GenAI API key"
3. **User enters key** → Key stored in localStorage
4. **Prompt selection** → Choose random or custom adventure
5. **Game initializes** → Story and image generated
6. **Auto-scroll** → User sees latest content

#### Returning User Experience

1. **User opens app** → API key found in localStorage
2. **Load conversations** → Most recent conversation restored
3. **Restore UI** → Story history rebuilt
4. **Auto-scroll** → User sees where they left off

### 2. Adventure Creation & Management

#### Random Adventure Creation

1. **User clicks "+ New Adventure"** → Conversation created
2. **Prompt selection modal** → User chooses "Random Adventure"
3. **AI generates story** → Atmospheric opening scene
4. **Image generation** → Scene illustration created
5. **Alt text generation** → Accessibility description
6. **Choices extracted** → 4 action options displayed
7. **Story cleaned** → Choices removed from narrative
8. **UI updated** → Story, image, and choices shown

#### Custom Adventure Creation

1. **User clicks "+ New Adventure"** → Conversation created
2. **Prompt selection modal** → User chooses "Custom Prompt"
3. **Custom prompt dialog** → User enters scenario description
4. **Character validation** → 500 character limit enforced
5. **AI generates story** → Based on custom prompt
6. **Image generation** → Scene illustration created
7. **Alt text generation** → Context-aware description
8. **Choices extracted** → 4 action options displayed
9. **Story cleaned** → Choices removed from narrative
10. **UI updated** → Custom story, image, and choices shown

### 3. Gameplay & Story Progression

#### Choice Button Selection

1. **User clicks choice button** → Choice text captured
2. **User message added** → Choice displayed in chat history
3. **Story history updated** → User action stored
4. **AI generates response** → Next story segment created
5. **Image generation** → Continuity-based scene illustration
6. **Alt text generation** → Action-context description
7. **Choices extracted** → New 4 action options
8. **Story cleaned** → Choices removed from narrative
9. **UI updated** → New story, image, and choices shown
10. **Auto-scroll** → User sees latest content

#### Custom Text Input

1. **User types in textarea** → Custom action entered
2. **User clicks "Send"** → Custom text captured
3. **User message added** → Custom action displayed in chat
4. **Story history updated** → Custom action stored
5. **AI generates response** → Next story segment created
6. **Image generation** → Continuity-based scene illustration
7. **Alt text generation** → Custom action-context description
8. **Choices extracted** → New 4 action options
9. **Story cleaned** → Choices removed from narrative
10. **UI updated** → New story, image, and choices shown

### 4. Conversation Management

#### Conversation Switching

1. **User clicks hamburger menu** → Sidebar opens
2. **User clicks conversation** → Conversation loaded
3. **Story history restored** → Previous messages rebuilt
4. **UI updated** → Story log populated
5. **Default choices shown** → Continue adventure options
6. **Auto-scroll** → User sees latest content
7. **Sidebar closes** → Return to game view

#### Conversation Title Editing

1. **User clicks title or edit icon** → Inline editing activated
2. **Input field appears** → Current title selected
3. **User types new title** → Character count updates
4. **User presses Enter** → Title saved
5. **Conversation updated** → New title stored
6. **UI refreshed** → Updated title displayed
7. **localStorage updated** → Changes persisted

#### Conversation Deletion

1. **User clicks trash icon** → Confirmation dialog appears
2. **User confirms deletion** → Conversation removed
3. **localStorage updated** → Changes persisted
4. **UI refreshed** → Conversation removed from list
5. **If current conversation** → New conversation created
6. **If other conversation** → List updated

### 5. Sharing & Collaboration

#### Adventure Sharing

1. **User clicks share button** → Share function triggered
2. **Story history gathered** → All messages collected
3. **Image data included** → Latest scene preserved
4. **Data encoded** → Unicode-safe base64 encoding
5. **URL generated** → Shareable link created
6. **Clipboard copy** → Link copied to clipboard
7. **Success message** → User notified of successful copy

#### Shared Adventure Loading

1. **User opens shared URL** → Adventure parameter detected
2. **Data decoded** → Story history and image restored
3. **New conversation created** → Shared adventure loaded
4. **Story history restored** → All messages rebuilt
5. **UI updated** → Complete adventure displayed
6. **Welcome message** → "Continuing shared adventure..."
7. **URL cleaned** → Parameter removed from address bar

### 6. Error Handling & Recovery

#### API Key Issues

1. **Invalid API key** → Error message displayed
2. **User clicks "Change API Key"** → New key prompt
3. **User enters new key** → Key updated in localStorage
4. **Game retries** → New attempt with valid key
5. **Success** → Game continues normally

#### Rate Limiting

1. **API rate limit hit** → Error message displayed
2. **Exponential backoff** → Automatic retry with delays
3. **User sees loading** → Retry attempts shown
4. **Success after delay** → Game continues
5. **Persistent failure** → User notified to try later

#### Image Generation Failure

1. **Image generation fails** → Error logged
2. **Game continues** → Story displayed without image
3. **Alt text skipped** → No image to describe
4. **User experience** → Uninterrupted gameplay
5. **Next attempt** → Image generation retried

#### Choice Extraction Failure

1. **Choices not found** → Extraction fails
2. **Default choices used** → Fallback options displayed
3. **Game continues** → User can still play
4. **Story cleaned** → Narrative still processed
5. **Next generation** → AI may provide better format

### 7. Data Persistence & State Management

#### Game State Saving

1. **User makes choice** → Story history updated
2. **Image generated** → Latest image stored
3. **Conversation updated** → Title and timestamp refreshed
4. **localStorage updated** → All data persisted
5. **UI refreshed** → Sidebar conversation list updated

#### Browser Refresh Recovery

1. **User refreshes page** → DOM reloads
2. **Conversations loaded** → All saved data restored
3. **Most recent selected** → Latest conversation active
4. **UI restored** → Story history rebuilt
5. **Auto-scroll** → User sees latest content

#### Cross-Session Continuity

1. **User closes browser** → Data remains in localStorage
2. **User reopens later** → All conversations available
3. **User selects conversation** → Exact state restored
4. **Game continues** → Seamless experience
5. **No data loss** → All progress preserved

### 8. Accessibility & User Experience

#### Screen Reader Support

1. **Image generated** → Alt text automatically created
2. **Context-aware descriptions** → Story and action context
3. **Fallback alt text** → Generic description if generation fails
4. **Semantic HTML** → Proper structure for assistive technology
5. **Keyboard navigation** → Full functionality without mouse

#### Mobile Responsiveness

1. **Touch interactions** → All buttons touch-friendly
2. **Responsive design** → Adapts to screen size
3. **Sidebar overlay** → Prevents accidental taps
4. **Smooth animations** → 300ms transitions
5. **Optimized scrolling** → Custom scrollbar styling

### 9. Advanced Features

#### Visual Continuity

1. **Previous image stored** → Base64 data preserved
2. **New image generated** → Continuity prompt includes previous image
3. **Consistent style** → Visual narrative maintained
4. **Scene progression** → Logical visual flow
5. **User immersion** → Cohesive visual experience

#### Dynamic Choice Generation

1. **AI analyzes story** → Context understanding
2. **Relevant choices created** → Story-appropriate options
3. **Multiple extraction methods** → Robust parsing
4. **Fallback system** → Default choices if parsing fails
5. **User agency** → Meaningful decision points

## Key Functions

### Core Game Functions

- `initGame(customPrompt)` - Initialize new game
- `initGameWithCustomPrompt(customPrompt)` - Initialize with custom prompt
- `handlePlayerChoice(choiceText)` - Process player actions
- `extractChoicesFromStory(story)` - Parse choices from AI response
- `addStoryEntry(text, imageBase64, isUserMessage, altText)` - Add story segment

### Conversation Management

- `createNewConversation()` - Create new adventure
- `loadConversation(conversationId)` - Switch to existing conversation
- `deleteConversation(conversationId)` - Remove conversation
- `editConversationTitle(conversationId)` - Edit conversation title
- `saveGameState()` - Persist conversation data

### API Functions

- `makeApiCall(url, payload)` - Generic API call with retry logic
- `generateInitialStory(customPrompt)` - Generate opening story
- `generateTextWithHistory()` - Generate story continuation
- `generateInitialImage(storyText)` - Generate scene image
- `generateImageWithContinuity(storyText, userAction)` - Generate continuity
  image
- `generateImageAltText(storyText, userAction)` - Generate accessibility text

### UI Functions

- `updateChoices(choices)` - Update choice buttons
- `setLoading(state)` - Manage loading states
- `scrollToBottom()` - Auto-scroll to latest content
- `toggleSidebar()` - Show/hide conversation sidebar
- `showPromptSelectionDialog()` - Adventure type selection
- `showCustomPromptDialog()` - Custom prompt input

### Sharing Functions

- `shareAdventure()` - Create shareable link
- `loadAdventureFromUrl()` - Load shared adventure

## Styling & UI Components

### Custom Scrollbars

```css
::-webkit-scrollbar {
  width: 8px;
}

::-webkit-scrollbar-track {
  background: #1f2937;
  border-radius: 4px;
}

::-webkit-scrollbar-thumb {
  background: #4b5563;
  border-radius: 4px;
  transition: background-color 0.2s ease;
}

::-webkit-scrollbar-thumb:hover {
  background: #6b7280;
}
```

### Font Configuration

- **Body Text**: Source Sans Pro (sans-serif)
- **Story Text**: Crimson Text (serif, line-height: 1.7)
- **Headers**: Crimson Text (serif, font-weight: 600)

### Color Scheme

- **Background**: Gray-900 (#111827)
- **Cards**: Gray-800 (#1f2937)
- **Primary**: Blue-600 (#2563eb)
- **Text**: White (#ffffff)
- **Secondary Text**: Gray-300 (#d1d5db)

## Performance Considerations

### Optimization Strategies

- **Single File**: All code in one HTML file for easy deployment
- **CDN Resources**: Tailwind CSS and Google Fonts from CDN
- **Lazy Loading**: Images loaded only when needed
- **Error Isolation**: Failures don't break entire application
- **Efficient DOM**: Minimal DOM manipulation

### Memory Management

- **localStorage Limits**: Conversations stored efficiently
- **Image Compression**: Base64 images optimized
- **Event Cleanup**: Proper event listener management
- **State Management**: Minimal global state variables

## Browser Compatibility

### Supported Browsers

- **Chrome**: Full support
- **Firefox**: Full support
- **Safari**: Full support
- **Edge**: Full support
- **Mobile Browsers**: iOS Safari, Chrome Mobile

### Required Features

- **ES6+ Support**: Arrow functions, template literals, destructuring
- **localStorage**: Data persistence
- **Fetch API**: HTTP requests
- **CSS Grid/Flexbox**: Layout system
- **Web APIs**: Clipboard API, History API

## Security Considerations

### API Key Management

- **Client-Side Storage**: API keys stored in localStorage
- **No Server**: All processing client-side
- **User Responsibility**: Users manage their own API keys
- **No Key Transmission**: Keys not sent to external servers

### Data Privacy

- **Local Storage**: All data stays in user's browser
- **No Tracking**: No analytics or user tracking
- **Shareable Links**: User controls what they share
- **Temporary URLs**: Shared links can be bookmarked

## Deployment

### Single File Deployment

1. **Upload index.html** to any web server
2. **No build process** required
3. **No dependencies** to install
4. **CDN resources** loaded automatically
5. **Ready to use** immediately

### Hosting Options

- **GitHub Pages**: Free static hosting
- **Netlify**: Free tier available
- **Vercel**: Free tier available
- **Any Web Server**: Apache, Nginx, etc.

## Future Enhancements

### Potential Features

- **Export/Import**: Save conversations as files
- **Themes**: Multiple color schemes
- **Sound Effects**: Audio for immersion
- **Multiplayer**: Shared real-time adventures
- **Story Templates**: Pre-built adventure frameworks
- **Advanced AI**: More sophisticated story generation
- **Image Styles**: Different artistic styles
- **Voice Input**: Speech-to-text for actions

### Technical Improvements

- **Service Worker**: Offline functionality
- **PWA Support**: App-like experience
- **Database Integration**: Server-side storage
- **Real-time Sync**: Multi-device synchronization
- **Advanced Analytics**: Usage insights
- **A/B Testing**: Feature experimentation

## Troubleshooting

### Common Issues

1. **API Key Invalid**: Check key validity and permissions
2. **Rate Limiting**: Wait and retry, or upgrade API quota
3. **Image Generation Fails**: Game continues without images
4. **Choices Not Extracted**: Default choices used as fallback
5. **localStorage Full**: Clear browser data or use fewer conversations

### Debug Information

- **Console Logging**: Detailed error messages
- **Network Tab**: API request/response inspection
- **Application Tab**: localStorage inspection
- **Performance Tab**: Loading time analysis

---

_This documentation covers the complete technical implementation of CYOA Banana,
including all features, scenarios, and implementation details._
