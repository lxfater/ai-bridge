# Ai-Bridge

## Important things

It is some kind of hacking thing so you should take responsibility to use this library since your account may be banned when your call the provider(chatgpt or ...) API frequently. I recommend you use the
unimportant account to use this library.

## What is ai-bridge ?

A browser extension library brings the AI power of the ChatGPT to another webpage.

# Something to say!!!

We have seen so many great applications to bring the AI power of the ChatGPT to another webpage. One of the most famous is the [ChatGPT for Google](https://github.com/wong2/chatgpt-google-extension).It brings the AI power of the ChatGPT to google.

My friend! You can do the same thing by using this library in the background.js and injecting the content_script to the traditional website you want using the [browser extension techie](https://developer.chrome.com/docs/extensions/mv3/). Then you can send the message between the website and
ChatGPT.

Here is my [little project](https://github.com/lxfater/Learning-By-GPT) bringing the AI power of the ChatGPT to generate articles to help the student to improve their reading skill without using any server to read their data.

You can do the same thing and do it better to make the traditional website better or just use [prompt engineering](https://github.com/dair-ai/Prompt-Engineering-Guide) to create an amazing application for people.

Time to write some code, developer!!

## Install

```
npm install @lxfater/ai-bridge
```

## Usage

Import AI-Bridge:

```javascript
import { ChatGptWebProvider } from "ai-bridge";
```

### Ask

```javascript
import { ChatGptWebProvider } from "ai-bridge";
let BridgeInstance = new ChatGptWebProvider();

const controller = new AbortController();
// The result is the final answer.
let result = await BridgeInstance.ask(keywordsPrompt, {
  deleteConversation: true, // Set it true to delete the conversation to avoid exposing your prompt engineering stuff to the user.
  signal: controller.signal, // Abort signal
  onMessage: (message) => {
    console.log(message); // Get the real-time message.
  },
});

// You can abort it later.
controller.abort();
```

### Talk

Talking in the same chat channel.

```javascript
let cId = "";
let pId = "";
const conversation = async (question: string, signal: AbortSignal) => {
  let {
    text,
    cId: conversationID,
    pId: parentMessageId,
  } = await BridgeInstance.talk(keywordsPrompt, {
    signal,
    cId,
    pId,
    onMessage: (message) => {
      console.log(message);
    },
  });
  cId = conversationID;
  pId = parentMessageId;

  return text;
};

const controller = new AbortController();
let result = await conversation("Hi!", controller.signal);
```

### Error Handle

There is a list of errors that should help you develop better.

```javascript
const errorText = {
  "Too many requests": "tooManyRequests",
  Unauthorized: "unauthorized",
  "Not Found": "notFound",
  Unknown: "unknown",
  CloudFlare: "cloudFlare",
  "Only one": "onlyOne",
};

import { ChatGptWebProvider } from "ai-bridge";
let BridgeInstance = new ChatGptWebProvider();
try {
  let result = await BridgeInstance.ask(keywordsPrompt, {
    deleteConversation: true,
    onMessage: (message) => {
      console.log(message); // Get the real-time message.
    },
  });
} catch(error) {
    if(error.message === errorText[Unauthorized]) {
        console.log('Go to login')
    } else if (error.message === errorText[CloudFlare]) {
        console.log('Remind the user  to pass by the Cloudflare')
    }
    // Try to handle most error
}
```
