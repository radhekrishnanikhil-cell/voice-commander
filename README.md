# Voice Commander App — Setup Guide (Hinglish)

## Ye app kya karta hai
1. **Enroll Voice** button se aap apni voice ek baar record karwate ho (3 takes).
2. **Listen** button dabane par app pehle check karta hai ki bol kaun raha hai (aapki voice ya kisi aur ki).
3. Agar voice match ho jaaye, tabhi app command sunega aur execute karega:
   - "open youtube" → YouTube khul jaayega
   - "open whatsapp" → WhatsApp khul jaayega
   - "call Rahul" → Contacts mein "Rahul" dhoondh kar call karega
   - "whatsapp call Rahul" → Rahul ke saath WhatsApp chat khol dega (call button khud dabana hoga — WhatsApp calls ko seedha trigger karne ka koi public Android API nahi hai)

## ⚠️ Zaroori Limitation (seedhe seedhe)
Is app ka voice-match 100% secure biometric lock nahi hai. Ye simple audio-feature comparison use karta hai (energy, pitch jaisi cheezein). Ye:
- Casual use ke liye theek hai (koi random dusra banda bol kar command na chala paaye)
- Lekin kisi determined insaan ko ya recorded voice ko fool-proof block nahi karega
- Agar bahut strict security chahiye to asli solution ek proper ML speaker-verification model hoga (jyada complex, extra setup chahiye)

Agar match kabhi fail ho (aapki apni voice reject ho rahi hai) ya bahut easily pass ho raha hai, to `VoiceAuth.kt` file mein ye line dhoondhiye:
```kotlin
const val MATCH_THRESHOLD = 0.82
```
Isko thoda kam (e.g. 0.75) karein agar aapki voice reject ho rahi hai, ya thoda zyada (e.g. 0.88) karein agar dusri voice bhi pass ho rahi hai.

## Install Kaise Karein (Step by Step)

### Cheezein chahiye honge:
1. Windows/Mac/Linux computer
2. **Android Studio** (free): https://developer.android.com/studio se download karein

### Steps:
1. Android Studio install karein aur open karein
2. "Open" par click karein, aur ye poora `VoiceCommanderApp` folder select karein
3. Android Studio thoda time lega "Gradle Sync" karne mein (internet chahiye hoga — pehli baar dependencies download hongi)
4. Apna phone USB cable se computer se connect karein:
   - Phone mein Settings → About Phone → "Build Number" par 7 baar tap karein (Developer Mode on hoga)
   - Settings → Developer Options → "USB Debugging" ON karein
5. Android Studio mein upar dropdown mein apna phone select karein
6. Green "Run" (▶) button dabayein
7. App apne aap phone mein install ho jaayega

### Agar USB se install nahi karna:
Android Studio ke menu se: `Build → Build Bundle(s) / APK(s) → Build APK(s)`.
Isse ek `.apk` file banegi jo aap WhatsApp/email/Drive se apne phone bhej kar directly install kar sakte hain (phone mein "unknown apps install" allow karna padega).

## Pehli baar app kholne par
1. App permissions maangega (Microphone, Call, Contacts) — sab "Allow" karein
2. "Step 1: Apni Voice Enroll Karein" dabayein, 3 baar bola gaya phrase bolein
3. Uske baad "Step 2: Voice Command Bolein" se commands test karein

## Aage Customize Kar Sakte Hain
- `CommandExecutor.kt` mein naye commands add kar sakte hain (jaise "open instagram", "open camera", etc.) — same pattern follow karein jo YouTube/WhatsApp ke liye use hua hai
- Hindi mein command bolwana hai to `MainActivity.kt` mein ye line change karein:
  ```kotlin
  putExtra(RecognizerIntent.EXTRA_LANGUAGE, "en-IN")
  ```
  isko `"hi-IN"` kar dein
