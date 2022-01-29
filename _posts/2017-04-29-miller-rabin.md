---
title: "মিলার রবিন প্রাইমালিটি টেস্ট"
categories:
  - Algorithms
tags:
  - algorithms
  - problem-solving
  - number-theory
  - contest
---

মৌলিক সংখ্যা বের করার অনেকগুলো উপায় আমরা জানি। খুব বেশি একটা উপায় না জানলেও ইরাটোস্থ্যানেস এর  সিভ (Sieve of Eratosthenes) – এই অ্যালগোরিদমটি প্রায় সবাই জানি। কিন্তু বড় যেকোনো সংখ্যার ক্ষেত্রে সিভ মেথডটা অনেক বেশি সময় নেয়। তাই সিভ এর পাশাপাশি যদি আরো কয়েকটি অ্যালগোরিদম আমাদের জানা থাকে তাহলে প্রাইম নাম্বার বের করা নিয়ে আর সমস্যা হবে না।

আমরা এখন মৌলিক সংখ্যা বের করার যে অ্যালগোরিদমটি দেখব সেটি হল- “মিলার-রবিন প্রাইমালিটি টেস্ট (Miller-Rabin Primality Test)”।
মিলার-রবিন অ্যালগোরিদম ভালোভাবে বুঝতে গেলে আমাদের ফার্মাটের লিটল থিওরেম ভালোভাবে বুঝতে হবে। ফার্মাটের লিটল থিওরেম এপ্লাই করেও আমরা মৌলিক সংখ্যা বের করতে পারি। কিন্তু ফার্মাটের থিওরেমের কিছু সীমাবদ্ধতা আছে যার ফলে সব সংখ্যার জন্য নির্ভুল মান পাওয়া যায় না।
আমরা প্রথমে ফার্মাটের লিটল থিওরেম দেখব, তারপর এর সীমাবদ্ধতা দেখব এবং সবশেষে ফার্মাটের লিটল থিওরেম ব্যবহার করে কিভাবে মিলার-রবিন প্রাইমালিটি টেস্ট কাজ করে তা দেখব।

ফার্মাটের লিটল থিওরেমঃ
ফার্মাটের লিটল থিওরেম অনুযায়ী, যদি n একটি মৌলিক সংখ্যা হয় এবং a যদি n এর চেয়ে ছোট যেকোন ধনাত্মক পূর্ণসংখ্যা  হয় (1 <= a < n ) তাহলে ,
<center>
a<sup>n</sup> ≡ a (mod n)
</center>
<center>
বা, a<sup>n – 1</sup> ≡ 1 (mod n) ………. (i)
</center>

অর্থ্যাৎ, n এর চেয়ে ছোট a এর যেকোনো ধনাত্মক মানের জন্য যদি (i) নং সমীকরণ সত্য না হয় তাহলে n সংখ্যাটি মৌলিক নয় এবং যদি সত্য হয় তাহলে, n সংখ্যাটি মৌলিক সংখ্যা।

উদাহরণ দিয়ে বুঝা যাক –

ধরি, n = 4, এবং a = 2, তাহলে, (i)নং – এ মান বসিয়ে ,
<center>
=> 23 (mod 4) = 0
</center>
<center>
অর্থ্যাৎ, 4 সংখ্যাটি প্রাইম নয়।
</center>

আবার, ধরি , n = 7 এবং a = 5; তাহলে, (i)নং – এ মান বসিয়ে ,

<center>
=> 56 (mod 7) = 1
</center>
<center>
অর্থ্যাৎ, 7 সংখ্যাটি প্রাইম।
</center>
এভাবে, ফার্মাটের থিওরেম ব্যবহার করে, প্রাইম নাম্বার বের করা যায়।


কিন্তু এর কিছু সীমাবদ্ধতা আছে। যেমন-

ধরি, n = 15 এবং a = 4; তাহলে, (i)নং – এ মান বসিয়ে ,
<center>
=> 414 (mod 15) = 1
</center>
অর্থ্যাৎ, 15 সংখ্যাটি প্রাইম।কিন্তু 15 সংখ্যাটি কি মৌলিক সংখ্যা? না, 15 মৌলিক সংখ্যা নয়, কিন্তু ফার্মাটের থিওরেম অনুযায়ী, 15 সংখ্যাটিকে প্রাইম নাম্বার দেখাচ্ছে।
ফার্মাটের সাহায্যে প্রাইম নাম্বার বের করাটা মূলত একটি প্রোবাবিলিস্টিক প্রসেস। তাই এটি শুধুমাত্র একটি সংখ্যার মৌলিক হওয়ার সম্ভাব্যতা নির্দেশ করে। এখন, দেখা যাক, ফার্মাটের থিওরেম ব্যবহার করে কিভাবে আরেকটু নির্ভুলভাবে প্রাইম নাম্বার বের করা যায়।

