# Most Active GitHub Users Counter

This CLI tool queries the GitHub GraphQL API for users and ranks them according to number of contributions. Several preset locations are provided.

**GitHub Token**

In order to make requests against the GitHub API one needs an access token, which can be created [here](https://github.com/settings/tokens). The token needs `read:org` and `read:user` permissions.

**Example usage (dev environment):**

```
go run *.go \
   --token paste-your-token-here \
   --preset worldwide \
   --amount 500 \
   --consider 1000 \
   --output csv \
   --file ./output.csv
```

## Contribution

Contributions are accepted. Please report issues or make pull requests against either `master` or [branch for the website](https://github.com/ashkulz/committers.top/tree/gh-pages) as appropriate.

*Please use the provided precommit hooks and run `go fmt`, `go vet` and `go lint` liberally.*

### Preset Changes

When sending PRs for changes to just `presets.go`, please ensure that you do the following (_replace `ashkulz` with your username in the links below_):

* Make sure that both your PR branch and the `gh-pages` branch on your fork are up-to-date with upstream (_be sure **not** to use "Copy the `master` branch only" option when forking_).
* Go to your fork's [daily update workflow](https://github.com/ashkulz/committers.top/actions/workflows/daily_update.yml) and trigger the workflow via "Run workflow" with your PR branch.
* A commit should get pushed to the [`gh-pages` branch](https://github.com/ashkulz/committers.top/tree/gh-pages) and verify that the changes look good.
* Reference the commit when making the PR, so that it's clear what the impact of merging the PR would be.

## FAQ

### Why is this running on a different domain?

The [original project](https://github.com/lauripiispanen/most-active-github-users-counter) by [@lauripiispanen](https://github.com/lauripiispanen) seems to have been archived. I also thought that `committers.top` was a more appropriate domain name and quickly grabbed it :grinning:

### Why am I not on this list?

This could be due to a number of things.

1) Firstly, GitHub API doesn't allow sorting by contributions, so instead it is first sorted by number of followers to get a larger list, which is then sorted by contributions. This means you need a minimum number of followers to be on this list. Each page shows the minimum number of followers needed.

2) You live in a city which is not included in the query. Unfortunately the query is free-text and not strictly geographical. Rural areas may be excluded from the list, but you can often remedy this by adding the country name to your location in GitHub.

3) You have mostly private commits. These are included too, but they are not listed on the main country page anymore. Instead you will find a subpage with a list that also has private contributions included. This arrangement is done to favor open source contributions over private contributions.

### Why is my contribution count not the same as on my GitHub profile?

Depending on your settings, your GitHub profile displays either only your public contributions or both your public and private ones. commits.top by default shows only public contributions, but public+private count is accessible on a subpage on each region page. You can use the [GitHub API GraphQL Explorer](https://developer.github.com/v4/explorer/) and run the following query to see what the API returns for your user:

```
query { 
  viewer { 
    login
    contributionsCollection {
      restrictedContributionsCount
      contributionCalendar {
        totalContributions
      }
    }
  }
}
```
