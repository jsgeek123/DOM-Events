# DOM Events

DOM Events is designed with two main goals:

1) To register event listeners and describe event flow through a tree structure.

2) To provide a common subset of the current event systems used in existing browsers. This is intended to foster interoperability of existing scripts and content.

## @ DOM Event Architecture

DOM Events are sent to notify that something interesting have taken place. Each event is represented by an object which is an instance of Event interface, and may have additional custom fields and/or functions used to get additional information about what happened.

##### $ Event Dispatch

Event objects are dispatched (sent) to an event target which is an element in the DOM.

##### $ Event Flow

Event flow describes how event objects propagate through the DOM ( Capturing and Bubbling ).

![Event Flow](https://raw.githubusercontent.com/mailtest954/DOM-Events/master/eventflow.jpg)

##### $ Event Capturing

In capturing phase, the Event object propagates through the target's ancestors from the Window to the target's parent. Event listeners registered for this phase must handle the event before it reaches its target. In this phase, the value of `event.eventPhase` is 1.


##### $ Event Target

In target phase, the Event object must arrive at the event object's event target. Event listeners registered for this phase must handle the event once it has reached its target. If the event type indicates that the event must not bubble, the event object must halt after completion of this phase. In this phase, the value of `event.eventPhase` is 2.

##### $ Event Bubbling

In this phase, the Event object propagates through the target's ancestors in reverse order, starting with the target's parent and ending with the Window. Event listeners registered for this phase must handle the event after it has reached its target. In this phase, the value of `event.eventPhase` is 3

And when the value of `event.eventPhase` is 0, It means none of the above three.

##### $ Default Actions and Cancelable Events

*Default Action* of an event is the action for which the event was created.

For example:

```html

<form name="eventForm" method="get" action="receiveEvent.php">
   <input type="submit" value="Submit">
</form>

```

Here, on clicking the submit button, the form will be submitted to the server. In this case, submission of the form to the server is the default action for the submit button.

*Cancellation of event* means to stop the default action. In the above example, we can cancel the default action (submission of form) using `preventDefault()` and then we can perform our own task and submit it manually.

##### $ Synchronous and Asynchronous Events

*Synchronous events* are treated as if they are in a virtual queue in first-in-first-out model ordered by sequence of occurrence with respect to other events, to change the DOM, and to user interaction.

*Asynchronous events* are dispatched when the results of the action are complete, with no relation to other events, to other changes in the DOM, nor to user interaction.

##### $ Trusted Events

Events that are generated by the user agent, either as a result of user interaction, or as a direct result of changes to the DOM, are *trusted event*.

Events generated by script through the `DocumentEvent.createEvent("Event")` method are generally called *untrusted (or custom) events*.

The `isTrusted` attribute of trusted events has a value of `true`, while untrusted events have a `isTrusted` attribute value of `false`.

##### $ Event Exceptions

Event operations can throw a DOMException as specified in their method descriptions.

*InvalidStateError*- Thrown when the `Event.type` was not specified by initializing the event before `dispatchEvent()` was called. Also thrown when the Event object provided to `dispatchEvent()` is already being dispatched.

*NotSupportedError*- Thrown when `DocumentEvent.createEvent()` is passed an Event interface that the implementation does not support.

##### $ Basic Event Interfaces

The basic event interfaces defined in the specification are fundamental to UI Events and it must always be supported by the implementation:

The Event interface and its following constants, methods and attributes:

```javascript

event.type - attribute

event.target - attribute

event.currentTarget - attribute

event.eventPhase - attribute

event.bubbles - attribute

event.cancelable - attribute

event.timeStamp - attribute

event.defaultPrevented - attribute

event.isTrusted - attribute

event.originalEvent - attribute

event.altKey - attribute

event.clientX - constant

event.clientY - constant

event.ctrlKey - attribute

event.metaKey - attribute

event.screenX - attribute

event.screenY - attribute

event.shiftKey - attribute

event.which - constant

event.charCode - constant

event.key - attribute

event.keyCode - constant

event.persisted - attribute for PageTransition event

event.relatedTarget - attribute

event.animationName - attribute for Animation event

event.elapsedTime - attribute for Animation and Transition event

event.propertyName - attribute for Transition event

event.deltaX - attribute for Mouse event (scroll amount on x-axis)

event.deltaY - attribute for Mouse event (scroll amount on y-axis)

event.deltaZ - attribute for Mouse event (scroll amount on z-axis)

event.deltaMode - attribute for Mouse event (returns one of pixels, lines or pages)

event.stopPropagation() - method

event.stopImmediatePropagation() - method

event.isPropagationStopped() - method

event.preventDefault() - method

event.isDefaultPrevented() - method

event.initEvent() - method

```

The `CustomEvent` interface and its following method and attribute:

```javascript

event.initCustomEvent() - method

event.detail - attribute

```

The `EventTarget` interface and its following methods:

```javascript

addEventListener() - method

removeEventListener() - method

dispatchEvent() - method

```

The `EventListener` interface and its `handleEvent` method

The Document interface's `createEvent()` method

The following chart describes the inheritance structure of the interfaces described in this specification.

![Event Flow](https://raw.githubusercontent.com/mailtest954/DOM-Events/master/eventInterface.jpg)

## @ List of Event Types

#### $ Mouse Events:

| Event Name    | Sync/Async | Bubbles | Event Target | Cancelable | Description                                                                                                                                                    |
| ------------- | ---------- | ------- | ------------ | ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| click         | Sync       | Yes     | Element      | Yes        | The event occurs when the user clicks on an element                                                                                                            |
| contextmenu   | Async      | Yes     | Element      | Yes        | The event occurs when the user right-clicks on an element to open a context menu	                                                                            |
| dblclick  	| Sync       | Yes     | Element      | No         | The event occurs when the user double-clicks on an element	                                                                                                    |
| mousedown     | Sync       | Yes     | Element      | Yes        | The event occurs when the user presses a mouse button over an element	                                                                                        |
| mouseup       | Sync       | Yes     | Element      | Yes        | The event occurs when a user releases a mouse button over an element                                                                                           |
| mouseout      | Sync       | Yes     | Element      | Yes        | The event occurs when a user moves the mouse pointer out of an element, or out of one of its children                                                          |
| mouseover     | Sync	     | Yes     | Element      | Yes        | The event occurs when the pointer is moved onto an element, or onto one of its children                                                                        |
| mousemove     | Sync       | Yes     | Element      | Yes        | The event occurs when the pointer is moving while it is over an element                                                                                        |
| mouseleave    | Sync       | No      | Element	  | No         | The event occurs when the pointer is moved out of an element                                                                                                   |
| mouseenter    | Sync       | No      | Element	  | No         | The event occurs when the pointer is moved onto an element                                                                                                     |
| wheel         | Async      | Yes     | Element      | Yes        | The event occurs when the user rolls the mouse wheel over an element                                                                                           |

#### $ Keyboard Events:

| Event Name    | Sync/Async | Bubbles | Event Target | Cancelable | Description                                                                                                                                                    |
| ------------- | ---------- | ------- | ------------ | ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| keydown       | Sync       | Yes     | Element      | Yes        | The event occurs when the user is pressing a key	                                                                                                            |
| keyup         | Sync       | Yes     | Element      | Yes        | The event occurs when the user releases a key	                                                                                                                |
| keypress      | Async      | Yes     | Element      | Yes        | The event occurs when the user presses a key                                                                                                                   |

#### $ Frame/Object Events:

| Event Name    | Sync/Async | Bubbles | Event Target               | Cancelable | Description                                                                                                                                      |
| ------------- | ---------- | ------- | -------------------------- | ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| abort         | Sync       | No      | Window, Element            | No         | The event occurs when the loading of a resource has been aborted                                                                                 |
| beforeunload  | Async      | No      | Window, Document, Element  | Yes        | The event occurs before the document is about to be unloaded                                                                                     |
| error         | Async      | Yes     | Window, Element            | No         | The event occurs when an error occurs while loading an external file                                                                             |
| hashchange    | Async      | Yes     | Window, Document           | Yes        | The event occurs when there has been changes to the anchor part of a URL                                                                         |
| load          | Async      | No      | Window, Document, Element  | No         | The event occurs when an object has loaded                                                                                                       |
| pageshow      |            | No      | Document                   | No         | The event occurs when the user navigates to a webpage and it is fired just after the load event                                                  |
| pagehide      |            | No      | Document                   | No         | The event occurs when the user navigates away from a webpage and it is fired just after unload event                                             |
| resize        | Sync       | No      | Window, Element            | No         | The event occurs when the document view is resized                                                                                               |
| scroll        | Async      | No      | Document, Element          | No         | The event occurs when an element's scrollbar is being scrolled                                                                                   |
| unload        | Sync       | No      | Window, Document, Element  | No         | The event occurs once a page has unloaded (for <body>)                                                                                           |

#### $ Form Events:

| Event Name    | Sync/Async | Bubbles | Event Target         | Cancelable | Description                                                                                                                                            |
| ------------- | ---------- | ------- | -------------------- | ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| blur          | Sync       | No      | Window, Element      | No         | The event occurs when an element loses focus                                                                                                           |
| change        | Async      | No      | Element              | Yes        | The event occurs when the content of a form element (the selection or the checked state) have changed (for input, keygen, select, and textarea)        |
| focus         | Sync       | No      | Window, Element      | No         | The event occurs when an element gets focus                                                                                                            |
| focusin       | Sync       | Yes     | Window, Element      | No         | The event occurs when an element is about to get focus                                                                                                 |
| focusout      | Sync       | Yes     | Window, Element      | No         | The event occurs when an element is about to lose focus                                                                                                |
| input         | Async      | No      | Element              | Yes        | The event occurs when an element gets user input                                                                                                       |
| invalid       | Async      | No      | Element              | Yes        | The event occurs when an element is invalid                                                                                                            |
| reset         | Async      | Yes     | Element              | Yes        | The event occurs when a form is reset                                                                                                                  |
| search        |            | No      | Element              | No         | The event occurs when the user writes something in a search field (for input of type search)                                                           |
| select        |            | Yes     | Element              | No         | The event occurs after the user selects some text (for input and textarea)                                                                             |
| submit        | Async      | Yes     | Element              | Yes        | The event occurs when a form is submitted                                                                                                              |

#### $ Drag Events:

| Event Name    | dataTransfer                                                                                                                                | Bubbles | Event Target                                       | Cancelable | Description                                                                                      |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------- | ------- | -------------------------------------------------- | ---------- | ------------------------------------------------------------------------------------------------ |
| drag          | Empty                                                                                                                                       | Yes     | Source Element                                     | Yes        | The event occurs when an element is being dragged                                                |
| dragend       | Empty                                                                                                                                       | Yes     | Source Element                                     | No         | The event occurs when the user has finished dragging an element                                  |
| dragenter     | Empty                                                                                                                                       | Yes     | Immediate user selection or the body element       | Yes        | The event occurs when the dragged element enters the drop target                                 |
| dragleave     | Empty                                                                                                                                       | Yes     | Previous target element                            | No         | The event occurs when the dragged element leaves the drop target                                 |
| dragover      | Empty                                                                                                                                       | Yes     | Current target element                             | Yes        | The event occurs when the dragged element is over the drop target                                |
| dragstart     | Contains source node unless a selection is being dragged, in which case it is empty; files returns any files included in the drag operation | Yes     | Source node                                        | Yes        | The event occurs when the user starts to drag an element                                         |
| drop          | getData() returns data set in dragstart event; files returns any files included in the drag operation                                       | Yes     | Current target element                             | Yes        | The event occurs when the dragged element is dropped on the drop target                          |

**Effects in Drag Operation:**

| effectAllowed                                                                     | dropEffect  |
| --------------------------------------------------------------------------------- | ----------- |
| none                                                                              | none        |
| copy, copyLink, copyMove, all                                                     | copy        |
| link, linkMove                                                                    | link        |
| move                                                                              | move        |
| uninitialized when what is being dragged is a selection from a text field         | move        |
| uninitialized when what is being dragged is a selection                           | copy        |
| uninitialized when what is being dragged is an a element with an href attribute   | link        |
| Any other case                                                                    | copy        |

**Effect Description:**

| Effect Name | Description                                                                   |
| ----------- | ------------------------------------------------------------------------------|
| copy        | Data will be copied if dropped here.                                          |
| link        | Data will be linked if dropped here.                                          |
| move        | Data will be moved if dropped here.                                           |
| none        | No operation allowed, dropping here will cancel the drag-and-drop operation.  |

#### $ Clipboard Events:

| Event Name | Event Target       | Bubbles | Cancelable | Description                                                        |
| ---------- | ------------------ | ------- | ---------- | ------------------------------------------------------------------ |
| copy       | Document, Element  | Yes     | Yes        | The event occurs when the user copies the content of an element    |
| cut        | Document, Element  | Yes     | Yes        | The event occurs when the user cuts the content of an element      |
| paste      | Document, Element  | Yes     | Yes        | The event occurs when the user pastes some content in an element   |

#### $ Print Events

| Event Name   | Event Target | Bubbles | Cancelable | Description                                                                                      |
| ------------ | ------------ | ------- | ---------- | ------------------------------------------------------------------------------------------------ |
| afterprint   | window       | No      | No         | The event occurs when a page has started printing, or if the print dialogue box has been closed  |
| beforeprint  | window       | No      | No         | The event occurs when a page is about to be printed                                              |

#### $ Media Events

| Event Name        | Event Target     | Bubbles | Cancelable | Description                                                                                                                     |
| ----------------- | ---------------- | ------- | ---------- | ------------------------------------------------------------------------------------------------------------------------------- |
| abort             | Audio, Video     | No      | No         | The event occurs when the loading of a media is aborted                                                                         |
| canplay           | Audio, Video     | No      | No         | The event occurs when the browser can start playing the media (when it has buffered enough to begin)                            |
| canplaythrough    | Audio, Video     | No      | No         | The event occurs when the browser can play through the media without stopping for buffering                                     |
| durationchange    | Audio, Video     | No      | No         | The event occurs when the duration of the media is changed                                                                      |
| ended             | Audio, Video     | No      | No         | The event occurs when the media has reach the end (useful for messages like "thanks for listening")                             |
| error             | Audio, video     | No      | No         | The event occurs when an error occurred during the loading of a media file                                                      |
| loadeddata        | Audio, Video     | No      | No         | The event occurs when media data is loaded                                                                                      |
| loadedmetadata    | Audio, Video     | No      | No         | The event occurs when meta data (like dimensions and duration) are loaded                                                       |
| loadstart         | Audio, Video     | No      | No         | The event occurs when the browser starts looking for the specified media                                                        |
| pause             | Audio, Video     | No      | No         | The event occurs when the media is paused either by the user or programmatically                                                |
| play              | Audio, Video     | No      | No         | The event occurs when the media has been started or is no longer paused                                                         |
| playing           | Audio, Video     | No      | No         | The event occurs when the media is playing after having been paused or stopped for buffering                                    |
| progress          | Audio, Video     | No      | No         | The event occurs when the browser is in the process of getting the media data (downloading the media)                           |
| ratechange        | Audio, Video     | No      | No         | The event occurs when the playing speed of the media is changed                                                                 |
| seeked            | Audio, Video     | No      | No         | The event occurs when the user is finished moving/skipping to a new position in the media                                       |
| seeking           | Audio, Video     | No      | No         | The event occurs when the user starts moving/skipping to a new position in the media                                            |
| stalled           | Audio, Video     | No      | No         | The event occurs when the browser is trying to get media data, but data is not available                                        |
| suspend           | Audio, video     | No      | No         | The event occurs when the browser is intentionally not getting media data                                                       |
| timeupdate        | Audio, Video     | No      | No         | The event occurs when the playing position has changed (like when the user fast forwards to a different point in the media)     |
| volumechange      | Audio, Video     | No      | No         | The event occurs when the volume of the media has changed (includes setting the volume to "mute")                               |
| waiting           | Audio, Video     | No      | No         | The event occurs when the media has paused but is expected to resume (like when the media pauses to buffer more data)           |

#### $ Animation Events

| Event Name          | Event Target     | Bubbles | Cancelable | Description                                            |
| ------------------- | ---------------- | ------- | ---------- | ------------------------------------------------------ |
| animationend        | Element          | Yes     | No         | The event occurs when a CSS animation has completed    |
| animationiteration  | Element          | Yes     | No         | The event occurs when a CSS animation is repeated      |
| animationstart      | Element          | Yes     | No         | The event occurs when a CSS animation has started      |

#### $ Transition Events

| Event Name          | Event Target     | Bubbles | Cancelable | Description                                            |
| ------------------- | ---------------- | ------- | ---------- | ------------------------------------------------------ |
| transitionend       | Element          | Yes     | Yes        | The event occurs when a CSS transition has completed   |

#### $ Touch Events

| Event Name          | Sync/Async | Event Target       | Bubbles | Cancelable | Description                                                    |
| ------------------- | ---------- | ------------------ | ------- | ---------- | -------------------------------------------------------------- |
| touchstart          | Sync       | Document, Element  | Yes     | Yes        | The event occurs when a finger is placed on a touch screen     |
| touchend            | Sync       | Document, Element  | Yes     | Yes        | The event occurs when a finger is removed from a touch screen  |
| touchmove           | Sync       | Document, Element  | Yes     | Yes        | The event occurs when a finger is dragged across the screen    |
| touchcancel         | Sync       | Document, Element  | Yes     | No         | The event occurs when the touch is interrupted                 |

#### $ Misc Events

| Event Name       | Event Target     | Bubbles | Cancelable | Description                                                          |
| ---------------- | ---------------- | ------- | ---------- | -------------------------------------------------------------------- |
| online           | Body             | No      | No         | The event occurs when the browser starts to work online              |
| offline          | Body             | No      | No         | The event occurs when the browser starts to work offline             |
| show             | Menu             | No      | No         | The event occurs when a menu element is shown as a context menu      |
| storage          | Window           | No      | No         | The event occurs when a Web Storage area is updated                  |
| toggle           | Details          | No      | No         | The event occurs when the user opens or closes the details element   |
| DOMContentLoaded | Document         | Yes     | Yes        | The document has finished loading (but not its dependent resources)  |

**NB:**

1) *Synchronous/Asynchronous*, *Bubble*, and *Cancelable* are browser dependents and therefor their values may change from browser to browser.

2) All *Events* may not work in all browsers.

# License ([MIT](https://opensource.org/licenses/MIT))

Copyright (c) 2015

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:


The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.


THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.  IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
