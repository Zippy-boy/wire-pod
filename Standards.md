# Standards

* This page is documentation for devs who want to develop for wire-pod.

## Plugins

* You may create native Go plugins for wire-pod, soon for use with the vector-go-sdk.

* Each plugin file should have an array of strings (Utterances []string) of which wire-pod will attempt to match each speech transcription with, a Name (Name string), and a function (func Action(string, string) string). The function will be called with whatever the full speech transcription was and with the serial number of the requester bot. It should be returned with an intent, even though the Intent request will likely be over by the time the function finishes.

* Example plugin .go file:

```
package main

import "fmt"

var Utterances = []string{"test plugin"}
var Name = "Test Plugin"

func Action(transcribedText string, botSerial string) string {
    fmt.Println("(in testPlugin) Transcribed text: " + transcribedText ", serial number: " + botSerial)
    return "intent_imperative_praise"
}
```

* The plugin should be built with `sudo /usr/local/go/bin/go build -buildmode=plugin plugin.go` (has to be built with the same version of Go wire-pod is using and same user)

* The resulting .so should be placed in `~/wire-pod/chipper/plugins/`. wire-pod will load it upon startup.

* If you want to use the vector-go-sdk in your plugin, I recommend importing my fork: `"github.com/kercre123/vector-go-sdk/pkg/sdk-wrapper"`

* Every bot uses the same GUID for authentication: 

```
tni1TRsTRTaNSapjo0Y+Sw==
```

## STT services

* Every intent and intent request sent to wire-pod gets put into a SpeechRequest type. This type has functions which go along with it.

* Your STT function should accept a SpeechRequest and return a string and error. You may have an init function if you need it.

* `SpeechRequest.FirstReq` is the first chunk of audio data from the robot, this needs to be processed before the the rest of the data

* Each sttHandler function should have a for loop which calls the `getNextStreamChunk(req)` function. The input SpeechRequest also needs to be set equal to the req the function returns.

* There is a built-in speech end detector (VAD). This function is called `detectEndOfSpeech(req)` and should be called in the for loop as well. Its output req should also be set equal to the sttHandler's input req.

* Example sttHandler function:

```
func VoskSTTHandler(req SpeechRequest) (string, error) {
	logger("(Bot " + strconv.Itoa(req.BotNum) + ", Vosk) Processing...")
    // set speechIsDone as a function-wide variable
	speechIsDone := false
	sampleRate := 16000.0
    // begin vosk
	rec, err := vosk.NewRecognizer(model, sampleRate)
	if err != nil {
		log.Fatal(err)
	}
	rec.SetWords(1)
    // process the first chunk of audio
	rec.AcceptWaveform(req.FirstReq)
	for {
		var chunk []byte
        // get the next mic chunk in the stream
        // (note the `=`, not a `:=`)
		req, chunk, err = getNextStreamChunk(req)
		if err != nil {
			return "", err
		}
        // input chunk into vosk for processing
		rec.AcceptWaveform(chunk)
        // input req into VAD to detect end of speech
		req, speechIsDone = detectEndOfSpeech(req)
        // if speech is done, break and complete the function
		if speechIsDone {
			break
		}
	}
    // parse transcribed text from json
	var jres map[string]interface{}
	json.Unmarshal([]byte(rec.FinalResult()), &jres)
	transcribedText := jres["text"].(string)
	logger("Bot " + strconv.Itoa(req.BotNum) + " Transcribed text: " + transcribedText)
	return transcribedText, nil
}
```