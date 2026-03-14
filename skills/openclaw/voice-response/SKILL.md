---
name: voice-response
description: Use when the user specifically requests a voice-only response (e.g., "用語音回覆", "voice message please", "speak to me") on Telegram or WhatsApp. This skill ensures the response is converted to speech, optimized via ffmpeg, and delivered as a voice note with zero accompanying text.
metadata: {"openclaw":{"emoji":"🎙️","os":["linux"],"requires":{"bins":["ffmpeg", "edge-tts"]}}}
---

# Voice Response (Telegram & WhatsApp)

This skill overrides default text-based communication to provide a pure audio experience across messaging channels.

## Workflow

1.  **Identify Intent**: Triggered when the user asks for a voice/audio reply.
2.  **Generate Response**: Formulate the response in the user's preferred language (defaulting to Cantonese/English mix unless specified).
3.  **Convert to Speech**: Call the `edge-tts` command-line tool (located at `/home/openclawbot/.local/bin/edge-tts` if not in PATH). 
    *   Use settings from `openclaw.json` (Voice: `zh-HK-WanLungNeural`).
    *   Save the output directly to the workspace's media folder.
    ```bash
    edge-tts --voice zh-HK-WanLungNeural --text "Speaking Text" --write-media media/voice.mp3
    ```
4.  **FFmpeg Optimization**: Convert the MP3 to the Telegram/WhatsApp standard (OGG Opus) to ensure it appears as a voice note bubble.
    ```bash
    ffmpeg -i media/voice.mp3 -y -c:a libopus -b:a 48k -ac 1 -ar 48000 -application voip media/voice.ogg
    ```
5.  **Direct Delivery**: 
    *   Use the `message` tool with `action="send"` to deliver the `media/voice.ogg` file.
    *   Set `asVoice=true` to ensure the platform treats it as a PTT/Voice Note.
    *   Set `replyTo` to the current message ID.
    *   **CRITICAL**: After calling the `message` tool, respond with exactly `NO_REPLY` to prevent any duplicate text messages.

## Example Usage

**User**: "用語音回覆，話我知聽日天氣點。"
**Agent**:
1. (Calls weather tool)
2. (Calls edge-tts) -> `media/voice.mp3`
3. (Calls ffmpeg) -> `media/voice.ogg`
4. (Calls message tool: `action="send"`, `filePath="media/voice.ogg"`, `asVoice=true`)
5. **Final Response**: `NO_REPLY`

## Technical Details

- **Formats**: WhatsApp and Telegram both prefer **OGG Opus** for voice notes.
- **Paths**: Always use relative paths like `media/voice.ogg` which resolve to the current workspace's media folder.
- **Silent Mode**: This skill is designed to be "text-free." Never include greetings or transcripts in the final turn output; only use the `message` tool and `NO_REPLY`.