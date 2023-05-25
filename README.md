# Flutter Keyboard Visibility

React to keyboard visibility changes.

Note about Flutter Web support

Web support is an open issue here. Currently this library will just return false for keyboard visibility on web.

## Usage: React to Keyboard Visibility Changes

Option 1: Within your Widget tree using a builder

Build your Widget tree based on whether or not the keyboard is visible by using KeyboardVisibilityBuilder.

   ```
   import 'package:flutter_keyboard_visibility/flutter_keyboard_visibility.dart';

/// In any of your widgets...
@override
Widget build(BuildContext context) {
  return KeyboardVisibilityBuilder(
      builder: (context, isKeyboardVisible) {
        return Text(
          'The keyboard is: ${isKeyboardVisible ? 'VISIBLE' : 'NOT VISIBLE'}',
        );
      }
  );
   ```

Option 2: Within your Widget tree using a provider

Build your Widget tree based on whether or not the keyboard is visible by including a KeyboardVisibilityProvider near the top of your tree.

   ```
   import 'package:flutter_keyboard_visibility/flutter_keyboard_visibility.dart';

// Somewhere near the top of your tree...
@override
Widget build(BuildContext context) {
  return KeyboardVisibilityProvider(
    child: MyDemoPage(),
  );
}

// Within MyDemoPage...
@override
Widget build(BuildContext context) {
  final bool isKeyboardVisible = KeyboardVisibilityProvider.isKeyboardVisible(context);
  return Text(
    'The keyboard is: ${isKeyboardVisible ? 'VISIBLE' : 'NOT VISIBLE'}',
  );
}
   ```

Option 3: Direct query and subscription

Query and/or subscribe to keyboard visibility directly with the KeyboardVisibilityController class.

   ```
   import 'package:flutter_keyboard_visibility/flutter_keyboard_visibility.dart';
import 'dart:async';

late StreamSubscription<bool> keyboardSubscription;

@override
void initState() {
  super.initState();

  var keyboardVisibilityController = KeyboardVisibilityController();
  // Query
  print('Keyboard visibility direct query: ${keyboardVisibilityController.isVisible}');

  // Subscribe
  keyboardSubscription = keyboardVisibilityController.onChange.listen((bool visible) {
    print('Keyboard visibility update. Is visible: $visible');
  });
}

@override
void dispose() {
  keyboardSubscription.cancel();
  super.dispose();
}
   ```

## Usage: Dismiss keyboard on tap

Place a KeyboardDismissOnTap near the top of your Widget tree. When a user taps outside of the currently focused Widget, the Widget will drop focus and the keyboard will be dismissed.

   ```
   import 'package:flutter_keyboard_visibility/flutter_keyboard_visibility.dart';

// Somewhere near the top of your tree...
@override
Widget build(BuildContext context) {
  return KeyboardDismissOnTap(
    child: MyDemoPage(),
  );
}
   ```

By default KeyboardDismissOnTap will only dismiss taps not captured by other interactive Widgets, like buttons. If you would like to dismiss the keyboard for any tap, including taps on interactive Widgets, set dismissOnCapturedTaps to true.

   ```
   import 'package:flutter_keyboard_visibility/flutter_keyboard_visibility.dart';

// Somewhere near the top of your tree...
@override
Widget build(BuildContext context) {
  return KeyboardDismissOnTap(
    dismissOnCapturedTaps: true,
    child: MyDemoPage(),
  );
}
   ```

The IgnoreKeyboardDismiss Widget can be used to further refine which taps do and do not dismiss the keyboard. Checkout the example app for more detail.
