
An extendable chat bot for the distributed, private messenger Jami.  It
interacts with the locally-installed Jami daemon via D-Bus and reacts to
both plain text messages and file transfers sent to local accounts.  Further
processing of either or both can be configured by adding functions to the
abnormal hooks, `jami-bot-text-message-functions' and
`jami-bot-data-transfer-functions', respectively.

Additionally, the bot allows special actions to be triggered by sending a
text message starting with an exclamation mark and a command keyword.  Further
commands than the ones included can be configured by mapping them to
functions through `jami-bot-command-function-alist'.  See, for example, the
function `jami-bot--command-function-ping' which is by default called when
a message starts with the "!ping" command string.

Set up `jami-bot' by executing `jami-bot-register'.  This will set up the
message handler, `jami-bot--messageReceived-handler', to be called on the
`messageReceived' D-Bus signal.  You will need to run `jami-bot-register'
whenever you restart Emacs or the Jami process.

`jami-bot' offers several customizable variables, e.g. to limit message
processing to certain local accounts or to specify a directory to store
downloaded files in.
