+++
title = "Peer to peer chatserver"
date = 2024-08-18
description = "A chatserver that has no server"

summary = "A chatserver that has no server"
+++


{{< github repo="TheNeg0t1ator/P2P-chatServer" >}}


## The idea
This chatroom was an exercise in cpp sockets, and QT gui design.
I built this chatroom on top of [this git from Axel van Herle](https://github.com/axelvanherle/P2P-TCP-chatclient-NP).
This was written by a fellow of mine, who had written a basic script for the client.

the messages are packed into json strings like this
```json
{
    "id": "Jeff Bezos",
    "ip": "127.0.0.1",
    "port": "666",
    "message": "Hey, musk calling from mars here.",
    "timestamp":"30/02/2030 18:43"
}
```
and support markdown, titles, links *italics* and **bold**/***bold italics***



## Changelog
- [x] send and receive jsons, as opposed to raw string (**CJSON**)
- [x] Username in arguments so that each client has unique username instead of ip being shown
- [x] Timestamps included for message sorting
- [x] Allow for "enter" to be used as key input
- [x] Added a file logger that saves the chat (no encryption ☹️)
- [x] file logger automatically loads the chats again when starting up.
- [x] Added markdown support (😊)

## //TODO
Now, there are still some things i would like to see in this project that did not end up getting developed in time:

- [ ] List of all individual users you have messaged with before
- [ ] A persistent chat history, the log file should be sent when the hash of both does not match
- [ ] encryption. encrypt the messages, and the logfiles, to make it more secure.
- [ ] ability for images, gifs and files to be sent.
