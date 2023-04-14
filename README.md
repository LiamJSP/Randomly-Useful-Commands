# Randomly Useful Commands

This is just a collection of very random terminal commands I've authored for various purposes. Hopefully they can save others time.


## Javascript Dev

### React/GatsbyJS

**Filter out an unwanted MASSIVE stack trace from Jest output:**

The following command pipes the output from "yarn test" (replace with whatever you'd like), it inserts newlines before the target text (in this case "at " - the word 'at' with a space following) - this is because true newlines are not always used, sometimes text autowraps or has different newline characters etc. Also when piping I've found sometimes newlines disappear. So I add possibly unnecessary additional newlines, then remove all lines starting with the target text, then finally remove all unnecessary newlines.

There may be a better way to do this, but jest --silent was not it for me. jest --noStackTrace ended up helping a lot but doesn't stop all traces/error dumps, so leaving this for posterity.

*Windows Powershell:*

``yarn test 2>&1 | Out-String | ForEach-Object { $withNewlines = $_ -replace "(?m)(?=at\s)", "`n"; $filtered = $withNewlines -replace "(?m)^\s*at\s.*$"; $cleaned = $filtered -replace "(?m)^\s*$", ""; if ($cleaned.Trim() -ne "") { $cleaned } }``

*Bash:*

``yarn test 2>&1 | perl -pe 's/(?=at\s)/\n/g' | awk '!/^\s*at\s/ && !/^\s*$/'``