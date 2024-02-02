# Full Screen and Resizing

## Fit in Viewport

When you set the size of the canvas as follows:

```javacript
const sizes = {
    width: window.innerWidth,
    height: window.innerHeight
}
```

There will be a margin around the canvas and the page is scrollable. You can remove this multiple ways.

```css
/* An easy approach that will remove margin not scrolling*/

*{
    margin: 0;
    padding: 0;
}

/*An approach that removes margin and scroll*/
.webgl{
    position: fixed; /*Can use absolute as well depending on use case */
    top: 0;
    left: 0;
    outline: none; /*To prevent drag and drop outline */

}

/* If controls aren't enabled to capture scroll events. You will still be able to scroll beyond canvas temporarily.
To fix this hide the overflow*/

html,
body
{
    overflow: hidden;
}
```

## Handle Resize

Using vanilla JS, add resize event listener. Update sizes object. Update camera aspect and rerender.

```javascript
window.addEventListener('resize', () => {

    //Update Sizes
    sizes.width = window.innerWidth;
    sizes.height = window.innerHeight;

    //Update Camera
    camera.aspect = sizes.width / sizes.height

    camera.updateProjectionMatrix();

    //Update Renderer

    renderer.setSize(sizes.width, sizes.height)
})
```

## Pixel Ratio

Pixel Ratio is 1 ^ 2 pixels per software pixel. Pixel ratio is usually on weaker devices which is kinda counterintuitive.

To get pixel ratio:

```javascript
window.devicePixelRatio
```

To set pixel ratio on renderer you want to limit it to 2:

```javascript
renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))
```

Also add this on the resize event for the case that people move the window to a different screen.

## Handle Full Screen

```javascript

window.addEventListener('dblclick', () => {

    const fullscreenElement = document.fullscreenElement || document.webkitFullscreenElement
    if(!fullscreenElement){
        if (canvas.requestFullscreen){
            // We want canvas to be full screen so we request on canvas
            canvas.requestFullscreen()
        } else if (canvas.webkitRequestFullscreen){
            //Done to be compatible with Safari
            canvas.webkitRequestFullscreen()
        }
        
    } else {
        // Since canvas spans document. We remove full screen by exiting on document
        document.exitFullscreen()

        if (document.exitFullscreen){
            // Since canvas spans document. We remove full screen by exiting on document
            document.exitFullscreen()
        } else if (canvas.webkitExitFullscreen){
            //Done to be compatible with Safari
            document.webkitExitFullscreen()
        }
    }
})

```
