# Plugins

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

# STT services

* Every intent and intent request sent to wire-pod gets put into a SpeechRequest type. This type has functions which go along with it.

##TODO finish