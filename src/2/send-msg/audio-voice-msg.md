# Audio and Voice Messages

[![audio tests](https://img.shields.io/badge/Examples-Audio_Messages-green.svg?style=flat-square)](https://github.com/TelegramBots/Telegram.Bot/blob/master/test/Telegram.Bot.Tests.Integ/Sending%20Messages/AudioMessageTests.cs)

These two types of messages are pretty similar. An audio is in MP3 format and gets displayed in music player.
A voice file has OGG format and is not shown in music player.

## Audio

[![send audio method](https://img.shields.io/badge/Bot_API_method-send_audio-blue.svg?style=flat-square)](https://core.telegram.org/bots/api#sendaudio)

This is the code to send a MP3 sound track. You might be wondering why some parameters are commented out.
That's because this MP3 file has metadata on it and Telegram does a good job at reading them.

```c#
Message msg = await botClient.SendAudioAsync(
  e.Message.Chat,
  "https://github.com/TelegramBots/book/raw/master/src/docs/audio-guitar.mp3"
  /* ,
  performer: "Joel Thomas Hunger",
  title: "Fun Guitar and Ukulele",
  duration: 91 // in seconds
  */
);
```

![audio message](../docs/shot-audio_msg.jpg)

Method returns an audio message. Let's take a look at the value of `msg.Audio` property in JSON format:

```json
{
  "duration": 91,
  "mime_type": "audio/mpeg",
  "title": "Fun Guitar and Ukulele",
  "performer": "Joel Thomas Hunger",
  "file_id": "CQADBAADKQADA3oUUKalqDOOcqesAg",
  "file_size": 1102154
}
```

## Voice

[![send voice method](https://img.shields.io/badge/Bot_API_method-send_voice-blue.svg?style=flat-square)](https://core.telegram.org/bots/api#sendvoice)

Voice message is just an OGG audio file.
Let's send it differently this time by uploading the file from disk alongside with HTTP request.

To run this example, download the
[NFL Commentary voice file](https://github.com/TelegramBots/book/raw/master/src/docs/voice-nfl_commentary.ogg)
to your disk.

A value is passed for `duration` because Telegram can't figure that out from file's metadata.

```c#
Message msg;
using (var stream = System.IO.File.OpenRead("/path/to/voice-nfl_commentary.ogg")) {
  msg = await botClient.SendVoiceAsync(
    chatId: e.Message.Chat,
    voice: stream,
    duration: 36
  );
}
```

![voice message](../docs/shot-voice_msg.jpg)

Voice message is returned from method. Inspect `msg.Voice` property to learn more.