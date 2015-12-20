# DOM Events

DOM Events is designed with two main goals:

1) To register event listeners and describe event flow through a tree structure.

2) To provide a common subset of the current event systems used in existing browsers. This is intended to foster interoperability of existing scripts and content.

##DOM Event Architecture

DOM Events are sent to notify that something interesting have taken place. Each event is represented by an object which is an instance Event interface, and may have additional custom fields and/or functions used to get additional information about what happened.

#####Event Dispatch

Event objects are dispatched (sent) to an event target which is an element in the DOM.

#####Event Flow

Event flow describes how event objects propagate through the DOM (Capturing and Bubbling).

![Event Flow](https://raw.githubusercontent.com/mailtest954/DOM-Events/master/eventflow.png)

######Event Capturing

In capturing phase, the event object propagates through the target's ancestors from the Window to the target's parent. Event listeners registered for this phase must handle the event before it reaches its target. In this phase, the value of event.eventPhase is 1.


#####Event Target

In target phase, the event object must arrive at the event object's event target. Event listeners registered for this phase must handle the event once it has reached its target. If the event type indicates that the event must not bubble, the event object must halt after completion of this phase. In this phase, the value of event.eventPhase is 2.

#####Event Bubbling

In this phase, the event object propagates through the target's ancestors in reverse order, starting with the target's parent and ending with the Window. Event listeners registered for this phase must handle the event after it has reached its target. In this phase, the value of event.eventPhase is 3

And when the value of event.eventPhase is 0, It means none of the above three.

#####Default Actions and Cancelable Events

*Default Action* of an event is the action for which the event was created.

For example:

```html

<form name="eventForm" method="get" action="receiveEvent.php">
   <input type="submit" value="Submit">
</form>

```

Here, on clicking the submit button, the form will be submitted to the server. In this case, submission of the form to the server is the default action for the submit button.

*Cancellation of event* means to stop the default action. In the above example, we can cancel the default action (submission of form) using preventDefault() and then we can perform our own task and submit it manually.

#####Synchronous and Asynchronous Events

*Synchronous events* are treated as if they are in a virtual queue in first-in-first-out model ordered by sequence of occurrence with respect to other events, to change the DOM, and to user interaction.

*Asynchronous events* are dispatched when the results of the action are complete, with no relation to other events, to other changes in the DOM, nor to user interaction.

#####Trusted Events

Events that are generated by the user agent, either as a result of user interaction, or as a direct result of changes to the DOM, are *trusted event*.

Events generated by script through the DocumentEvent.createEvent("Event") method are generally called *untrusted (or custom) events*.

The isTrusted attribute of trusted events has a value of true, while untrusted events have a isTrusted attribute value of false.

#####Event Exceptions

Event operations can throw a DOMException as specified in their method descriptions.

*InvalidStateError*- Thrown when the Event.type was not specified by initializing the event before dispatchEvent was called. Also thrown when the Event object provided to dispatchEvent is already being dispatched.

*NotSupportedError*- Thrown when DocumentEvent.createEvent() is passed an Event interface that the implementation does not support.

#####Basic Event Interfaces

The basic event interfaces defined in the specification are fundamental to UI Events and it must always be supported by the implementation:

The Event interface and its following constants, methods and attributes:
<br>
&nbsp;&nbsp;&nbsp;**NONE** - constant
<br>
&nbsp;&nbsp;&nbsp;**CAPTURING_PHASE** - constant
<br>
&nbsp;&nbsp;&nbsp;**AT_TARGET** - constant
<br>
&nbsp;&nbsp;&nbsp;**BUBBLING_PHASE** - constant
<br>
&nbsp;&nbsp;&nbsp;**type** - attribute
<br>
&nbsp;&nbsp;&nbsp;**target** - attribute
<br>
&nbsp;&nbsp;&nbsp;**currentTarget** - attribute
<br>
&nbsp;&nbsp;&nbsp;**eventPhase** - attribute
<br>
&nbsp;&nbsp;&nbsp;**bubbles** - attribute
<br>
&nbsp;&nbsp;&nbsp;**cancelable** - attribute
<br>
&nbsp;&nbsp;&nbsp;**timeStamp** - attribute
<br>
&nbsp;&nbsp;&nbsp;**defaultPrevented** - attribute
<br>
&nbsp;&nbsp;&nbsp;**isTrusted** - attribute
<br>
&nbsp;&nbsp;&nbsp;**stopPropagation()** - method
<br>
&nbsp;&nbsp;&nbsp;**stopImmediatePropagation()** - method
<br>
&nbsp;&nbsp;&nbsp;**preventDefault()** - method
<br>
&nbsp;&nbsp;&nbsp;**initEvent()** - method

The CustomEvent interface and its following method and attribute:
<br>
&nbsp;&nbsp;&nbsp;**initCustomEvent()** - method
<br>
&nbsp;&nbsp;&nbsp;**detail** - attribute

The EventTarget interface and its following methods:
<br>
&nbsp;&nbsp;&nbsp;**addEventListener()** - method
<br>
&nbsp;&nbsp;&nbsp;**removeEventListener()** - method
<br>
&nbsp;&nbsp;&nbsp;**dispatchEvent()** - method

The **EventListener** interface and its **handleEvent** method

The **Document** interface's **createEvent** method

The following chart describes the inheritance structure of the interfaces described in this specification.

![Event Flow](https://raw.githubusercontent.com/mailtest954/DOM-Events/master/eventInterface.png)

#####List of Event Types

**Mouse Events**

| Event Type    | Sync/Async | Bubbles | Event Target Types | Cancelable | Description                                                                                                                                                    |
| ------------- | ---------- | ------- | ------------------ | ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| click         | Sync       | Yes     | Element            | Yes        | The event occurs when the user clicks on an element                                                                                                            |
| contextmenu   | Async      | Yes     | Element            | Yes        | The event occurs when the user right-clicks on an element to open a context menu	                                                                              |
| dblclick  	| Sync       | Yes     | Element            | No         | The event occurs when the user double-clicks on an element	                                                                                                  |
| mousedown     | Sync       | Yes     | Element            | Yes        | The event occurs when the user presses a mouse button over an element	                                                                                      |
| mouseup       | Sync       | Yes     | Element            | Yes        | The event occurs when a user releases a mouse button over an element                                                                                           |
| mouseout      | Sync       | Yes     | Element            | Yes        | The event occurs when a user moves the mouse pointer out of an element, or out of one of its children                                                          |
| mouseover     | Sync	     | Yes     | Element            | Yes        | The event occurs when the pointer is moved onto an element, or onto one of its children                                                                        |
| mousemove     | Sync       | Yes     | Element            | Yes        | The event occurs when the pointer is moving while it is over an element                                                                                        |
| mouseleave    | Sync       | No      | Element	        | No         | The event occurs when the pointer is moved out of an element                                                                                                   |
| mouseenter    | Sync       | No      | Element	        | No         | The event occurs when the pointer is moved onto an element                                                                                                     |
| wheel         | Async      | Yes     | Element            | Yes        | The event occurs when the user rolls the mouse wheel over an element                                                                                           |

**Keyboard Events**

| Event Type    | Sync/Async | Bubbles | Event Target Types | Cancelable | Description                                                                                                                                                    |
| ------------- | ---------- | ------- | ------------------ | ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| keydown       | Sync       | Yes     | Element            | Yes        | The event occurs when the user is pressing a key	                                                                                                              |
| keyup         | Sync       | Yes     | Element            | Yes        | The event occurs when the user releases a key	                                                                                                              |
| keypress      | Async      | Yes     | Element            | Yes        | The event occurs when the user presses a key                                                                                                                   |

**Frame/Object Events**

| Event Type    | Sync/Async | Bubbles | Event Target Types         | Cancelable | Description                                                                                                                                            |
| ------------- | ---------- | ------- | -------------------------- | ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| abort         | Sync       | No      | Window, Element            | No         | The event occurs when the loading of a resource has been aborted                                                                                       |
| beforeunload  | Async      | No      | Window, Document, Element  | Yes        | The event occurs before the document is about to be unloaded                                                                                           |
| error         | Async      | Yes     | Window, Element            | No         | The event occurs when an error occurs while loading an external file                                                                                   |
| hashchange    | Async      | Yes     | Window, Document           | Yes        | The event occurs when there has been changes to the anchor part of a URL                                                                               |
| load          | Async      | No      | Window, Document, Element  | No         | The event occurs when an object has loaded                                                                                                             |
| pageshow      |            | No      | Document                   | No         | The event occurs when the user navigates to a webpage and it is fired just after the load event                                                        |
| pagehide      |            | No      | Document                   | No         | The event occurs when the user navigates away from a webpage and it is fired just after unload event                                                   |
| resize        | Sync       | No      | Window, Element            | No         | The event occurs when the document view is resized                                                                                                     |
| scroll        | Async      | No      | Document, Element          | No         | The event occurs when an element's scrollbar is being scrolled                                                                                         |
| unload        | Sync       | No      | Window, Document, Element  | No         | The event occurs once a page has unloaded (for <body>)                                                                                                 |

**Form Events**

| Event Type    | Sync/Async | Bubbles | Event Target Types         | Cancelable | Description                                                                                                                                            |
| ------------- | ---------- | ------- | -------------------------- | ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| blur          | Sync       | No      | Window, Element            | No         | The event occurs when an element loses focus                                                                                                           |
| change        | Async      | No      | Element                    | Yes        | The event occurs when the content of a form element (the selection or the checked state) have changed (for input, keygen, select, and textarea)        |
| focus         | Sync       | No      | Window, Element            | No         | The event occurs when an element gets focus                                                                                                            |
| focusin       | Sync       | Yes     | Window, Element            | No         | The event occurs when an element is about to get focus                                                                                                 |
| focusout      | Sync       | Yes     | Window, Element            | No         | The event occurs when an element is about to lose focus                                                                                                |
| input         | Async      | No      | Element                    | Yes        | The event occurs when an element gets user input                                                                                                       |
| invalid       | Async      | No      | Element                    | Yes        | The event occurs when an element is invalid                                                                                                            |
| reset         | Async      | Yes     | Element                    | Yes        | The event occurs when a form is reset                                                                                                                  |
| search        |            | No      | Element                    | No         | The event occurs when the user writes something in a search field (for input of type search)                                                               |
| select        |            | Yes     | Element                    | No         | The event occurs after the user selects some text (for input and textarea)                                                                         |
| submit        | Async      | Yes     | Element                    | Yes        | The event occurs when a form is submitted                                                                                                              |

**Drag Events**

| Event Type    | Sync/Async | Bubbles | Event Target Types         | Cancelable | Description                                                                                                                                            |
| ------------- | ---------- | ------- | -------------------------- | ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| drag          |
|  |
|  |
|  |
|  |
|  |
|  |
|  |
|  |
|  |






