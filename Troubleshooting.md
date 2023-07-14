If you have an issue, check here before opening an issue. This page will also contain issue standards.

## Vector authenticated with vector-wirepod-setup, but wire-pod doesn't seem to know he exists.

-   This likely means that Vector is not running escape pod firmware like he should be. He authenticated with the official servers. Follow the first section of the installation steps again, and make sure he actually gets to recovery mode. His face will show either "anki.com/v" or "ddl.io/v" if he is in recovery mode.

## Vector has working commands, and is communicating with wire-pod, but isn't showing up as a bot in "Bot Settings".

-   Follow the last section of the installation guide. You need to go back to the vector-wirepod-setup page and finish authentication. You don't only use the site once.

## "Error logging in. The bot is likely unable to communicate with your wire-pod instance."

-   This error appears on the vector-wirepod-setup page when the bot reports an error communicating with wire-pod. This is a common error and many things could be causing it.

1. Make sure you don't have an official escape pod running, or any other device with the hostname `escapepod`.

2. Make sure that the device running wire-pod has the hostname `escapepod`.

3. Make sure wire-pod is running.