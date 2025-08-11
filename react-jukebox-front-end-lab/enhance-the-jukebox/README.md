<h1>
  <span class="headline">Jukebox Front-End Lab</span>
  <span class="subhead">Enhance the Jukebox</span>
</h1>

## Level Up: Enhance your jukebox

Ready to take your Reactville jukebox to the next level? As you've built a solid foundation for managing and displaying tracks, you can enhance the user experience by integrating more interactive and visual elements. Consider adding the following features to your front-end application:

### Display cover art

Show cover art for each track in the UI.

- **Update the `Track` model**: Ensure your back-end model includes a `coverArtUrl` attribute.
- **Modify the `TrackForm` component**: Update the form used to add or update tracks to include an input field for the cover art URL.
- **Display in UI**: Modify the `TrackList` and `NowPlaying` components to display the cover art `img` associated with each track.

### Play audio clips

This is an ***advanced Level Up*** that requires the help of an outside API for music or sound clips.

Allow users to preview tracks by playing audio clips directly in the browser.

- **Update the `Track` model**: Include a `soundClipUrl` attribute in your back-end model if not already present.
- **Modify the `TrackForm` component**: Ensure there is an input field in the form for users to add or update the URL of the audio clip.
- **Audio playback in UI**: Implement an audio player in the `NowPlaying` component that uses the HTML `<audio>` element to play the sound clip.

Here's an example of how to integrate a basic audio player component in React:

```jsx
<audio controls src={currentTrack.soundClipUrl}>
  Your browser does not support the audio element.
</audio>
```
