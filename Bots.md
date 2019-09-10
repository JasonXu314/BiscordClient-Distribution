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
            * `"username"`: The username of your bot
            * `"id"`: The UUID of the bot; determined by join time, similar to above method
            * `"icon"`: The profile picture of the bot, in a data URL format
       * `"attachments"`: The file attachments of the message, represented by binary strings
   2. `"delete"`: A deletion event (signifies that a message was deleted)
       * `"id"`: The UUID of the message that was deleted
       * `"creds"`: The credentials of the user deleting the message
            * `"username"`: The username of the user
            * `"id"`: The UUID of the user
   3. `"edit"`: 