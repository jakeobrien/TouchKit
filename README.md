TouchKit
====

TouchKit aims to make touch handling in Unity more sane. Touches in TouchKit are actual objects as opposed to Structs like Unity uses by default. The advantage to this is that touch tracking becomes orders of magnitude more simple. You can retain a touch that began and since you are only holding on to a pointer (as opposed to a Struct) the properites of that touch will be updating as the touch changes. TouchKit is not all the useful for simple, single-tap processing. It's usefulness is in detecting and managing gestures (hence the name TouchKit).

TouchKit also allows gesture recognizers to define a Rect in which to do detection. If a touch doesn't orginate in the Rect the touches won't be passed on to the recognizer.


Included Gesture Recognizers
---

TouchKit comes with a few built in recognizers to get you started and to serve as an example for how to make your own. Included are the following:
* **Tap Recognizer**: detects one or more taps from one or more fingers
* **Long Press Recognizer**: detects long-presses with configurable duration and allowed movement. Fires when the duration passes and when the gesture is complete.
* **Button Recognizer**: generic button recognizer designed to work with any 2D sprite system at all
* **Pan Recognizer**: detects a pan gesture (one or more fingers down and moving around the screen)
* **Swipe Recognizer**: detects swipes in the four cardinal directions
* **Pinch Recognizer**: detects pinches and reports back the delta scale
* **Rotation Recognizer**: detects two finger rotation and reports back the delta rotation
* **One Finger Rotation Recognizer**: pass it a target object's center position and it will report back the delta rotation of a single finger


How Do I Use TouchKit?
-----

If you are just using the built in recognizers, check out the demo scene which shows how to use them. You will want to use Unity's script execution order (Edit -> Project Settings -> Script Execution Order) to ensure that TouchKit executes before your other scripts. This is just to ensure you have your input when the Update method on your listening objects runs. If you want to make your own (and feel free to send pull requests if you make any generic recognizers!) all you have to do is subclass **TKAbstractGestureRecognizer** and implement the three methods: touchesBegan, touchesMoved and touchesEnded. Recognizers can be discrete (they only complete once like a tap or long touch) or continous (they can fire continuously like a pan). Recognizers use the **state** veriable to determine if they are still active, completed or failed. If you set the state to **Recognized**, the completion event is fired automatically for you and the recognizer is reset in preparation for the next set of touches.

Starting with **touchesBegan**, if you find a touch that look interesting you can add it to the **_trackingTouches** List and set the **state** to **Began**. By adding the touch to the **_trackingTouches** List you are telling TouchKit that you want to receive all future touch events that happen with that touch. As the touch moves, **touchesMoved** will be called where you can look at the touches and decide if the gesture is still possible. If it isn't, set the state to **Failed** and TouchKit will reset the recognizer for you.

Using the Tap recognizer as an example, the flow would be like the following:
* **touchesBegan**: add the touch to **_trackingTouches** signifying we are watching it. Set the state to **Began** and record the current time (we don't want a press that is too long to be considered a tap)
* **touchesMoved**: check the deltaMovement of the touch and if it moves too far set the **state** to **Failed** signifying we the gesture failed and we are done with the touch
* **touchesEnded**: if not too much time has elapsed we successfully recognized the gesture so we set **state** to **Recognized** which will fire the event for us. If too much time elapsed we set **state** to **Failed**



UI Touch Handling Replacement
----

TouchKit can be used for any and all touch input in your game. The **TKButtonRecognizer** class has been designed to work with any sprite solution for button touch handling. This lets you keep your input totally separate from your rendering. It implements the same setup that iOS does: a highlighted button expands its hit rect for better useability.


License
----
For any developers just wanting to use TouchKit in their games go right ahead.  You can use TouchKit in any and all games either modified or unmodified.  In order to keep the spirit of this open source project it is expressly forbid to sell or commercially distribute TouchKit outside of your games. You can freely use it in as many games as you would like but you cannot commercially distribute the source code either directly or compiled into a library outside of your game.

Feel free to include a "prime31 inside" logo on your about page, web page, splash page or anywhere else your game might show up if you would like.  
[small](http://prime31.com/assets/images/prime31InsideSmall.png) or 
[larger](http://prime31.com/assets/images/prime31Inside.png) or
[huge](http://prime31.com/assets/images/prime31InsideHuge.png)
