How to make a Biscord Bot
=========================

Biscord is designed to accept messages from any user, and as such, it is possible to build a bot for Biscord.

## Specifications
Biscord uses the `WebSocket` protocol to transmit all messages, libraries of which are found in practically all programming languages. This means that, yes, a Biscord Bot need not be programmed in Javascript.
  * All `WebSocket` connections should be initiated to the IP `10.10.66.51` on port 3000 (URL would look like `"ws://10.10.66.51:3000"`)
  * Any connections displaying unsupported or unexpected behavior will be assumed to be malicious, and, as such, temrinated immediately

The Biscord server accepts a variety of message types, but all share the same basic structure: JSON (Javascript Object Notation). Once again, most programming languages support some sort of module to convert an object into a JSON string. All that is needed from there is to simply send the string to the server.

The server recognizes several message types (as signified by the `type` attribute in the message)
  1. `"message"`: A standard Biscord message
       * `"messageRaw"`: The raw message contained by the message. Key differences between this and the displayed message are: Mentions are replaced with `<@[user id here]>`, and emotes are replaced with `:emote_name:`
       * `"id"`: The UUID of the message (Must be the sending time of the message, in ms); most commonly determined by calling the `now` function of whatever date/time determination method your language provides
       * `"edits"`: The edits of the message (Should be empty array since the bot is "generating" the messages itself)
       * `"channel"`: The name of the channel the message should be sent to; simple string
       * `"author"`: Object representing the author of the message (in this case, the bot itself)
       * `"attachments"`: The file attachments of the message, represented by binary strings
   2. `"delete"`: A deletion event (signifies that a message was deleted)
       * `"id"`: The UUID of the message that was deleted
       * `"creds"`: The credentials of the user deleting the message
            * `"username"`: The username of the user
            * `"id"`: The UUID of the user
   3. `"edit"`: An edit event (signifies that a message has been edited, and describes what the message is edited to)
       * `"id"`: The UUID of the message that was edited
       * `"newMsgRaw"`: The raw message that the message is being edited to
       * `"newMsgDisplay"`: The displayed message the message is being edited to
       * `"oldMsgDisplay"`: The old displayed message
       * `"creds"`: The credentials of the user editing the message
            * `"username"`: The username of the user
            * `"id"`: The UUID of the user
   4. `"join"`: Signifies that a new user has joined the chat room
       * `"user"`: The user that has joined the chat room
   5. `"disconnect"`: Signifies that a user has left the chat room
       * `"user"`: The user that has left the cht room
   6. `"createChannel"`: Event fired when a user creates a new channel
       * `"name"`: The name of the new channel
       * `"user"`: The user creating the channel
   7. `"deleteChannel"`: Event fired when a user deletes a channel
       * `"name"`: The name of the channel being deleted
   8. `"editChannel"`: Event fired when a user edits a channel
       * `"name"`: The old name of the channel
       * `"newName"`: The new name of the channel

> Note: Each user will also be a object, of the format
 * `"username"`: The username of the user
 * `"id"`: The UUID of the user
 * `"icon"`: The icon of the user, represented in data URL format

---

Your bot may accept some or all of these message types, depending on what you want your bot to do. However, it is recommended to accept at least the `message`, `join`, and `disconnect` messages to be able to keep track of the current users in the room.