এখন পর্যন্ত ফার্মাটের থিওরেম ব্যবহার করে শুধুমাত্র a- এর একটি মানের জন্য n সংখ্যাটি প্রাইম কি না তা নির্ধারণ করেছিলাম। কিন্তু এখন একটু অন্য পদ্ধতি এপ্লাই করা যাক। এবার  আমরা a – এর কয়েকটি মানের জন্য n -এর প্রাইমালিটি টেস্ট করব।

যেমন-
ধরি, a-এর কয়েকটি মান = k

এখন, যদি, k = 3 হয়, তাহলে a এর তিনটি মানের জন্য আমরা n – এর প্রাইমালিটি টেস্ট করব। যদি a-এর তিনটি মানের জন্যই ফার্মাটের থিওরেম সত্য হয় তাহলে আমরা সংখ্যাটি প্রাইম বলব। 

যদি তিনটি মানের মধ্যে একটির জন্যও ফার্মাটের থিওরেম সত্য না হয় তাহলে সংখ্যাটিকে নট-প্রাইম বলে ঘোষণা করব। একটি উদাহরণ দেখা যাক –

ধরি, n = 15, a = 4 , 3 , 2; তাহলে,
<center>
১। a = 4 এর জন্য,
<br>
=> 414 (mod 15) = 1; 
</center>
অর্থ্যাৎ, a = 4 এর জন্য 15 সংখ্যাটি প্রাইম।
<center>
২। a = 3 এর জন্য,
<br>
=> 314 (mod 15) ≠ 1; 
</center>
অর্থ্যাৎ, a = 3 এর জন্য 15 সংখ্যাটি প্রাইম নয়।

এতটুকু পর্যন্ত আসার পর আমরা বুঝে গেছি যে, 15 সংখ্যাটি প্রাইম নয়। a = 2 এর জন্য চেক না করলেও চলবে। কারণ, n তখনই প্রাইম হবে যখন a – এর k সংখ্যক মানের জন্যই ফার্মাটের থিওরেম সত্য হবে। 

আচ্ছা, এইখানে যেকোনো n – এর জন্য a – এর মান কত থেকে কত পর্যন্ত হতে পারে বলুন তো? 1 থেকে  n – 1 পর্যন্ত ? নাকি 2 থেকে n – 2 পর্যন্ত? একটু চিন্তা করে বের করে ফেলতে হবে কিন্তু !!

এতক্ষণ পর্যন্ত আসার পর আমরা এখন ফার্মাটের লিটল থিওরেম ব্যবহার করে বেশ নির্ভুলভাবেই( সম্পূর্ণ নির্ভুলভাবে নয় কিন্তু 😉 )  প্রাইম নাম্বার বের করে ফেলতে পারব। কিন্তু n = 65, 91, 703, 1729, 1921, 2821, 5611, 8911- এই কয়টি সংখ্যার জন্য ফার্মাটের থিওরেম সবসময়ই ব্যর্থ হবে। এই কয়টি সংখ্যা কম্পোজিট বা মৌলিক সংখ্যা না হওয়া সত্ত্বেও ফার্মাটের থিওরেম এই সংখ্যাগুলোকে মৌলিক বলে দাবি করবে। আর এই সমস্যা থেকে বের হয়ে আসার জন্যই আমরা মিলার-রবিন প্রাইমালিটি টেস্টের সাহায্য নেব।


ফার্মাটের থিওরেম এর মত , মিলার-রবিন প্রাইমালিটি টেস্ট ও একটি প্রোবাবিলিস্টিক এ্যালগোরিদম। কিন্তু প্রাইম নাম্বার বের করা ক্ষেত্রে এর error rate ফার্মাট অপেক্ষা কম।  তবে এখন দেখে ফেলা যাক, ফার্মাট থিওরেমের কাঁধের উপর বসে মিলার-রবিন এর সাহায্য নিয়ে কিভাবে নির্ভুলভাবে প্রাইম নাম্বার বের করা যায়।
<br>
<br>
### মিলার-রবিন টেস্টঃ

ফার্মাটের থিওরেমটা আবার মনে করা যাক –
a<sup>n – 1</sup> ≡ 1 (mod n)
এখানে, n হিসেবে আমরা শুধু বেজোড় সংখ্যাগুলোকেই  কাউন্ট করব। কারণ, আমরা জানি, 2 ছাড়া আর কোন জোড় সংখ্যাই মৌলিক নয়। মিলার-রবিন অ্যালগোরিদম এর মেইন আইডিয়া হল- (n – 1) -কে 2s.d – আকারে লিখব ;যেখানে d = বেজোড় সংখ্যা।

