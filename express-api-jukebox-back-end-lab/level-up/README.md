<h1>
  <span class="headline">Jukebox Back-End Lab</span>
  <span class="subhead">README</span>
</h1>

## Level Up

As you become more comfortable with the basics of your Jukebox API, you can enhance the application's functionality and user experience by adding more features. Here are some Level Up features that will not only make your application more robust but also more engaging:

### Expanded track model

Expand the `Track` model to include additional properties that add to the track information and user experience:

- **Cover art URL**: Include a URL to an image for the cover art of the album the track is on.
- **Sound clip URL**: Provide a link to a short sound clip of the track.

**A third-party music API may help provide this data, but using one is optional.**

### Updated track model entity

| Field       | Type   | Required | Description                                           |
| ----------- | ------ | -------- | ----------------------------------------------------- |
| title       | String | Yes      | The title of the track.                               |
| artist      | String | Yes      | The artist of the track.                              |
| coverArtUrl | String | No       | URL to the track's cover art image. This is optional. |
| soundClipUrl| String | No       | URL to a sound clip of the track. This is optional.   |

### Integration with external music API

- Integrate with an external music API to fetch sound clips and album art for each track. This could involve:
  - Researching and choosing an appropriate music API that provides this functionality - don't pay for one!
  - Implementing API calls from your backend to retrieve the URL to a sound clip or album art when adding or updating tracks.

Happy Coding!
!
