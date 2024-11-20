## Testing

Run the server with `hugo server`

## Content Creation

Create new content with `hugo new content /content/location/to/newcontent`

Quick refresher if you haven't setup the hugo in a while.
`hugo server -D` will create a local hugo server to confirm your configuration is desired. The `-D` flag will include all files labelled as drafts.

If you want to host the temporary hugo server on an ip other than localhost, you can use
```sh
hugo server --bind 192.168.1.20 --baseURL http://192.168.1.20/
```

If you're installing the hugo repo to a new computer, you will need to redownload the submodule things. This can be done with
```sh
git submodule init
git submodule update
```
