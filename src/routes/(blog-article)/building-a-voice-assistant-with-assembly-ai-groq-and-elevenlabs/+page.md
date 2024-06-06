---
title: Building a Voice-Assistant with Assembly AI, Groq, and ElevenLabs
slug: building-a-voice-assistant-with-assembly-ai-groq-and-elevenlabs
coverImage: /images/posts/va-Cover.jpg
date: 2024-04-06T00:00:00.000Z
excerpt: Step-by-step guide to building an insanely fast voice-assistant using Assembly AI, Groq, and ElevenLabs.
tags:
  - AI
  - Voice Assistant
  - Python
  - Programming
  - Assembly AI
  - Groq
  - ElevenLabs
---

<script>
  import Callout from "$lib/components/molecules/Callout.svelte";
  import CodeBlock from "$lib/components/molecules/CodeBlock.svelte";
  import Image from "$lib/components/atoms/Image.svelte";
</script>

## Introduction

I wanted to build a voice-assistant for my daughter when she gets older so she could ask questions to when shes playing and get answers from the AI. Creating a voice-assistant that operates in real-time requires the integration of multiple technologies. In this post, I'll walk you through how I built a fast voice-assistant using Python libraries for real-time transcription, AI response generation, and live audio streaming. By the end of this tutorial, you'll understand how to perform real-time speech-to-text transcription with Assembly AI, generate responses with Groq, and stream audio responses using ElevenLabs.

## Step 1: Install Python Libraries

First, we need to install the required Python libraries. These libraries will allow us to transcribe speech in real-time, generate AI responses, and stream audio.

<CodeBlock lang="bash">

```bash
pip install groq
pip install "assemblyai[extras]"
pip install elevenlabs
brew install portaudio
brew install mpv
```

</CodeBlock>

These commands will install the following libraries:

- **assemblyai**: For real-time speech-to-text transcription.
- **groq**: For generating AI responses using LLAMA 3.
- **elevenlabs**: For streaming audio responses.
- **portaudio**: Required for audio processing.
- **mpv**: Required for audio playback.

## Step 2: Real-Time Transcription with AssemblyAI

To transcribe speech in real-time, we use AssemblyAI's real-time transcription service. Here's how we set it up:

<CodeBlock lang="python">

```python
import assemblyai as aai

class AI_Assistant:
    def __init__(self):
        aai.settings.api_key = "your_assemblyai_api_key"
        self.transcriber = None

    def start_transcription(self):
        self.transcriber = aai.RealtimeTranscriber(
            sample_rate=16_000,
            on_data=self.on_data,
            on_error=self.on_error,
            on_open=self.on_open,
            on_close=self.on_close,
        )
        self.transcriber.connect()
        microphone_stream = aai.extras.MicrophoneStream(sample_rate=16_000)
        self.transcriber.stream(microphone_stream)

    def stop_transcription(self):
        if self.transcriber:
            self.transcriber.close()
            self.transcriber = None

    def on_open(self, session_opened: aai.RealtimeSessionOpened):
        return

    def on_data(self, transcript: aai.RealtimeTranscript):
        if not transcript.text:
            return
        if isinstance(transcript, aai.RealtimeFinalTranscript):
            self.generate_ai_response(transcript)

    def on_error(self, error: aai.RealtimeError):
        return

    def on_close(self):
        return
```

</CodeBlock>

In this code, we set up a real-time transcriber using AssemblyAI. When the transcriber receives new data, it calls the `on_data` method, which processes the transcription.

## Step 3: Pass Real-Time Transcript to LLAMA 3

Next, we need to pass the real-time transcription to LLAMA 3 to generate an AI response. Here's how we do it:

<CodeBlock lang="python">

```python
import assemblyai as aai
from groq import Groq

class AI_Assistant:
    def __init__(self):
        aai.settings.api_key = "your_assemblyai_api_key"
        self.groq_client = Groq(api_key="your_groq_api_key")
        # Start with a predefined message in the transcript
        self.full_transcript = [{"role": "system", "content": "You are a friendly AI assistant."}]

    def generate_ai_response(self, transcript):
        # Stop transcription to process the current data
        self.stop_transcription()
        # Append the user's last spoken text to the transcript
        self.full_transcript.append({"role": "user", "content": transcript.text})

        # Create a streaming session with Groq using the updated transcript
        groq_stream = self.groq_client.chat.completions.create(
            model="llama3-8b-8192", messages=self.full_transcript, stream=True
        )
        text_buffer = ""
        full_text = ""
        # Process each chunk received from the Groq stream
        for chunk in groq_stream:
            if chunk.choices[0].delta.content is not None:
                text_buffer += chunk.choices[0].delta.content
                # If a sentence ends, stream it as audio and reset the buffer
                if text_buffer.endswith("."):
                    self.stream_audio_response(text_buffer)
                    full_text += text_buffer
                    text_buffer = ""

        # If there's remaining text that didn't end with a period, stream it
        if text_buffer:
            self.stream_audio_response(text_buffer)
            full_text += text_buffer

        # Append the full AI-generated response to the transcript
        self.full_transcript.append({"role": "assistant", "content": full_text})
        # Restart transcription after processing
        self.start_transcription()
```

