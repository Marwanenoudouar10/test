result :

class HelpdeskViewModel extends ChangeNotifier {
  // Use consistent naming (remove asterisks)
  final String email = 'support@tandeem.net';
  final String phone = '+212636574208';  // Changed from *phone to phone
  final String whatsapp = '+212636574208';

  Future launchPhone() async {
    try {
      // Add debug print
      debugPrint('Attempting to launch phone with number: $phone');
      
      await NativeBridge.callNativeLaunchPhone(phone);  // Changed from _phone to phone
    } catch (e) {
      debugPrint('Impossible de lancer : $e');
    }
  }

  Future launchEmail() async {
    const subject = "Bonjour";
    const body = "body";
    try {
      if (Platform.isAndroid) {
        await NativeBridge.callNativeLaunchEmail(email);  // Changed from _email to email
      } else {
        await NativeBridge.callNativeLaunchEmail2(
          email,  // Changed from _phone to email
          subject,
          body
        );
      }
    } catch (e) {
      debugPrint('Impossible de lancer : $e');
    }
  }

  Future launchWhatsApp() async {
    try {
      await NativeBridge.callNativeLaunchWhatsApp(whatsapp);  // Changed from _phone to whatsapp
    } catch (e) {
      debugPrint('Impossible de lancer : $e');
    }
  }
}

natvie bridg;

static Future callNativeLaunchPhone(String phoneNumber) async {
    try {
      debugPrint('NativeBridge: Attempting to launch phone with number: $phoneNumber');
      
      // Validate phone number
      if (phoneNumber.isEmpty) {
        throw PlatformException(
          code: 'INVALID_ARGUMENT',
          message: 'Phone number cannot be empty'
        );
      }

      await platform.invokeMethod('launchPhone', {
        'phoneNumber': phoneNumber
      });
      
      debugPrint('NativeBridge: Phone launch successful');
    } on PlatformException catch (e) {
      debugPrint('NativeBridge: Platform Exception - ${e.message}');
      throw Exception('Failed to launch phone: $e');
    } catch (e) {
      debugPrint('NativeBridge: General Exception - $e');
      throw Exception('Failed to launch phone: $e');
    }
  }
