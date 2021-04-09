# Basic flow for this slightly convoluted, but overall efficient system of serving translations on the site

This gives a concise overview of the entire process of my overengineered solution.

- Admins upload new content to the Instagram page, and run the `notify` command in the Discord server.<br>
  This pings the translators and lets them know that new content is available for them to work on.
- The admins also update the various language pages to point the thumbnail of the new post to the expected location of the translated document, i.e. `https://docs.naarivad.in/{post_id}.pdf`
  <br>If this file does not exist, it will just return a `404 Not Found` status, which can be handled.
- The translators then do their jobs, and upload their finished document(s), named as per the [accepted format](filename_guidelines.md) to the designated channel on Discord.
- The bot responds with a [status code](#Status-Codes) indicating the status of the upload.
- Files are converted to PDF (if not already done) and re-uploaded. This page is now available for open read-only access on the server.

## Editing
A second upload with the exact same filename will replace the existing file. This allows you to edit existing translations.<br>
Be mindful that this can also cause inadvertent replacement of other peoples' work, therefore please _**check your filenames before uploading**_.

## Deleting
You generally would not want to delete your work from here, but in case you do, you can reach out to VJ, and it will be looked into.

## Status codes

Given below is a list of possible status codes returned on uploading a file through the bot, and how they should be interpreted

|Code|Indication|
|---|---|
|200|All OK|
|400|Your file is corrupt, or was corrupted during the upload process. (This can happen if you used something shady to convert formats)|
|401|The bot's configuration is incorrect/incomplete. Ping VJ|
|500|Go to https://docs.naarivad.in/. <br> - If it says `Internal Server Error`, then the server is dead. <br> - If it displays the page correctly, then the file system has improper permissions.<br>In any case, ping VJ. But, annoy him more if it's an internal server error.|
|404|何 moment. This happens rarely and honestly I have no idea why. It indicates that the file is "uploaded" but isn't physically present on any of the servers. I've spent days debugging this but I can't repro when I _try_. Just ping me and I'll add it manually via SFTP|
|No response| - If the bot is offline, that's the problem. Ping VJ. <br> - If the bot is online, then your filename doesn't match the [format spec](filename_guidelines.md). Fix this and try again.

If a situation requires you to ping me, reply to the bot's message containing the status code, and provide relevant information, if any.

## Technical details
If you're interested in the inner workings of this system, and would like to know how it came to be, and how it evolved to it's current shape, then you can look at
this [gist](https://gist.github.com/darthshittious/0e9e0ca4db0d70c76104c3db2dc37772).
