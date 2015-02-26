##Handling Keyboard Input

###Specifying the Input Method Type

```xml
android:inputType="textPassword|textCapSentences|textAutoCorrect"
```

Most soft input methods provide a user action button in the bottom corner that's appropriate for the current text field. By default, the system uses this button for either a Next or Done action unless your text field allows multi-line text (such as with ``android:inputType="textMultiLine"``), in which case the action button is a carriage return. However, you can specify additional actions that might be more appropriate for your text field, such as Send or Go.To specify the keyboard action button, use the ``android:imeOptions`` attribute with an action value such as ``"actionSend"`` or ``"actionSearch"``. 

You can then listen for presses on the action button by defining a ``TextView.OnEditorActionListener`` for the ``EditText`` element. In your listener, respond to the appropriate IME action ID defined in the ``EditorInfo`` class, such as ``IME_ACTION_SEND``. For example:

```java
EditText editText = (EditText) findViewById(R.id.search);
editText.setOnEditorActionListener(new OnEditorActionListener() {
    @Override
    public boolean onEditorAction(TextView v, int actionId, KeyEvent event) {
        boolean handled = false;
        if (actionId == EditorInfo.IME_ACTION_SEND) {
            sendMessage();
            handled = true;
        }
        return handled;
    }
});
```

###Handling Input Method Visibility
Show the Input Method When the Activity Starts,Although Android gives focus to the first text field in your layout when the activity starts, it does not show the input method. This behavior is appropriate because entering text might not be the primary task in the activity. However, if entering text is indeed the primary task (such as in a login screen), then you probably want the input method to appear by default.To show the input method when your activity starts, add the ``android:windowSoftInputMode`` attribute to the ``<activity>`` element with the ``"stateVisible"`` value.

If there is a method in your activity's lifecycle where you want to ensure that the input method is visible, you can use the InputMethodManager to show it.

```java
public void showSoftKeyboard(View view) {
    if (view.requestFocus()) {
        InputMethodManager imm = (InputMethodManager)
                getSystemService(Context.INPUT_METHOD_SERVICE);
        imm.showSoftInput(view, InputMethodManager.SHOW_IMPLICIT);
    }
}
```

When the input method appears on the screen, it reduces the amount of space available for your app's UI. The system makes a decision as to how it should adjust the visible portion of your UI, but it might not get it right. To ensure the best behavior for your app, you should specify how you'd like the system to display your UI in the remaining space.To declare your preferred treatment in an activity, use the ``android:windowSoftInputMode`` attribute in your manifest's ``<activity>`` element with one of the ``"adjust"`` values.For example, to ensure that the system resizes your layout to the available space¡ªwhich ensures that all of your layout content is accessible (even though it probably requires scrolling)¡ªuse ``"adjustResize"``.

###Supporting Keyboard Navigation
Users can also navigate your app using the arrow keys on a keyboard (the behavior is the same as when navigating with a D-pad or trackball). The system provides a best-guess as to which view should be given focus in a given direction based on the layout of the views on screen. Sometimes, however, the system might guess wrong.If the system does not pass focus to the appropriate view when navigating in a given direction, specify which view should receive focus with the following attributes:``android:nextFocusUp``,``android:nextFocusDown``,``android:nextFocusLeft``,``android:nextFocusRight``.Each attribute designates the next view to receive focus when the user navigates in that direction, as specified by the view ID.

```xml
<Button
    android:id="@+id/button1"
    android:nextFocusRight="@+id/button2"
    android:nextFocusDown="@+id/editText1"
    ... />
```

###Handling Keyboard Actions
When the user gives focus to an editable text view such as an EditText element and the user has a hardware keyboard attached, all input is handled by the system. If, however, you'd like to intercept or directly handle the keyboard input yourself, you can do so by implementing callback methods from the ``KeyEvent.Callback`` interface, such as ``onKeyDown()`` and ``onKeyMultiple()``.Both the ``Activity`` and ``View`` class implement the ``KeyEvent.Callback`` interface, so you should generally override the callback methods in your extension of these classes as appropriate.To handle an individual key press, implement ``onKeyDown()`` or ``onKeyUp()`` as appropriate. Usually, you should use ``onKeyUp()`` if you want to be sure that you receive only one event. If the user presses and holds the button, then ``onKeyDown()`` is called multiple times.

To respond to modifier key events such as when a key is combined with ``Shift`` or ``Control``, you can query the ``KeyEvent`` that's passed to the callback method. Several methods provide information about modifier keys such as ``getModifiers()`` and ``getMetaState()``. However, the simplest solution is to check whether the exact modifier key you care about is being pressed with methods such as ``isShiftPressed()`` and ``isCtrlPressed()``.

```java
@Override
public boolean onKeyUp(int keyCode, KeyEvent event) {
    switch (keyCode) {
        ...
        case KeyEvent.KEYCODE_J:
            if (event.isShiftPressed()) {
                fireLaser();
            } else {
                fireMachineGun();
            }
            return true;
        case KeyEvent.KEYCODE_K:
            if (event.isShiftPressed()) {
                fireSeekingMissle();
            } else {
                fireMissile();
            }
            return true;
        default:
            return super.onKeyUp(keyCode, event);
    }
}
```











