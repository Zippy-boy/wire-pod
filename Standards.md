# Plugins

* You may create native Go plugins for wire-pod, soon for use with the vector-go-sdk.

* Each plugin file should have an array of strings (Utterances []string) of which wire-pod will attempt to match each speech transcription with, a Name (Name string), and a function (func Action(string) string). The function will be called with whatever the full speech transcription was. It should be returned with an intent, even though the Intent request will likely be over by the time the function finishes.

* Example plugin .go file:

```
package main

import "fmt"

var Utterances = []string{"test plugin"}
var Name = "Test Plugin"

func Action(transcribedText string) string {
    fmt.Println("(in testPlugin) Printing transcribed text: " + transcribedText)
    return "intent_imperative_praise"
}
```

* The plugin should be built with `/usr/local/go/bin/go build -buildmode=plugin plugin.go`

* The resulting .so should be placed in `~/wire-pod/chipper/plugins/`. wire-pod will load it upon startup.

# STT services

* Every intent and intent request sent to wire-pod gets put into a SpeechRequest type. This type has functions which go along with it.

##TODO finish