</CodeBlock>

This method stops the transcription, appends the user's transcription to the conversation, generates a response using LLAMA 3, and streams the response audio.

## Step 4: Live Audio Stream from ElevenLabs

Finally, we use ElevenLabs to stream the AI response as live audio:

<CodeBlock lang="python">

```python
from elevenlabs import stream
from elevenlabs.client import ElevenLabs

class AI_Assistant:
    def __init__(self):
        aai.settings.api_key = "your_assemblyai_api_key"
        self.groq_client = Groq(api_key="your_groq_api_key")
        self.client = ElevenLabs(api_key="your_elevenlabs_api_key")

        self.transcriber = None



    def stream_audio_response(self, text):
        audio_stream = self.client.generate(
            text=text, model="eleven_turbo_v2", stream=True
        )
        stream(audio_stream)
```

</CodeBlock>

This method uses ElevenLabs to generate and stream the audio response based on the text generated by LLAMA 3.

## Full Code Example

Here is the complete code for the AI assistant:

<CodeBlock lang="python">

```python
import assemblyai as aai
from elevenlabs import stream
from elevenlabs.client import ElevenLabs
from groq import Groq

class AI_Assistant:
    def __init__(self):
        aai.settings.api_key = "your_assemblyai_api_key"
        self.groq_client = Groq(api_key="your_groq_api_key")
        self.client = ElevenLabs(api_key="your_elevenlabs_api_key")

        self.transcriber = None
        self.full_transcript = [
            {
                "role": "system",
                "content": """You are a friendly and engaging AI voice assistant designed to interact with toddlers aged 3-5 years old. Your primary goal is to provide fun, educational, and age-appropriate interactions. Here are your guidelines:

                    1. **Language**: Use simple and clear words. Speak slowly and clearly.
                    2. **Engagement**: Incorporate interactive elements like songs, rhymes, and simple questions to keep the child engaged.
                    3. **Tone**: Maintain a friendly and gentle tone throughout the conversation.
                    4. **Encouragement**: Provide positive reinforcement and encourage curiosity and learning.
                    5. **Brevity**: Keep responses under 50 words.
                    
                    Example interactions:
                    - "Hi there! I'm your friend. Do you want to sing a song with me? Let's sing 'Twinkle, Twinkle, Little Star'!"
                    - "Can you show me your favorite toy? Wow, that's amazing! What color is it?"
                    - "Do you want to play a game? Let's find something red in the room!"

                Always be cheerful, supportive, and ready to engage the toddler in fun and educational activities.
                """,
            },
        ]

    def start_transcription(self):
        self.transcriber = aai.RealtimeTranscriber(
            sample_rate=16_000,
            on_data=self.on_data,
            on_error=self.on_error,
            on_open=self.on_open,
            on_close=self.on_close,
        )
        self.transcriber.connect()
        microphone_stream = aai.extras.MicrophoneStream(sample_rate=16_000)
        self.transcriber.stream(microphone_stream)

    def stop_transcription(self):
        if self.transcriber:
            self.transcriber.close()
            self.transcriber = None

    def on_open(self, session_opened: aai.RealtimeSessionOpened):
        return

    def on_data(self, transcript: aai.RealtimeTranscript):
        if not transcript.text:
            return
        if isinstance(transcript, aai.RealtimeFinalTranscript):
            self.generate_ai_response(transcript)

    def on_error(self, error: aai.RealtimeError):
        return

    def on_close(self):
        return

    def generate_ai_response(self, transcript):
        self.stop_transcription()
        self.full_transcript.append({"role": "user", "content": transcript.text})

        groq_stream = self.groq_client.chat.completions.create(
            model="llama3-8b-8192", messages=self.full_transcript, stream=True
        )
        text_buffer = ""
        full_text = ""
        for chunk in groq_stream:
            if chunk.choices[0].delta.content is not None:
                text_buffer += chunk.choices[0].delta.content
                if text_buffer.endswith("."):
                    self.stream_audio_response(text_buffer)
                    full_text += text_buffer
                    text_buffer = ""

        if text_buffer:
            self.stream_audio_response(text_buffer)
            full_text += text_buffer

        self.full_transcript.append({"role": "assistant", "content": full_text})
        self.start_transcription()

    def stream_audio_response(self, text):
        audio_stream = self.client.generate(
            text=text, model="

eleven_turbo_v2", stream=True
        )
        stream(audio_stream)

ai_assistant = AI_Assistant()
ai_assistant.start_transcription()
```

</CodeBlock>

## Conclusion

Building a real-time voice-assistant requires integrating multiple technologies to handle speech-to-text transcription, AI response generation, and live audio streaming. By following this guide, you can create your own voice-assistant using Python. This setup is perfect for creating interactive and engaging experiences, such as an AI voice assistant for toddlers. Happy coding!
