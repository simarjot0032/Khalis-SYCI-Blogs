---
title: "Problems Faced while coding OpenAI API"
layout: post
---

## Open AI speech to text API

Hey reader I was given a task to code a webpage which taskes speech as user input and converts to text using Open AI API.

For this first of all I did search about the open ai's API and what is the endpoint of that so then I read their API refrence and watch some tutorials and then I start coding it.


## Problems Faced 
- During coding firstly I got then error which says **Daily Quota Excedded**. So I waited till next day then I again hit the API it says the same error again
- Then I changed some code and hit it again then it says 400 error which means some is wrong in code 
- Then I changed the code again and then it shows error code 429 whcih means many request at same time

## Steps I took To Solve The Error
- Read Open AI's Documentation
- Read many blogs.
- Watched youtube tutorials
- Searched google, Stack overflow.

## Solution
Then I came to know that we also need some credits in your Open AI's account then I contacted my mentor (Inderpreet Singh) sir who helped me to get the credits in Open AI's account.
Then I again tried to code to hit the API with samll changes in code I was able to get the output I am also attaching the JS code.
``` JavaScript
let recorder;
let chunks = [];

micBtn.addEventListener("click", async () => {
  try {
    const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
    recorder = new MediaRecorder(stream);
    recorder.ondataavailable = (e) => {
      chunks.push(e.data);
    };

    recorder.onstop = async () => {
      const audioBlob = new Blob(chunks, { type: "audio/mp3" });
      const audioStream = audioBlob.stream();
      chunks = [];
      const API = "https://API.openai.com/v1/audio/transcriptions";
      const Key =
        "API Key";
      const formData = new FormData();
      formData.append("model", "whisper-1");
      formData.append("file", audioBlob, "recording.mp3");
      formData.append("prompt", "give output in indian punjabi");
      formData.append(" response_format", "verbose_json");
      try {
        const response = await fetch(API, {
          method: "POST",
          headers: {
            Authorization: `Bearer ${Key}`,
          },
          body: formData,
        });

        if (!response.ok) {
          console.error(response.status);
        } else {
          const data = await response.json();
          searchBox.value = data.text;
          outputArray.push(data.text);
        }
      } catch (e) {
        console.error("Fetch error:", e.message);
      }
    };

    recorder.start();
    setTimeout(() => recorder.stop(), 5000);
  } catch (err) {
    console.error("Error accessing media devices:", err);
  }
});
``` 

I am also attaching the full code [here](https://github.com/simarjot0032/Khalis-SYCI/blob/main/SpeechReco/Openai.js)