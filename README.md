# test



The problem : 

flutter: Impossible de lancer : Exception: Failed to launch phone: PlatformException(INVALID_ARGUMENT, Phone number cannot be null, null, null)
Reloaded 1 of 1949 libraries in 411ms (compile: 53 ms, reload: 181 ms, reassemble: 143 ms).
20
flutter: Impossible de lancer : Exception: Failed to launch phone: PlatformException(INVALID_ARGUMENT, Phone number cannot be null, null, null)

use of native code in flutter:

import 'package:flutter/services.dart';

class NativeBridge {
  static const platform = MethodChannel('com.example.myapp/launcher');

  static Future<void> callNativeLaunchUrl(String url) async {
    try {
      await platform.invokeMethod('launchURL', {'url': url});
    } on PlatformException catch (e) {
      throw Exception('Failed to launch URL: $e');
    }
  }

  static Future<void> callNativeLaunchPhone(String phonenumber) async {
    try {
      await platform.invokeMethod('launchPhone', {'phoneNumber': phonenumber});
    } on PlatformException catch (e) {
      throw Exception('Failed to launch phone: $e');
    }
  }

  static Future<void> callNativeLaunchEmail2(String email, String subject, String body) async {
    try {
        await platform.invokeMethod('launchEmail', {
            'email': email,
            'subject': subject,
            'body': body,
        });
    } on PlatformException catch (e) {
        throw Exception('Failed to launch email: $e');
    }
}


  static Future<void> callNativeLaunchWhatsApp(String phonenumber) async {
    try {
      await platform.invokeMethod('launchWhatsApp', {'phoneNumber': phonenumber});
    } on PlatformException catch (e) {
      throw Exception('Failed to launch WhatsApp: $e');
    }
  }

  static Future<void> callNativeLaunchEmail(String email) async {
    try {
      await platform.invokeMethod('launchEmail', {'email': email});
    } on PlatformException catch (e) {
      throw Exception('Failed to launch email: $e');
    }
  }

}


viewmodel that consumes it : 

import 'package:flutter/foundation.dart';
import 'package:tandeem_app/native/native_bridge.dart';
import 'dart:io' show Platform;

class HelpdeskViewModel extends ChangeNotifier {
  final String _email = 'support@tandeem.net';
  final String _phone = '+212636574208';
  final String _whatsapp = '+212636574208';

  String get email => _email;
  String get phone => _phone;
  String get whatsapp => _whatsapp;

  Future<void> launchPhone() async {
    try {
      await NativeBridge.callNativeLaunchPhone(_phone);
    } catch (e) {
      debugPrint('Impossible de lancer : $e');
    }
  }

  Future<void> launchEmail() async{
    const subject = "Bonjour";
    const body = "body";
    try {
      Platform.isAndroid ?
      await NativeBridge.callNativeLaunchEmail(_email)
      : await NativeBridge.callNativeLaunchEmail2(_phone,body,subject);
    } catch (e) {
      debugPrint('Impossible de lancer : $e');
    }
  }

  Future<void> launchWhatsApp() async{
    try {
      await NativeBridge.callNativeLaunchWhatsApp(_phone);
    } catch (e) {
      debugPrint('Impossible de lancer : $e');
    }
  }
}

