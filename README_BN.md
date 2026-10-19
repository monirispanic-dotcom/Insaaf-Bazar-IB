# Insaaf Bazar — Customer + Admin Web/Android Starter

এই প্রজেক্টটি আপনার `Insaaf_Bazar_Order_Form_No_Banner.zip` ফাইলের মূল দোকানের রঙ, বাংলা শিরোনাম, ক্যাটাগরি, পণ্যের কার্ড, কার্ট এবং অর্ডার ফর্মের ধারণা অনুসরণ করে নতুনভাবে সাজানো হয়েছে। মূল ZIP-এ স্ট্যাটিক demo পণ্য ও Netlify form ছিল; Firebase সংযোগ ছিল না। এই সংস্করণে Firebase Firestore-এ পণ্য ও অর্ডার রাখার কোড যোগ করা হয়েছে।

## যা আছে
- কাস্টমার দোকান: পণ্য দেখা/সার্চ/ক্যাটাগরি, কার্ট, COD অর্ডার।
- অ্যাডমিন: নির্দিষ্ট ইমেইলে লগইন, পণ্য যোগ/সম্পাদনা/মুছুন, অর্ডার দেখা ও স্ট্যাটাস বদল।
- একই React অ্যাপ web ও Capacitor Android APK-এর ভিত্তি হিসেবে ব্যবহার করা যায়। অ্যাডমিন আলাদা অ্যাপের মতো `#admin` রুটে খোলে; আলাদা APK বানাতে চাইলে Android build-এর আগে আলাদা entry/branding সেট করতে হবে।
- Firebase Firestore rules ফাইল অন্তর্ভুক্ত।

## 1. Firebase সেটআপ
1. https://console.firebase.google.com/ এ নতুন project তৈরি করুন।
2. Project settings → General → Your apps → Web app তৈরি করে Firebase config নিন।
3. Authentication → Sign-in method-এ **Email/Password** চালু করুন।
4. Authentication → Users থেকে `monirispanic@gmail.com`-এর জন্য অ্যাকাউন্ট তৈরি করুন। শক্তিশালী পাসওয়ার্ড দিন এবং ইমেইল verify করুন।
5. Firestore Database তৈরি করুন। Project-এর `.env.example` কপি করে `.env` বানান এবং Firebase config-এর মান বসান। `VITE_ADMIN_EMAIL=monirispanic@gmail.com` রাখুন।
6. Firebase Console → Firestore → Rules-এ `firestore.rules`-এর বিষয়বস্তু প্রকাশ করুন।
7. প্রথমবার Admin-এ ঢুকে পণ্য যোগ করুন। ডেমো পণ্য কেবল fallback; Firestore-এ অন্তত একটি পণ্য যোগ করলে সেটিই দেখাবে।

## 2. কম্পিউটারে চালানো
Node.js LTS ইনস্টল করে project folder-এ terminal খুলুন:
```bash
npm install
cp .env.example .env
# .env ফাইল সম্পাদনা করুন
npm run dev
```
Web build:
```bash
npm run build
npm run preview
```

## 3. Web hosting
Firebase CLI ইনস্টল/লগইন করুন, এরপর:
```bash
npm install -g firebase-tools
firebase login
firebase use --add
firebase deploy --only firestore:rules,hosting
```
Netlify-তেও `npm run build` build command এবং `dist` publish directory ব্যবহার করা যাবে; `.env`-এর মান Netlify site environment variables-এ যোগ করুন।

## 4. Android APK তৈরি
Android APK তৈরি করতে কম্পিউটারে Node.js, Android Studio ও Android SDK প্রয়োজন। Android Studio/Java setup সম্পন্ন করে:
```bash
npm install
npx cap add android
npm run android:sync
npm run android:open
```
Android Studio-তে Gradle sync শেষ হলে Build → Build Bundle(s) / APK(s) → Build APK(s)। APK সাধারণত `android/app/build/outputs/apk/debug/app-debug.apk`-এ পাওয়া যায়। পরিবর্তনের পরে `npm run android:sync` আবার চালান।

## নিরাপত্তা ও গুরুত্বপূর্ণ সীমাবদ্ধতা
- Admin-এর email client-side-এ দেখানো হলেও আসল অনুমতি `firestore.rules`-এ যাচাই করা হয়। Rules প্রকাশ না করলে নিরাপদ হবে না।
- Firebase config web app-এ প্রকাশ্য থাকতে পারে; নিরাপত্তা Firestore Rules দিয়ে enforce করতে হবে। কখনো service-account private key অ্যাপে রাখবেন না।
- এই starter-এ পেমেন্ট গেটওয়ে, কুরিয়ার API, OTP, কাস্টমারের order tracking/login, স্বয়ংক্রিয় স্টক কমানো, push notification, image upload storage বা production-grade anti-fraud নেই। বাস্তব ব্যবহারের আগে পরীক্ষা প্রয়োজন।
- এই চ্যাট থেকে আপনার Firebase project-এ লগইন করে deploy বা APK compile করা হয়নি; আপনার নিজস্ব Firebase credentials এবং Android build environment লাগবে।
