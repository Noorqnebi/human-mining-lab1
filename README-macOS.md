# Phase 1 — Local Browser Acceptance Package

Human Mining Lab الحالي + Local Trusted Oracle + Legacy HMC Private Testnet.
**هذه ليست Cosmos SDK. HMC-Test عملة Native تجريبية بلا قيمة مالية. التشغيل محلي فقط.**

## 1. المتطلبات على macOS

- جهاز Apple Silicon أو Intel، ومساحة مبدئية نحو 4 GB للاعتماديات والبناء والبيانات.
- Node.js 24 LTS من https://nodejs.org/en/download — يشمل npm.
- Go 1.27.1 أو أحدث، مثبت macOS المناسب لمعالجك من https://go.dev/dl/ . النسخة الدنيا مثبتة في `testnet/go.mod`.
- Xcode Command Line Tools إذا لم تكن موجودة: `xcode-select --install`؛ قد تتطلبها بعض الاعتماديات الأصلية.
- متصفح Chrome حديث أو Safari يدعم WebCrypto Ed25519. عند خطأ unsupported algorithm استخدم Chrome حديثًا؛ لا تعطل التشفير.
- اتصال إنترنت لأول `npm ci` وأول Go build لتنزيل الاعتماديات؛ لا تحتاج حساب Cloudflare أو استضافة أو Docker أو محفظة بأموال حقيقية.
- المنافذ المحلية 4173 و8788 و26656/26657 و26666/26667 و26676/26677 غير مستخدمة. لا تفتحها في الراوتر.

بعد تثبيت Node وGo أعد فتح Terminal. استخدم بنية معالج واحدة للاعتماديات؛ لا تنقل node_modules من جهاز آخر.

## 2. من الصفر إلى الواجهة

نفّذ في Terminal، بافتراض أن ZIP داخل Downloads:

```bash
cd ~/Downloads
unzip human-mining-lab-local-test.zip
cd human-mining-lab-local-test
npm ci
node testnet/scripts/start-local-lab.mjs --check
node testnet/scripts/start-local-lab.mjs
```

لا تحتاج نسخ `.env.example` للتشغيل الافتراضي. لا توجد فيه مفاتيح، ولا يتطلب المشغّل قيمًا سرية. أول تشغيل يبني `testnet/bin/hmcd` على macOS، وينشئ مفاتيح اختبار جديدة تلقائيًا ويبدأ ثلاثة validators وOracle والواجهة. قد يستغرق البناء عدة دقائق.

انتظر:

```text
READY — http://localhost:4173
```

افتح **http://localhost:4173** على الجهاز نفسه. لا تستخدم اسم الجهاز أو عنوان LAN أو رابط المعاينة القديم، لأن HTTP localhost هو سياق آمن لـWebCrypto. أبقِ Terminal مفتوحًا طوال الاختبار. لا تحتاج أي أمر آخر أثناء الجلسة.

المشغّل يتحقق من HTML والـOracle proxy ومن بلوكات العقد الثلاث قبل READY. هذه الجاهزية تعني أن الخدمات تستجيب؛ لا تعني نجاح اختبار الإنسان أو التعدين من المتصفح تلقائيًا.

## 3. أول Browser → Native Mint

1. افتح Lab. إن ظهرت شاشة الدخول المحلية استخدم التدفق المتاح؛ لا تحتاج حسابًا خارجيًا لربط الشبكة المحلية.
2. Wallet → HMC-Test On-Chain Wallet: أنشئ محفظة بكلمة مرور 12 محرفًا على الأقل، وصدّر النسخة الاحتياطية المشفرة، ثم Unlock. لا تشارك كلمة المرور.
3. Mine → HMC Private Testnet. إذا كانت خانة Testnet connection مخصصة من اختبار قديم، أعدها إلى الاتصال الافتراضي `/api/testnet`.
4. اختر 10 Minutes للاختبار الأول، وابدأ Start Mining. نفّذ التحديات نفسها، وابدأ كل تحدٍّ خلال 4 ثوانٍ. استمر بمتابعة التحديات المستمرة حتى تنتهي.
5. عند اكتمال الجلسة انتظر توليد الشهادة. اضغط Submit to HMC Testnet فور توفره؛ صلاحية الشهادة 180 ثانية، وتوليدها الأول مطلوب خلال 30 ثانية من نهاية الجلسة.
6. انتظر MINING REWARD CONFIRMED ON-CHAIN. راجع Reward وTransaction Hash وBlock وEpoch وWallet Balance.
7. View Transaction ثم Explorer لعرض البلوك والشهادة. اضغط Test replay protection: المتوقع رفض المطالبة نفسها.
8. نزّل Export session acceptance report. نفّذ النقل إلى عنوان محفظة اختبار ثانية إذا أردت اختبار transfer؛ Supply لا يزيد بسبب النقل.

الشبكة الجديدة تبدأ من صفر؛ أرصدة HMC Simulation لا تتحول إلى HMC-Test. لا تبحث عن معاملات البيئة السابقة على هذه الشبكة.

## 4. التوقف والاستئناف

Ctrl+C في Terminal المشغّل يطلب توقف الخدمات. انتظر رسالة التوقف قبل إغلاق Terminal. للاستئناف دون حذف البيانات:

```bash
node testnet/scripts/start-local-lab.mjs
```

أعد فتح نفس localhost ونفس ملف المتصفح، ثم Unlock. بيانات البلوكات والأرصدة في `testnet/.data`، والمحفظة المشفرة في تخزين المتصفح وملف النسخة الاحتياطية الذي صدّرته.

الجلسة النشطة عند توقف Oracle تصبح interrupted ولا تُمنح شهادة؛ ابدأ جلسة جديدة. الشهادات المكتملة تبقى محفوظة لكن صلاحيتها لا تتوقف عند إطفاء الجهاز. لا تستخدم reset لتجاوز خطأ تشغيل.

لإعادة شبكة الاختبار إلى بداية جديدة **عند رغبتك فقط**، أوقف المشغّل ثم:

```bash
bash testnet/scripts/reset-testnet.sh --confirm-testnet-reset
bash testnet/scripts/stop-testnet.sh
node testnet/scripts/start-local-lab.mjs
```

Reset يؤرشف بيانات الشبكة السابقة وينشئ مفاتيح جديدة. العملات القديمة لا تظهر في الشبكة الجديدة. ليس جزءًا من اختبار الاستمرارية.

## 5. ما داخل الحزمة وما المستبعد

المصدر الكامل للواجهة والتحديات، Oracle، تطبيق ABCI المدمج مع CometBFT، scripts، package.json/package-lock.json، go.mod/go.sum، الاختبارات، HMP/0.1 golden vectors، والوثائق. `PACKAGE-MANIFEST.json` يحتوي SHA-256 لكل ملف من المصدر داخل ZIP.

لا تحتوي node_modules أو build caches أو ملفات تنفيذية خاصة بمنصة أو بيانات الشبكة السابقة أو Git credentials أو كلمات مرور أو مفاتيح validator/oracle/admin أو محافظ المتصفح. الملف `.openai/hosting.json` معرف مشروع غير سري تحتاجه إعدادات المصدر؛ لا يمنح صلاحية نشر ولا ينشر المشغّل شيئًا.

لا نحتاج استمرارية السلسلة القديمة لهذا الاختبار، لذلك **لا توجد حزمة أسرار منفصلة**. عند التشغيل تُولَّد أسرار خاصة بهذا الجهاز داخل `testnet/.data`، ومنها `oracle.seed` و`admin.seed` ومفاتيح validator/node في node*/config. لا ترفع هذا المجلد أو ترسله. للاستمرارية محليًا احتفظ به كاملًا مع بيانات توقيع validators؛ لا تشغّل نسختين من بيانات توقيع validator نفسها معًا.

## 6. الاختبارات وإرسال النتائج

اختبارات دون تشغيل شبكة حقيقية (بعضها يشغّل Oracle مع RPC تجريبي):

```bash
node --test tests/raw-evidence.test.mjs tests/testnet-wallet.test.mjs tests/research-report.test.mjs tests/golden-certificate.test.mjs tests/oracle-restart.test.mjs tests/testnet-oracle.test.mjs
cd testnet
go test ./x/humanmining/...
cd ..
```

بعض اختبارات المصدر الأخرى تحتاج build أو شبكة؛ ليست كلها اختبارات مستقلة. دليل القبول المفصل في `testnet/LOCAL-ACCEPTANCE.ar.md`. لا تستبدل اختبار المتصفح باختبارات API.

أرسل `hmc-browser-acceptance.json` مع لقطة CONFIRMED ورفض replay، ورقم البلوك وhash المعاملة ونوع المتصفح. لا ترسل مفاتيح أو passphrase أو session token. لتجربة 1/2/3 نوافذ راجع Human multi-window trial، ثم أرسل `hmc-human-window-report.json` بعد تأكيد كل جلسة.

## 7. حل مشاكل البداية

- command not found: node/go: ثبّت الأداة وأعد فتح Terminal.
- Port is in use: أوقف نسخة Lab القديمة؛ لا توقف تطبيقات أخرى عشوائيًا.
- Node build/download failure: احتفظ برسالة الخطأ، تحقق من الإنترنت وGo وXcode tools. لا تغيّر go.mod لحجب الخطأ.
- Service not ready: راجع node*.log وoracle.log داخل testnet/.data محليًا؛ شارك رسالة الخطأ بعد حذف أي بيانات حساسة، وليس مجلد البيانات.
- WebCrypto unavailable: افتح localhost بالضبط وفي متصفح حديث. لا تخفض تشفير المحفظة.
- npm ci fails: أرسل أول خطأ فعلي مع Node/npm version ونوع Mac. لا تحذف lockfile.
- Sleep أثناء الجلسة: سيؤثر على VHT والتحديات. أبقِ الجهاز مستيقظًا واتصل بالطاقة.

## 8. حدود التحقق

`PACKAGE-VALIDATION.md` يسجل ما تم اختباره فعليًا عند إنتاج هذا الملف. لا توجد شهادة قبول macOS أو Human Browser Mint قبل تنفيذك للاختبار. Oracle موثوق ومركزي، والأدلة الخام قابلة للتصنيع آليًا؛ النظام ليس إثبات إنسان لامركزيًا ولا حلًا لـSybil. Cosmos SDK migration لم تبدأ في Phase 1.