অর্থ্যাৎ, (n – 1) = (2 এর পাওয়ার * যেকোনো বেজোড় সংখ্যা)।
<center>
(n – 1) = 2<sup>s</sup>.d ....…..(ii)
</center>
(ii)নং সত্য হলে,
<center>
a<sup>d</sup> ≡ 1 (mod n) ……. (iii)
</center>
যেকোনো i ∈ { 0 , … , s−1} এর জন্য a<sup>2<sup>i</sup></sup>.d ≡ -1(mod n) সত্য হবে। ……(iv)

(iii) অথবা (iv) এর যেকোনো একটি সত্য হবে।

#### প্রমাণঃ 
<center>
a<sup>n – 1</sup> ≡ a<sup>2<sup>s</sup></sup>.d
<br>
=> a<sup>n – 1</sup> ≡ a<sup>2. 2<sup>s – 1</sup>.d</sup>
<br>
=> a<sup>n – 1</sup> ≡ (a<sup>2<sup>s – 1</sup>.d</sup>)<sup>2</sup>
</center>
অতএব, ফার্মাটের থিওরেম অনুযায়ী,
<center>
(a<sup>2<sup>s – 1</sup>.d</sup>)<sup>2</sup> ≡ 1 (mod n)
<br>
(a<sup>2<sup>s – 1</sup>.d</sup>)  ≡ ±1 (mod n)
</center>
আমরা একটি উদাহরণ দিয়ে দেখব-
<center>
ধরি, n = 221,
এখন, n – 1 = 2<sup>s</sup>.d
<br>
=> 220 = 2<sup>2</sup>.55
<br>
এখানে , s = 2 এবং d = 55।
<br>
</center>
a = 174 এর জন্য –
<center >
a<sup>2<sup>s – 1</sup>.d</sup> = 174<sup>2<sup>2-1</sup>.55</sup>
<br>
= 174<sup>2<sup>1</sup>.55</sup>  
<br>
= 174110 (mod 221)
<br>
= 220 = n – 1
</center>

অতএব, a = 174 এর জন্য n = 221 সংখ্যাটি প্রাইম।
<br>
এবার, a = 137 এর জন্য-
<center>
a<sup>2<sup>s – 1</sup>.d</sup> = 174<sup>2<sup>2-1</sup>.55</sup>
<br>
= 137<sup>2<sup>1</sup>.55</sup>  
<br>
= 137110 (mod 221)
<br>
= 188 ≠ n – 1
<br>
যেহেতু, উত্তরটি n – 1 এর সমান হয় নি, সেহেতু এখন আমরা a<sup>2<sup>s-2</sup>.d</sup> এর মান বের করব।
<br>
a<sup>2<sup>s – 2</sup>.d</sup> = 137<sup>2<sup>2-2</sup>.55</sup>
<br>
= 137<sup>2<sup>0</sup>.55</sup>               
<br>
= 13755 (mod 221)
<br>
= 205 ≠ n – 1
</center>
অর্থ্যাৎ, সংখ্যাটি মৌলিক নয়। কারণ সংখ্যাটি মৌলিক হতে হলে iii অথবা iv এর মধ্যে যেকোনো একটিকে সত্য হতে হবে।

যদি (iii) এবং (iv)  এর মধ্যে কোনটিই সত্য না হয় তাহলে সংখ্যাটি মৌলিক নয়।

আশা করি, এখন সূডোকোড দেখলে সবটুকু ক্লিয়ার হয়ে যাবে এবং নিজে নিজেই ইমপ্লিমেন্ট করা যাবে। 🙂

সূডোকোডঃ
![millerrabin-pseudocode](../assets/images/blog/millerrabin.png)
<center>Source: Wikipedia</center>

পরবর্তী অংশে এটার ইমপ্লিমেন্টেশন ও কমপ্লেক্সিটি নিয়ে লিখার চেষ্টা করব, যদিও নিজে একটু পড়ালেখা করলেই এর ইমপ্লিমেন্টেশন ও কমপ্লেক্সিটি বুঝে ফেলা সম্ভব। কিছু লিংক দিয়ে দিচ্ছি, চাইলে আরও ডিটেইলে পড়ে নিতে পারেন।


Additional Resource Links: 

https://en.wikipedia.org/wiki/Fermat%27s_little_theorem
https://en.wikipedia.org/wiki/Miller%E2%80%93Rabin_primality_test
https://comeoncodeon.wordpress.com/2010/09/18/miller-rabin-primality-test/
https://miller-rabin.appspot.com/

প্র্যাকটিস এর জন্য প্রবলেমঃ
[SPOJ-Prime or Not](https://www.spoj.com/problems/PON/)