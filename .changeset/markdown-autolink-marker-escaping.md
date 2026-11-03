---
'@lexical/markdown': patch
---

[fix] Write the markdown markers in an autolink's URL literally, so exporting `https://x.com/user_name/status/1` no longer produces `https://x.com/user\_name/status/1`

An autolink is written bare rather than as `[text](url)`, so its text is the URL. Escaping `*`, `_`, `` ` ``, `~` or `\` there put a backslash inside the URL, which is not a URL character, so nothing autolinked the exported text again: `AutoLinkPlugin` stopped matching at the backslash and then discarded even that match, because the character after it is not a separator. Markers in an autolink's text are now written as-is. Character references and the bracket guards that keep an autolink from re-parsing as a link are unchanged, as is the escaping of text beside an autolink.

Verify: round-trip a document containing an autolinked URL with a `_` or `*` in it through `$convertToMarkdownString`; the URL is unchanged and is autolinked again on import.
