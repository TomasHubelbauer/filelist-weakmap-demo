# `FileList` `WeakMap` demostration

Attempting to reproduce an issue I came across at work.

How to reproduce (note that the uploading is simulated with delays):

- Open `index.html` in your browser (as a file via the `file:` protocol is fine)
- Select multiple files using the `input[type="file]`
- Wait for the first selected file to start uploading
- Cancel the second file as the first one is uploading
- Wait for the first file's faux-upload to finish
- Observe no other files following the one that was canceled start uploading

The last step doesn't actually happen, but first:

Initially my hunch was that when removing the corresponding entry from `uplads`,
the `File` instance gets garbage collected, because there is no other reference
to it any longer, which breaks the `FileList`'s iterator and it short-circuits,
whereas with an indexer-based enumeration, there is no underlying collection
being mutated and hence, that works.

I've prototyped both solutions here though, and they both work…
So now I'm left scratching my head what was it that actually was going wrong at
work and why changing from an iterator-based access to an indexed-based one made
it work.
