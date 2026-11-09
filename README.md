# Lineup

Type your city and what you're into; get one ranked list of hackathons, meetups,
workshops, conferences, coding contests and CTFs pulled live from 14 sites.

## Run it

You need [Node.js](https://nodejs.org) 18 or newer. Nothing to install.

- **Windows:** double-click `start.bat`. It starts the server and opens the page.
- **Anywhere:** run `node server.js`, then open <http://localhost:4321>.

Leave that window open while you use the site. Close it (or press Ctrl+C) to stop.
Set `PORT=5000` before starting if 4321 is taken.

## What it searches

| Site | What it adds | Needs your city |
| --- | --- | --- |
| Devfolio | Hackathons (strong in India) | |
| Devpost | Hackathons, mostly online, with prize pools | |
| MLH | Student hackathons | |
| Unstop | Hackathons, competitions, quizzes, workshops (India only) | |
| HackerEarth | Hackathons and coding challenges | |
| Hack Club | High-school hackathons | |
| Luma | Tech, AI and other community events near you | yes |
| Meetup | Local meetups | yes |
| Google Developer Groups | GDG events and DevFests near you | yes |
| confs.tech | Tech conferences worldwide | |
| CTFtime | Capture-the-flag security contests | |
| Codeforces, CodeChef, LeetCode | Upcoming coding contests | |

Google, Eventbrite and LinkedIn don't let programs read their listings, so the
page ends with links that open your search on each of them instead.

## How the ranking works

Every event gets up to 100 points (`lib/rank.js`):

- **Interests, up to 46.** A match in the title counts more than one in the tags
  or description; matching several interests adds a little.
- **Place, up to 25.** In your city beats a few hours away beats elsewhere in
  your country. Online events sit in the middle. In-person events farther than
  the distance you chose are left out.
- **Timing, up to 15.** Sooner ranks higher.
- **Extras, up to 14.** Sign-ups, prizes, free entry, and beginner-friendliness
  if you ticked that box.

Events whose registration has closed, or that fall outside your time window, are
dropped. The same event on two sites is shown once with both links. Events from
Luma and Meetup must mention one of your interests to appear at all, because
those sites list everything from yoga to book clubs.

## Files

```
server.js          the web server and the search endpoint
lib/sources.js     one small adapter per site
lib/rank.js        filtering, de-duplication and scoring
lib/topics.js      the interests and the words that count as a match
lib/geo.js         city lookup and distances
lib/net.js         fetching with a timeout and a 15-minute cache
public/            the page (index.html, styles.css, app.js)
```

To add a site, write a function in `lib/sources.js` that returns events in the
shape described at the top of that file, and add it to the `SOURCES` list.

## Good to know

- Your preferences and saved events stay in your browser. The server keeps
  nothing except a small file of city coordinates in `.cache/`.
- The first search for a new city takes 15 to 20 seconds while distances are
  worked out; after that, searches take a few seconds.
- These sites don't offer official search APIs, so Lineup reads the same public
  listings their own pages use. If a site changes its layout, that one source
  shows "no answer" and the rest keep working. Results are cached for 15 minutes
  to keep the load on those sites light; please keep it to personal use.
- Dates and details come from the listings. Check the organiser's page before
  you plan around an event.
