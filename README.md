# Repo for testing GraphQL query on issue reactions.

I was curious about how to find out which issues (in a given repository) have a reaction (like etc.) by me. 
I created this repo to test my solution. 

This repository contains only one empty file and this README. Besides this, I used a separate account to open two issues here.
Then I reacted on [the first one](https://github.com/paolo42/testing-issue-reaction-filtering/issues/1) with an "eyes" emoticon.
The [second issue](https://github.com/paolo42/testing-issue-reaction-filtering/issues/2) has no reaction from me.

I used Github GraphQL API explorer on URL [https://developer.github.com/v4/explorer/](https://developer.github.com/v4/explorer/) with this query:

```
{
  viewer {
    login
  }
  repository(name: "testing-issue-reaction-filtering", owner: "paolo42") {
    issues (first: 10) {
      edges {
        node {
          url,
          reactions {
            viewerHasReacted
          }
        }
      }
    }
  }
}
```

This lists first 10 issues in the repository 
[paolo42/testing-issue-reaction-filtering](https://github.com/paolo42/testing-issue-reaction-filtering)
(or less if not enough issues are found). For every issue it returns issue URL and a boolean field `viewerHasReacted` 
which is true only for issues with my reaction.

This is API response:

```
{
  "data": {
    "viewer": {
      "login": "paolo42"
    },
    "repository": {
      "issues": {
        "edges": [
          {
            "node": {
              "url": "https://github.com/paolo42/testing-issue-reaction-filtering/issues/1",
              "reactions": {
                "viewerHasReacted": true
              }
            }
          },
          {
            "node": {
              "url": "https://github.com/paolo42/testing-issue-reaction-filtering/issues/2",
              "reactions": {
                "viewerHasReacted": false
              }
            }
          }
        ]
      }
    }
  }
}
```

## See also 

* [https://github.community/t5/How-to-use-Git-and-GitHub/Where-can-I-see-my-reactions-or-comments-to-a-issue/m-p/46616/highlight/true#M10467](https://github.community/t5/How-to-use-Git-and-GitHub/Where-can-I-see-my-reactions-or-comments-to-a-issue/m-p/46616/highlight/true#M10467)
* [https://developer.github.com/v4/guides/forming-calls/](https://developer.github.com/v4/guides/forming-calls/)
* Github API docs for [repository](https://developer.github.com/v4/object/repository/),
[issue](https://developer.github.com/v4/object/issue/)
and [reaction](https://developer.github.com/v4/object/reaction/) object.
